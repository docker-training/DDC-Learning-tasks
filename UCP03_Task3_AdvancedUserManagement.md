# Task 3 - Advanced User Management

## Pre-requisites

- Must have completed all steps in Task 1 and Task 2

## Step 1 - Create additional users and teams

1. Make sure you are logged into UCP as the Admin user
2. Create an additional user with the following details:

| Username   | First Name   | Last Name  | Role               |
| :--------- | :----------- | :--------- | :----------------- |
| chloe      | Chloe        | Operate    | Full Control       |

   

2. Create another team called **Operations**. Select "Managed" as the team type
3. Add user **Chloe** to the team
4. Click on the **Permissions** tab and create the following **Labels**

| Resource Label   |   Permission         |
| :-------------   |   :------------------|
| production       |   Full Control       |
| development      |   Full Control       |
| qa               |   Full Control       |

5. Switch over to the Engineering team and create the following **Labels**

| Resource Label   |   Permission         |
| :-------------   |   :------------------|
| production       |   View Only          |
| development      |   Full Control       |
| qa               |   View Only          |


## Step 2 - Deploy Compose Application

1. Login to UCP as the user **Chloe** and download her client bundle. Unzip the client bundle into a folder of your choice.
2. Connect to UCP using the client bundle
3. Clone the https://github.com/johnny-tu/HelloRedis repository to a folder on your PC or Mac
4. Deploy the application using the `docker-compose.prod.yml` file and apply the `production` label on each container.

   To apply a label on Containers deployed via a Compose application, we must define this in our `docker-compose` file. Open the `docker-compose.prod.yml` 
   file and add the `labels` instruction to both services. Copy the example below;

   ```
javaclient:
    image: trainingteam/hello-redis:1.0
    labels:
      com.docker.ucp.access.label: "production"
redis:
    image: redis
    labels:
      com.docker.ucp.access.label: "production"
   ```
   
   Once you have modified the `docker-compose.prod.yml` file, launch the application with the following command:
   `$ docker-compose -f docker-compose.prod.yml` up -d`
   
5. Click on the **Applications** link in UCP and check for the HelloRedis Application. Check both containers to make sure they have the "production" label.
6. Check the **console** tab on one of the containers and confirm that you can get terminal access


## Step 3 - Check access

1. Login to UCP as someone from the **Development** team. i.e. John
2. Verify that you can see the application
3. Try and get terminal access to one of the HelloRedis containers. What do you observe and why do you think this is the case?
