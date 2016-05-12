# Task 4 - User Management with LDAP

## Pre-requisites
- Pre-configured LDAP server with users and groups setup. Your instructor will provide you with details.
- Basic understanding of LDAP structure

## Step 1 - Examine the LDAP server

1. Open a browser and go to http://LDAP_SERVER/phpldapadmin. LDAP_SERVER should be the domain name or public IP address of your LDAP node. This will open up the GUI for your LDAP server
2. Login to the server with the following credentials:

   **Login DN:** cn=admin,dc=test,dc=com
   **Password:** admin
   
3. Expand the left navigation bar and take note of the LDAP entries underneath `ou=all users`, `ou=engineering` and `ou=HR`

   ![](images/ucp03_t4_LDAP.PNG)
   
   In this setup, we have 6 users.
   
## Step 2 - Integrate UCP and LDAP

1\. Login to UCP as the admin user

2\. Go to the “Settings” page of UCP

3\. Click on the Auth section and change the Method from "Managed" to "LDAP"

4\. Specify your LDAP server URL. Use the format "ldap://<domain name to your LDAP node>"

5\. Select "Full Control" on the **Default Permission for Newly Discovered Accounts** field

6\. For the **LDAP Server Configuration** section, fill in the following details.

| Field                     | Value            |
| :---------                | :-----------     |
| Recovery Admin Username   | admin            |
| Recovery Admin Password   | orca         |
| Reader DN                 | cn=Chuck Norris,ou=all users,dc=test,dc=com |
| Reader Password           | password  |
   
7\. For the **LDAP Security Options**, leave both options unchecked.

8\. For the **User Search Configurations** section, fill in the following details.

| Field                     | Value            |
| :---------                | :-----------     |
| Base DN                   | dc=test,dc=com   |
| Username Attribute        | uid |
| Full Name Attribute       | cn  |
| Filter                    | objectClass=inetOrgPerson |

8\. Tick the **Scope Subtree** checkbox

   ![](images/ucp03_t4_LDAP_user_search.PNG)
   
9\. Scroll down to the **Test LDAP Connection** section and specify the following:

| Field                     | Value            |
| :---------                | :-----------     |
| LDAP Test Username        | cnorris          |
| LDAP Test Password        | password         |

10\. Click the **Test** button and verify that you get a **Success** message

11\. Click **Update Auth Settings**
   
## Step 3 - Test User Access

1. In the **LDAP Sync Status** section, click on the **Sync Now** button.
2. Click on **User and Teams** and check to see that all the users from our LDAP server are present. You will also notice that
   the previous users you created are still present.
   
3. Logout of UCP as the admin user. Try and login as user `johnfull`. What do you notice?

   Once UCP is integrated with LDAP, all user accounts will come from LDAP. **Managed** accounts that were previously setup in UCP will be disabled, accept for the account that your specified in the **Recovery Admin Username** 
   configuration field. 
   
4. Login as the user `cnorris`. The password is `password`

5. Logout as `cnorris` and log back in as `admin`. Disable the LDAP integration by switching the Authentication back to **Managed**


   






   
