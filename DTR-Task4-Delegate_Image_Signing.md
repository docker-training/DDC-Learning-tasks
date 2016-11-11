# Task 3 - Delegate Image Signing

In this task you will complete the following steps:
1. 

## Pre-requisites
- DTR installed and integrated with UCP
- All Docker Engines configured to trust DTR certificate. 
- Must have completed all user management exercises in order to have the necessary user accounts setup in DTR
- Must have completed Task 3 in this module


## Step 1 - Rotate Snapshot key

It's not practical to have to sign every image yourself. Hence the signing of images can be delegated to other users. 

Before we can delegate image signing to other users, the repository **snapshot** key needs to be managed by the Notary server. Currently the key is managed by the 
Notary client and exists in the notary `trust_dir` directory (./docker/trust). What we need to do is `rotate` this key so that it is now managed by the Notary server.
This is so we don't have to distribute this key to every other user who needs to sign images against the repo.  

1. Rotate the repository **snapshot** key so that it is managed by the Notary server.

   `$ notary key rotate <dtr_url>/engineering/core-app snapshot --server-managed`
   
   ```
   ubuntu@notary-client:~$ notary key rotate ec2-54-213-179-82.us-west-2.compute.amazonaws.com/engineering/core-app snapshot --server-managed
   Enter username: johnfull
   Enter password:
   Enter passphrase for root key with ID ae433e3:
   Successfully rotated snapshot key for repository ec2-54-213-179-82.us-west-2.compute.amazonaws.com/engineering/core-app
   ```
   
## Step 2 - Get the cert.pem file and delegate image signing.   

For each user that you want to delegate image signing to, you need to get their `**cert.pem**` file. This is the cert.pem file that comes with each user's client bundle.
   
For the purposes of this exercise, we will delegate our image signing to the user **Chloe**.

1. Login to the UCP web UI as **Chloe**.
2. Download the client bundle to your local machine and extract the zipped folder
3. Upload the `cert.pem` file in the extracted folder to the **eng-local** VM. 

   You can use `scp` or any other file transfer tool to upload the file.

4. Run the following command to create a new Notary delegation role, using the user certificate:

   `$ notary delegation add -p <dtr_url>/<account>/<repository> targets/releases --all-paths cert.pem`  
   
   When prompted for authentication details, specify the credentials of the **johnfull** user.

   ```
   ubuntu@notary-client:~$ notary delegation add -p ec2-54-213-179-82.us-west-2.compute.amazonaws.com/engineering/core-app targets/releases --all-paths cert.pem

   Addition of delegation role targets/releases with keys [b010041f7677ba692c9a8cbf8cbab640ef06b3e5392c7138c385c964fd69cd76], with paths ["" <all paths>], to repository "ec2-54-213-179-82.us-west-2.compute.amazonaws.com/engineering/core-app" staged for next publish.

   Auto-publishing changes to ec2-54-213-179-82.us-west-2.compute.amazonaws.com/engineering/core-app
   Enter username: johnfull
   Enter password:
   Enter passphrase for targets key with ID aba7206:
   Successfully published changes for repository ec2-54-213-179-82.us-west-2.compute.amazonaws.com/engineering/core-app
   ```
   

## Step 3 - Push a signed image

For this step, we will pretend that we are the user Chloe. 

1. SSH into the **dev-local** VM 
2. Upload Chloe's client bundle into that node. 
3. Go into the client bundle folder
4. In the client bundle folder. Run the command:

   $ notary key import key.pem

   ```
   ubuntu@notary-client:~$ cd ucp-bundle-chloe/
   ubuntu@notary-client:~/ucp-bundle-chloe$ notary key import key.pem
   Enter passphrase for new delegation key with ID b010041 (tuf_keys):
   Repeat passphrase for new delegation key with ID b010041 (tuf_keys):
   ```
   
   This is needed because before delegation role users can publish signed content with Notary or Docker Content Trust, 
   they must import the private key associated with the user certificate:

5. Pull the `hello-world` image into the node

   `$ docker pull hello-world`
   
6. Re-tag the `hello-world` image with the `core-app` repository and use a 2.0 tag.

   `$ docker tag hello-world ec2-54-213-179-82.us-west-2.compute.amazonaws.com/engineering/core-app:2.0`
   
7. Enable content trust

   `export DOCKER_CONTENT_TRUST=1`
   
8. Login to DTR on the Docker CLI as Chloe
9. Push the `core-app` repo

   ```
   ubuntu@notary-client:~$ docker push ec2-54-213-179-82.us-west-2.compute.amazonaws.com/engineering/core-app:2.0
   The push refers to a repository [ec2-54-213-179-82.us-west-2.compute.amazonaws.com/engineering/core-app]
   a02596fdd012: Layer already exists
   2.0: digest: sha256:a18ed77532f6d6781500db650194e0f9396ba5f05f8b50d4046b294ae5f83aa4 size: 524
   Signing and pushing trust metadata
   Enter passphrase for delegation key with ID b010041:
   Successfully signed "ec2-54-213-179-82.us-west-2.compute.amazonaws.com/engineering/core-app":2.0
   ```

10. Go to the core-app repository on your web browser and check for the 2.0 tag.    
   