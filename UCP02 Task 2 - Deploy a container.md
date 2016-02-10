# Task 2: Deploy a container

## Prerequisites
UCP setup with 3 nodes connected to the UCP controller

# Deploy a container
In this exercise we will deploy an NGINX container

1. Click on **Containers** link on left navigation bar
2. Click on **Deploy Container** button
3. On the Basic settings section fill in the following details:
4. **Image Name:** nginx  
   **Container Name:** nginx_server
5. Go to the Network section and configure the following port mappings:

![](images/ucp02_t2_networksettings.png)



6. Hit the “Run Container” button on the right side panel
7. You should now see your container listed. Check that it is running on node-0

 

8. Click on the container details button on the right side and check the port mappings section
Access the NGINX server on your web browser. You will need to specify the hostname of the node that the NGINX container is running on
