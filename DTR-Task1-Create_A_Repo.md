# Task 1 - Create a Repository

In this task, you will complete the following steps:
1. Create a public repository in Docker Trusted Registry using a non admin user account
2. Push an image from your local Docker client into the repository
3. Pull an image from the DTR repository into your local environment
4. Push an image into the same repository as a different user.

## Pre-requisites
- DTR installed and integrated with UCP
- All Docker Engines configured to trust DTR certificate. 
- Docker Toolbox OR Docker for Mac / Windows installed on local machine
- Must have completed all user management exercises in order to have the necessary user accounts setup in DTR

## Step 1 - Create a repository

1. Login to your Docker Trusted Registry web UI as the user `John Full` 
2. Create a New Repository called "johns-repo". Set the visibility to "public"

![CreateARepo](/images/DEOPS-DTR-T1_Create_a_public_repo.PNG)

## Step 2 - Push an image into the repository

**Note: In this step, make sure you DO NOT use the UCP client bundle**

1. Open a new terminal in your local machine
2. Pull the `alpine` image 

   ```
   johnny@LAPTOP-CT7E41JV MINGW64 ~/sandbox
   $ docker pull alpine
   Using default tag: latest
   latest: Pulling from library/alpine
   3690ec4760f9: Pull complete
   Digest: sha256:1354db23ff5478120c980eca1611a51c9f2b88b61f24283ee8200bf9a54f2e5c
   Status: Downloaded newer image for alpine:latest
   ```
3. Re-tag the image with your DTR repository name and a tag of 1.0. 
   In this example, our DTR instance is hosted at **ec2-54-213-179-82.us-west-2.compute.amazonaws.com**
   
   ```
   johnny@LAPTOP-CT7E41JV MINGW64 ~/sandbox
   $ docker tag alpine ec2-54-213-179-82.us-west-2.compute.amazonaws.com/johnfull/johns-repo:1.0
   ```
4. Push the image into DTR. What do you notice?

   ```
   johnny@LAPTOP-CT7E41JV MINGW64 ~/sandbox
   $ docker push ec2-54-213-179-82.us-west-2.compute.amazonaws.com/johnfull/johns-repo:1.0
   The push refers to a repository [ec2-54-213-179-82.us-west-2.compute.amazonaws.com/johnfull/johns-repo]
   011b303988d2: Preparing
   unauthorized: authentication required
   ```
   
   We get an **authentication required**  error because we have not logged into our DTR instance on the Docker CLI.

5. Login to DTR as **John Full* on your Docker CLI. 
   ```
   johnny@LAPTOP-CT7E41JV MINGW64 ~/sandbox
   $ docker login ec2-54-213-179-82.us-west-2.compute.amazonaws.com
   Username: johnfull
   Password:
   Login Succeeded
   ```
6. Push the image again. This time it should succeed. 
   ```
   johnny@LAPTOP-CT7E41JV MINGW64 ~/sandbox
   $ docker push ec2-54-213-179-82.us-west-2.compute.amazonaws.com/johnfull/johns-repo:1.0
   The push refers to a repository [ec2-54-213-179-82.us-west-2.compute.amazonaws.com/johnfull/johns-repo]
   011b303988d2: Pushed
   1.0: digest: sha256:1354db23ff5478120c980eca1611a51c9f2b88b61f24283ee8200bf9a54f2e5c size: 528
   ```
   
7. Go to the repository on the DTR web UI and verify that you now have a 1.0 tag on the repository.

   ![](/images/DEOPS-DTR-T1_image_tag.PNG)
   

## Step 3 - Pull an image from a DTR Repository

In this step, we will pull the image we have just uploaded to DTR, using a different machine and as a different user. 

**Make sure you do not use the UCP client Bundle in this step**

1. Open a new SSH terminal to your **ucp-controller** node. 
2. Pull the image uploaded in Step 2 from DTR.

   Notice how you did not have to login to pull the image. This is because our repository is public.
   
## Step 4 - Push a new version of the image into the DTR repository

1. Tag the image you pulled in Step 3 as version 1.1

   ```
   ubuntu@ucp-controller:~$ docker tag ec2-54-213-179-82.us-west-2.compute.amazonaws.com/johnfull/johns-repo:1.0 ec2-54-213-179-82.us-west-2.compute.amazonaws.com/johnfull/johns-repo:1.1
   ```

2. Login to DTR as the user **Barry View** using the Docker CLI

3. Push the image.

   Notice the **authentication required** error message. We are not able to push images into the repository because the repository was created by **John**. 
   Even though it is a public repository, only the repo creator and admin users an push to it. Other users must be granted additional 
   access in order to be able to push. This is done via the use of Organizations and Teams.
   
   
   
