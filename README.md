# F5 Auth Tree Node

Secure your digital properties by leveraging F5’s proven approach to solving today's most sophisticated cybersecurity and fraud challenges – empowering you to deliver exceptional and secure digital engagements. F5 Distributed Cloud Services support a wide variety of use cases for businesses of any size to connect and secure distributed applications across public/private cloud and edge infrastructure while leveraging a single policy engine and management console. Unlike commoditized bot and fraud solutions that require extensive manual operation and introduce unnecessary friction for consumers, F5 Distributed Cloud Services for bot and fraud provide ForgeRock applications ongoing and seamless protection. Our no-friction approach mitigates the need for clunky multi-factor authentication or CAPTCHA – providing your customers optimal and secure digital experiences.    

  

F5 Distributed Cloud Services are trusted by leaders at the world's largest banks, retailers, and airlines. Now you can protect your Adobe Commerce site against malicious bots, seamlessly authenticate users, and stop online fraud – enabling you to fully maximize your ForgeRock investment.  

  

Distributed Cloud Account Protection monitors transactions in real-time across the entire user journey to determine user intent and to identify malicious activity. This cutting-edge solution detects and eliminates online fraud with AI powered by a real-time closed-loop engine that stops illegitimate activity before it happens. 

  

Distributed Cloud Authentication Intelligence identifies multiple users on one device, determines user intent, and maintains a globalized historical profile—all to make life easy for good users, but hard for everyone else. With this solution, network operators can securely enable trust across the entire customer journey without negatively impacting real users. And even when an attack campaign tries to bypass F5 defenses by somehow retooling, we are still able to identify the campaign based on hundreds of other signals. 

## Installation

### Prerequisites

1) Tomcat 9 
2) Java 8 or aboove
3) Forgerock Open AM (7.1.2)

### Steps to deploy F5 

4) Sign up and download integration jar file from https://my.f5.com/manage/s/downloads.  Once logged into F5 downloads, select " XC-Bot-and-Risk-Management " as product family, and ForgeRock Connector. 
5) The .jar file contains custom nodes. 
6) Paste F5 jar file in “tomcat/webapps/openam/WEB-INF/lib” 
7) If you are updating a newer version of the Jar file. First, delete the older version and then copy the higher version. 
8) Restart the tomcat server.

### ForgeRock authentication workflow setup 

Below are the nodes that will be available after deploying the jar file: 

9) F5 XC FR Configurations node – general configuration needed to set up Account Protection and Authentication Intelligence. Detailed configuration instructions are provided in the User Manual in the integration package.   

![image](https://user-images.githubusercontent.com/94064355/232973722-36b7e2b9-03db-4fb0-b608-47ecf88b05d3.png)

10) F5 XC Analyze Machine node – this node will help securely decrypt F5 recommendations 

11) F5 XC Account Protection node – this node will take Account Protection action based on recommendation and configurations 

12) F5 XC Check Persistence node – this node will check persistence of authentication related cookies tokens  

13) F5 XC Check Session node – this node will check session 

14) F5 XC Authentication Intelligence node - this node will take Authentication Intelligence action based on recommendation and configurations 

15) F5 XC Set Session Settings node – this node facilitate session setting related tasks to be achieved with the tree 

16) F5 XC Set Session Data node - this node facilitates with setting session data  

### Set Logging Level 

User can set logging level in ForgeRock instance with the following path: 

DEPLOYMENT-->SERVERS-->LocalInstance-->Debugging 

Logs will be available under userDir/openam/var/debug/debug.out (Example: In mac - home/user/openam/var/debug/debug.out) 

### Configure the trees with the following 

* Navigate to **Realm** > **Authentication** > **Trees** > **Create Tree** 
