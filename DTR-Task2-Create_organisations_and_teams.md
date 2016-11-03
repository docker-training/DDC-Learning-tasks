# Task 2 - Create Organizations and Teams

In this task you will complete the following steps:
1. 


## Pre-requisites
- DTR installed and integrated with UCP
- All Docker Engines configured to trust DTR certificate. 
- Docker Toolbox OR Docker for Mac / Windows installed on local machine
- Must have completed all user management exercises in order to have the necessary user accounts setup in DTR

## Step 1 - Create Organization and Team

1. Create an organization called **"engineering"**
   
   First, click the **Organizations** link on the left navigation of the page.
   
   <img src="/images/DEOPS-DTR-T2_left_nav.PNG" width="25%"/>
   
   Then click on the **New Organization** button on the top right side. Specify the organization name and click the **save** button.
   
2. Click on the **engineering** organization and then click the **+** symbal to create a new team.
   
   <img src="/images/DEOPS-DTR-T2_createteam.PNG" width="25%"/>
   
3. Specify **web** as the team name and click **save**
4. Repeat the process to create another team called **database**

## Step 2 - Add members to teams

1. Click on the **database** team and then click the **Add user** button on the top right corner
2. Add the user **Chloe** to the team.

   ![](/images/DEOPS-DTR-T2_addusers.PNG)

   Select her name and then click the **save** button
3. Now select the **web** team and add the user **barryview** to the team.
4. Click on the **Org Members** menu item on the left. You should see three members
   * Admin
   * barryview
   * chloe 
5. Click **Add user** on the top right 
6. Add the user **johnfull** to the organization and click **save**

   Users can belong to a organization but do not have to be on a team. Users who are on a team are automatically part of the organization
   that the team belongs to.
   
## Step 3 - Test a repository

1. Create a public repository under the **engineering** account called **sample-app**

   ![](/images/DEOPS-DTR-T2_create_eng_repo.PNG)

2. Open a terminal on your local machine
3. Tag the `hello-world` repository with the new Repo you just created
   
   ```
   johnny@LAPTOP-CT7E41JV MINGW64 ~
   $ docker tag hello-world ec2-54-213-179-82.us-west-2.compute.amazonaws.com/engineering/sample-app:1.0
   ```
   
4. Login to DTR using the user account **chloe**

   ```
   johnny@LAPTOP-CT7E41JV MINGW64 ~
   $ docker login ec2-54-213-179-82.us-west-2.compute.amazonaws.com
   Username: chloe
   Password:
   Login Succeeded
   ```
   
5. Push the image to DTR

   ```
   johnny@LAPTOP-CT7E41JV MINGW64 ~
   $ docker push ec2-54-213-179-82.us-west-2.compute.amazonaws.com/engineering/sample-app:1.0
   The push refers to a repository [ec2-54-213-179-82.us-west-2.compute.amazonaws.com/engineering/sample-app]
   a02596fdd012: Preparing
   unauthorized: authentication required
   ```
   
   We get the authentication required error because we do not yet have the permission to push images into the `engineering/sample-app` repository.
   We need to configure our organization and teams to have access to this repo.
   
   
## Step 4 - Configure permissions

1. Go into the **engineering** organization and click on the **database** team
2. Click **Add Repository** on the top right corner
3. Select the existing **sample-app** repository and select the **Read-write** permission (as shown below)

   ![](/images/DEOPS-DTR-T2_add_repo.PNG)
   
4. Repeat the procedure for the **web** team but select the **Read only** permission
5. Click on the **Repositories** link on the left navigation and then click on your `engineering/sample-app` repo.
6. Click on the **permissions** tab of the repo and verify that you can see the following:

   ![](/images/DEOPS-DTR-T2_repo_permissions.PNG)
   

  


   
   
   
