# Task 4 - Deploy a simple application on UCP

## Deploy an application using Docker Compose

In this exercise we will deploy a simple multi container application. The application will be run using **Docker Compose** and contains 2 services (containers)
- A Redis Container
- A Java client that pings the container to get a response


1. SSH into your UCP controller AWS machine
2. Use Git to clone the application repository from https://github.com/johnny-tu/HelloRedis.git 

   ```bash
   ubuntu@ucptest-0:~$ git clone https://github.com/johnny-tu/HelloRedis.git
   Cloning into 'HelloRedis'...
   remote: Counting objects: 42, done.
   remote: Total 42 (delta 0), reused 0 (delta 0), pack-reused 42
   Unpacking objects: 100% (42/42), done.
   Checking connectivity... done.
   ```
3. You should now see a folder called `HelloRedis`. Change directory into this folder
4. List the files in the directory. You should see a `docker-compose.yml` file

   ```bash
   ubuntu@ucptest-0:~/HelloRedis$ ls
   docker-compose.yml  Dockerfile  lib  README.md  src
   ```
   
5. Run `docker-compose up -d`

   ```bash
   ubuntu@ucptest-0:~/HelloRedis$ docker-compose up -d
   Pulling redis (redis:latest)...
   ....
   ....
   58e12b50fee8: Already exists
   a1a961e320bc: Already exists
   Digest: sha256:de86bd14ab69c9b707fe5f3213f6e3c6f543df28bc05ae6cef7b61f2b12be343
   Status: Downloaded newer image for redis:latest
   Creating helloredis_redis_1
   Building javaclient
   Step 1 : FROM java:7
    ---> fa92e621c863
   Step 2 : COPY /src /HelloRedis/src
    ---> 3c03510b7d63
   Removing intermediate container 0677d35671e7
   Step 3 : COPY /lib /HelloRedis/lib
    ---> e8a3e4b97232
   Removing intermediate container d57cc413f15d
   Step 4 : WORKDIR /HelloRedis
    ---> Running in 6d0dc774f3b8
    ---> 4e7f89946fbf
   Removing intermediate container 6d0dc774f3b8
   Step 5 : RUN javac -cp lib/jedis-2.1.0-sources.jar -d . src/HelloRedis.java
    ---> Running in 3f864a7e7060
    ---> b12b150e9564
   Removing intermediate container 3f864a7e7060
   Step 6 : CMD java HelloRedis
    ---> Running in a4206d4b6f70
    ---> ed3870e00457
   Removing intermediate container a4206d4b6f70
   Successfully built ed3870e00457
   Creating helloredis_javaclient_1
   ubuntu@ucptest-0:~/HelloRedis$
   ```
   
6. Now run `docker-compose ps`. What can you observe?

   ```bash
   ubuntu@ucptest-0:~/HelloRedis$ docker-compose ps
            Name                       Command             State    Ports
   ------------------------------------------------------------------------
   helloredis_javaclient_1   java HelloRedis               Up
   helloredis_redis_1        /entrypoint.sh redis-server   Up      6379/tcp
   ```

7. Switch over to UCP on your web browser
8. Click on the “Applications” link on the left navigation bar
9. You should see the following output

   ![](images/ucp02_t4_applications.PNG)

10. Click on the “Show” link on the right side to expand the view of the application

   ![](images/ucp02_t4_applications_expanded.PNG)
   
   You will notice that when we expand the the view of the `helloredis` application, we also see each service that the application is composed of. 

## Deploy another application   

For the following section, use what you have learnt just now and complete the steps listed below.
   
1. Now that you’ve deployed your first application, it’s time to try another example. Go to https://github.com/prakhar1989/FoodTrucks
2. Clone the `FoodTrucks` repo into your **UCP Controller** AWS instance
3. Deploy the `FoodTrucks` application into UCP
4. View the application in your web browser

   If you completed all the steps correctly, you should see a very cool application that allows you to search for food trucks in San Francisco
   
   ![](images/ucp02_t4_foodtrucks.PNG)




   

