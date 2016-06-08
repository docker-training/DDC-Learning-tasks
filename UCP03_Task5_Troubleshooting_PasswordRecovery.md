# Task 5 - Troubleshooting - Password Recovery

In this task you will see how to recover from a scenario where the password to the built-in UCP admin account has been lost.

If a regular user (not the built-in **admin** account) forgets their password they must contact the UCP admin who can set a new password for them. 
However, if the password for the built-in **admin** account is lost, there is no built-in email based reset function. We must run a command against a UCP container to reset the password

## Pre-requisites
- UCP configured to use the **Managed** authentication method (**Settings** > **Authentication**).

  > The procedures described in this lab will not work if UCP is configured to use the **LDAP** authentication method.

## Step 1 - Recovering admin passwords


1. If you have not already done so, login to UCP as the **admin** user.
2. Go to **Settings** and click the **AUTH** tab.
3. Make sure the Authentication Method is set to **Managed**.
4. Change the password of the Admin account to something else and then logout

   Click on **profile** from your user account dropdown list.
   
   ![](images/ucp02_t4_profile_dropdown.PNG)
   
   Fill in the new password of your choice and click **Change Password**
   ![](images/ucp03_t5_ChangePW.PNG)  

5. Pretend that you have forgotten the password to your admin accout.
6. SSH into the UCP Controller node
7. If we do a quick `docker ps` we can see all our UCP containers.

   ```   
   7b7d60c6abe4        docker/ucp-controller:1.1.1   "/bin/controller serv"   5 hours ago         Up 5 hours          0.0.0.0:443->8080/tcp                                                             ucp-controller
   653b604f74c7        docker/ucp-auth:1.1.1         "/usr/local/bin/enzi "   5 hours ago         Up 5 hours          0.0.0.0:12386->4443/tcp                                                           ucp-auth-worker
   38135e049bf4        docker/ucp-auth:1.1.1         "/usr/local/bin/enzi "   5 hours ago         Up 5 hours          0.0.0.0:12385->4443/tcp                                                           ucp-auth-api
   4a0b6f5636ba        docker/ucp-auth-store:1.1.1   "/usr/local/bin/rethi"   5 hours ago         Up 5 hours          0.0.0.0:12383-12384->12383-12384/tcp                                              ucp-auth-store
   ebb8deb21e23        docker/ucp-cfssl:1.1.1        "/bin/cfssl serve -ad"   5 hours ago         Up 5 hours          8888/tcp, 0.0.0.0:12381->12381/tcp                                                ucp-cluster-root-ca
   29c85b229c2c        docker/ucp-cfssl:1.1.1        "/bin/cfssl serve -ad"   5 hours ago         Up 5 hours          8888/tcp, 0.0.0.0:12382->12382/tcp                                                ucp-client-root-ca
   4184c66881b2        docker/ucp-swarm:1.1.1        "/swarm manage --tlsv"   5 hours ago         Up 5 hours          0.0.0.0:2376->2375/tcp                                                            ucp-swarm-manager
   1921b913b2a8        docker/ucp-swarm:1.1.1        "/swarm join --discov"   5 hours ago         Up 5 hours          2375/tcp                                                                          ucp-swarm-join
   e6ab82c02bd5        docker/ucp-proxy:1.1.1        "/bin/run"               5 hours ago         Up 5 hours          0.0.0.0:12376->2376/tcp                                                           ucp-proxy
   67d7c53e6cf1        docker/ucp-etcd:1.1.1         "/bin/etcd --data-dir"   5 hours ago         Up 5 hours          2380/tcp, 4001/tcp, 7001/tcp, 0.0.0.0:12380->12380/tcp, 0.0.0.0:12379->2379/tcp   ucp-kv
   ```
   
   The container we want to access is `ucp-auth-api`. 
      
8. Run the following command. 

   `docker exec -it ucp-auth-api enzi "$(docker inspect --format '{{ index .Args 0 }}' ucp-auth-api)" passwd --interactive`
   
9. You will be prompted for the username. Specify **admin**. Then specify the password **orca**
   ```
   ubuntu@ucp-controller:~$ docker exec -it ucp-auth-api enzi "$(docker inspect --format '{{ index .Args 0 }}' ucp-auth-api)" passwd --interactive
   Username: admin
   Password:
   Verify Password:
   INFO[0007] successfully set user account password: admin
   ```
   
10. Login to the **admin** account on the UCP web UI, making sure that the password is **orca**



   
   
   
   
   