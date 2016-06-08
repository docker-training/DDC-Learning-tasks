# Task 3 - Advanced User Management

In this Task you will complete create new users, teams and resource labels. You will then deploy a web app and test access to the web apps resources. In order to do this you will complete the following steps:

1. Create additional users and teams
2. Deploy a Compose application
3. Check access

## Pre-requisites

- Must have completed all steps in Task 1 and Task 2

## Step 1 - Create additional users and teams

In this step you will create a new user, a new team, and some new permissions labels. You will use these in the next step.

1. Make sure you are logged into UCP as the **admin** user.

2. Create an additional user with the following details.

  Be sure to make a note of the password you set, as you will need this later.

| Username   | Full Name       | Default Permissions  |
| :--------- | :-----------    | :------------------- |
| chloe      | Chloe Operate   | Full Control         |


2. Create a new team called **"Operations"** and make sure **TYPE** is "Managed".

3. Add user **Chloe** to the new **Operations** team.

4. With the **Operations** team selected, click on the **Permissions** tab and create the following **Labels**.

| Resource Label   |   Permission         |
| :-------------   |   :------------------|
| production       |   Full Control       |
| development      |   Full Control       |
| qa               |   Full Control       |

5. Switch over to the **Engineering** team and create the following **Labels**.

| Resource Label   |   Permission         |
| :-------------   |   :------------------|
| production       |   View Only          |
| development      |   Full Control       |
| qa               |   View Only          |


## Step 2 - Deploy a Compose application

In this step you will deploy a Dockerized web app and apply the "production" label to it.

1. Login to UCP as the user **chloe** and download her client bundle.

2. Unzip the client bundle into a folder of your choice.

3. Connect to UCP using the client bundle.

  If you need details on how to do this, refer back to Step 3 in Task 2.

4. Clone the https://github.com/johnny-tu/HelloRedis repository from withint the directory that contains Chloe's unzipped client bundle.

  ```
  git clone https://github.com/johnny-tu/HelloRedis
  ```

5. Add the **production** lable to both services in the `docker-compose.prod.yml` file.

   When completed, your `docker-compose.prod.yml` file should look like the following:

   ```
javaclient:
    image: trainingteam/hello-redis:1.0
    links:
    - redis:redisdb
    labels:
      com.docker.ucp.access.label: "production"
redis:
    image: redis
    labels:
      com.docker.ucp.access.label: "production"
   ```

6. Launch the application with the following command:

  ```
  $ docker-compose -f docker-compose.prod.yml up -d
  ```

  > This step may take a few minutes while images are pulled and the application is started.

  If you experience issues with this step, ensure that you have correctly set the environment variables using the script file supplied as part of Chloe's client bundle.

7. Click on the **Applications** link in the UCP Dashboard to see the **helloredis** application. Check both containers to make sure they have the "production" label.

8. Click the **helloredis** application and drill into each container to make sure it has the **com.docker.ucp.access.label: "production"** label.

9. Click the **Console** tab on one of the containers and run a bash shell to confirm that you can get terminal access.


## Step 3 - Check access

1. Login to UCP as **johnfull** from the **Engineering** team.

2. Click on the **Applications** link in the left pane and verify that you can see the application.

3. Drill into one of the containers and try and open a bash terminal from the **Console** tab.

  This operation will fail. This is because members of the **Engineering** team only have "View Only" access to resources tagged with the **production** label.