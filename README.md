# F5 Auth Tree Node

## Installation

### Prerequisites

1) Tomcat 9 
2) Java 8 or aboove
3) Forgerock Open AM (7.1.2)

### Steps to deploy F5 

1) Clone a F5 Maven Project from github and run mvn package command.
2) The project will generate a .jar file under target folder of repository containing our custom nodes. i.e. F5-ForgeRock-Integration-1.0.jar.
3) F5 jar file→paste it in “tomcat/webapps/openam/WEB-INF/lib”
4) If you are updating a newer version of the Jar file. First, delete the older version and then copy the higher version.
5) Restart the tomcat server.

## Forgerock authentication workflow setup

Below are the nodes that will be available after deploying the jar file:

1) F5 XC FR Configurations node.
2) F5 XC Analyze Machine node.
3) F5 XC Account Protection node.
4) F5 XC Check Persistence node.
5) F5 XC Check Session node.
6) F5 XC Authentication Intelligence node.
7) F5 XC Set Session Settings node.
8) F5 XC Set Session Data node.

![node7](https://user-images.githubusercontent.com/106667867/231533564-0cde07cf-5752-4426-a204-de2b8937ee99.png)
<img width="162" alt="nodes2" src="https://user-images.githubusercontent.com/106667867/229393864-0ed7fbde-ac76-4f5c-a9d5-9fccda47f69f.png">
<img width="132" alt="nodes3" src="https://user-images.githubusercontent.com/106667867/229393871-e06a3fac-eeb2-46c5-ad22-4e3a08a11bab.png">

### F5 XC FR Configurations node
```js
Discription : This Node will be used to capture all the configurations needed for the rest of the F5 nodes to operate.

Output : It contains generic configuration, Account protection specific configuration and Authentication Intelligence Specific configurations.

Configurations/Inputs : Configuration details are provided below

### Generic Configurations -

1. JS Path Frontend: Specify JS Path at the front end, recommend using the same as JS Path Backend, but the Customer can customize it.
   Example: /__imp_apg__/js/sed-qa_test-aa589dfb.js

2. Customer ID: Specify the Customer ID, provided by F5, but the Customer can customize it. Example: sed-qa_test-aa589dfb

3. Cookie Name: Specify Cookie Name, provided by F5, but Customer can customize. Please update F5 before customizing. This cookie name will be needed to      extract the cookie value and decrypt it. Example : _imp_apg_r_

4. Decryption Key for Cookie: Specify the decryption key, provided by F5. Example : Test!1234

5. Flag For JS Ingestion: Use the toggle button to enable or disable JS injection. True/False

6. Default Session in Mins: This is the Max Session time used for any user without any session extension recommendations. Post this time the user will be logged out automatically and they have to provide credentials to log in again.

7. FR Session User Attribute Name: This is a user attribute available in the Datastore of ForgeRock. This user Attribute will be used by the F5 Plugin to
save session-related data.

### Account Protection Specific configurations –

8. Account Protection Enabled: Use the toggle button to enable or disable AI action.

9. Action When Cookie Cannot Decrypt: Define the action (Block, Redirect, Continue) by using the dropdown when the cookie cannot decrypt.

10. Action When Cookie Not Found: Define the action (Block, Redirect, Continue) by using the dropdown when the cookie is not found.

11. Fr = 2 Action: Define various initiating points and what action to be taken, when fr recommendation in 2. Possible actions are – Block, Continue, and Redirect

12. Fr = 3 Action: Define various initiating points and what action to be taken, when fr recommendation in 3. Possible actions are – Challenge, Continue, and Redirect

### Authentication Intelligence Specific configurations –

13. Account Intelligence Enabled: Use the toggle button to enable or disable AP action.

14. Endpoint Actions for Session Extension: Define various initiating points and what action to be taken, when the dc recommendation is Extend (“eX”. Eg –
e7,e30 or e90). Possible actions are – Extend and Default.
** Extend – Extend the user session based on the F5 recommendation.
** Default – Ignore the F5 session extension recommendation and let the user log in with the default session time only.
```

### F5 XC Analyze Machine node
```js
Discription : This Node will send a JS Tag if the JS Tag flag is enabled inthe F5 XC Configuration Node. The Login page which is integrated with the F5 plugin will read this JS Tag and insert it into the Login page to initiate the JavaScript injection.

Output : True – Java script injection is successful, and the node receives an encrypted cookie.

Configurations/Inputs : This node has no configuration items
```

### F5 XC Account Protection node
```js
Description : This Node will implement the Account protection feature. The following tasks will be done at this node –
** Decrypt the F5 cookie and read the fr recommendations.
** Take action (Continue, Block, Challenge or Redirect) based on the AP configurations provided in the F5 XC Configurations node.

Output : The following are the possible outputs of this node –
** AP Disabled – Account protection is not enabled at the F5 XC Configuration node.
** The rest are the AP actions that are decided based on the fr recommendations received by the F5 cookie.

o Continue
o Block
o Redirect
o Challenge

Configurations/Inputs : This node has no configuration items
```

### F5 XC Authentication Intelligence node.
```js
Discription : This Node will implement the Authentication Intelligence feature. The following tasks will be done at this node –
** Decrypt the F5 cookie and read the dc recommendations.
** Take action (extends the user session or let the user log in with default session time) based on the AI configurations provided in the F5 XC Configurations node.

Output : The following are the possible outputs of this node –
** AI Disabled – Authentication Intelligence is not enabled at the F5 XC Configuration node.
** The rest are the AI actions that are decided based on the fr recommendations received by the F5 cookie.

o Eligible (EX)
o Ineligible (INE)
o Eligible Control (EC)

Configurations/Inputs : This node has no configuration items
```

### F5 XC Check Persistence node
```js
Discription : This node helps save the results of the Persistence Decision Cookie node output. And save it to the Shared state, which will then be used by other nodes to make necessary decisions on the user login.

Output : True

Configurations/Inputs : Persistence Cookie Present toggle button. Enable it if the Node is connected to the True Output of Persistence Decision Cookie Node else false.
```

### F5 XC Check Session node
```js
Discription : This node will verify the user session’s validity. If the user session is under the allowed time limit, the outcome of this node will be “True” otherwise “False”. Moreover, if the user is over the allowed max session time limit, this node will force the user to re-authenticate.

Output : 
o True – If the user has a valid session (under the max session time)
o False – if the session is expired or there is no user session available.

Configurations/Inputs : This node has no configuration inputs
```

### F5 XC Set Session Settings node
```js
Discription : This node will set the session max time based on the recommendations received from F5 Cookie (dc value), which is evaluated at the F5 XC Authentication Intelligence node.

Output : The following are the possible outputs of this node –
o Auth – This means the user does not have a valid session and has to re-authenticate.
o Pass – The user has a valid session thus,let the user login without any authentication.

Configurations/Inputs : This node has no configuration inputs.
```

### F5 XC Set Session Data node
```js
Discription : This node will save the session and AP/AI action details into a user attribute (configured in F5 XC FR Configuration Node).

Output : True

Configurations/Inputs : This node has no configuration inputs.
```


## Additional configuration 

ForgeRock built-in nodes – 

1) Page Node : This is just a placeholder node where other nodes can be dragged to be grouped.

2) Data Store Decision Node : This node will verify the Username and Password given by the user. The output will be true if Username and password match with the Data Store user otherwise return false.

4) Username Collector Node : This node will send a callback to the requesting server so that the server can respond back with the username for the authentication process.

5) Password Collector Node : Similar to the username collector node, this node will collect the password for the user authentication process.

6) Set Persistence Cookie Node : This is to set the persistence cookie for the user session. For more details please check the link - https://backstage.forgerock.com/docs/auth-node-ref/7.3/auth-node-set-persistent-cookie.html

7) Message Node : These nodes are utilized just as an example of Redirect and Challenge. Implementing the Redirect and challenge feature for F5 flow is dependent on each customer.



### Configure the HMAC Signing Key 
```js
1. generate HMAC Signing key and set into the Persistent Cookie Decesion Node and Check Perssitance Node.
2. 6.	Update the signing key – go to Authentication -> Settings->Security-> Organization Authentication Signing Secret. 
And paste the same key here as well and save the changes. 
```

### Create a Custom User Attribute
```js
ForgeRock User attributes are parameters that define user-related data. ForgeRock use these user attributes in following ways - 

1. To save user related data – These attributes are used only to save user related data. For eg – First Name, Last Name, Address, Pin Code, etc. 
2. To save ForgeRock internal data – These attributes will be used for ForgeRock Operations. For eg – iplanet-am-user-login-status – this attribute will be used internally by ForgeRock to save user’s login status.

F5 ForgeRock Plugin, will use a User attribute to save the users session and recommendation actions details. 
To avoid any conflict with ForgeRock’s predefined user attribute , creation on a custom attribute is recommended. 

For testing purposes, we can use any existing user attribute ex- “telephoneNumber”.
```

### Create Identity/User
```js
Every user created in ForgeRock is called an Identity. 
ForgeRock manages identities via Identity stores. ForgeRock provides its own embedded Identity Store as well as it supports third-party identity store. 
For eg – Active Directory, SUN/Oracle DSEE, Tivoli Directory services etc. 

For testing purpose admin can create specific users via Identity stores as shown below 
```

## Configure the trees as follows

### F5 Authentication tree flow
<img width="470" alt="image" src="https://user-images.githubusercontent.com/106667867/229398579-8363659d-acc5-4315-a414-88cfefc18f24.png">


### Account Createtion Workflow
Create workflow for Account creation using Account protection features. 

<img width="491" alt="nodes4" src="https://user-images.githubusercontent.com/106667867/229397440-2c8b91ed-679e-407d-9aa8-d359490e0e99.png">


## Set Logging Level

* User can set log level in forgerock instance, To set user need to follow this path:
```js
DEPLOYMENT-->SERVERS-->LocalInstance-->Debugging
```
Logs will be available under userDir/openam/var/debug/debug.out (Example: In mac - home/user/openam/var/debug/debug.out)


## Configure the trees as follows
```js
* Navigate to **Realm** > **Authentication** > **Trees** > **Create Tree**




