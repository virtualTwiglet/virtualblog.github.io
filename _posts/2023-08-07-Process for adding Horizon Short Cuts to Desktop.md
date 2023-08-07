---
layout: post
title: Process for adding Horizon Short Cuts to Desktop (hidden)
---
Step 1: Install Horizon Client 
The first step is to install the Horizon Client to enable the LogonAsCurrentUser feature and to set the Horizon Connection Server details as well as install elements.

Create a batch file for installing the Horizon Client for example eucclientinstall.cmd:  
•VMware-Horizon-Client-2306-8.10.0-21964678.exe /s /v /qn VDM_SERVER=ServerURL VDM_IP_PROTOCOL_USAGE=IPv4 LOGINASCURRENTUSER_DEFAULT=1 ADDLOCAL=All DESKTOP_SHORTCUT=0 AUTO_UPDATE_ENABLED=0 /norestart /L C:\Software\HznClient.log

Step 2: Enable Horizon Client to run Auto-Start at Logon in Windows 
Login to the Windows machine with an administrative account.
•Click the Windows logo and type “Run” and press enter 
•In the Run command box enter “shell:common startup” press enter 


<img src="{{ site.baseurl }}/images/horizon-client/run.png">

•This will launch Windows Explorer to the All Users Startup location 
•Copy the Horizon Client shortcut here 


<img src="{{ site.baseurl }}/images/horizon-client/startup.png">


•Edit the properties of the Horizon Client shortcut  
•In the Target box add the following to the command line “-serverURL CSAddress -LogonAsCurrentUser True -installShortcutsThenQuit” 

<img src="{{ site.baseurl }}/images/horizon-client/client-properties.png">

•So, the line will for example look like:  
•“C:\Program Files\VMware\VMware Horizon View Client\vmware-view.exe” -serverURL CSAddress -LoginAsCurrentUser true -installShortcutsThenQuit 
•To confirm that the Horizon Client is available to Auto-Start 
•Launch “Settings” and select “Apps” and scroll down to “Startup” 

<img src="{{ site.baseurl }}/images/horizon-client/apps-startup.png">

•Click “Startup” and verify that the Horizon Client displays in the list and is enabled “On” 
<img src="{{ site.baseurl }}/images/horizon-client/apps-startup2.png">

Step3: Enable Logon As Current User on the Connection Server

•Login to the Horizon Admin Console and go to Servers 
•Select Connection Servers Tab and select Connection Server click Edit 

<img src="{{ site.baseurl }}/images/horizon-client/server-properties.png">

•Select the Authentication Tab 
<img src="{{ site.baseurl }}/images/horizon-client/server-auth.png">

•Browse down and enable Accept logon as current user 
<img src="{{ site.baseurl }}/images/horizon-client/server-currentuser.png">

Step4: Test Shortcuts Available  
•Login to workstation with test user  
•Required shortcuts are presented to Desktop or Start Menu  
<img src="{{ site.baseurl }}/images/horizon-client/desktop.png">

Summary  
You should now have a solution whereby when a user logs in to the workstation Horizon Client will take their logon credentials and auto launch in the background and pull down any desktop and start menu items they are entitled to.
