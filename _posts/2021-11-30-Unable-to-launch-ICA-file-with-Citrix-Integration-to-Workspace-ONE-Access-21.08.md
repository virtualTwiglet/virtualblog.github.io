---
layout: post
title: Unable to launch ICA file with Citrix Integration to Workspace ONE Access 21.08
---
I was brought in to assist and troubleshoot for a customer who was experiencing issues whilst integrating Citrix with their Workspace ONE Access environment. When ever they tried to launch a Citrix App via the portal it would fail with the following message.
<img src="{{ site.baseurl }}/images/WS1-ICA-Request.jpg"> <br><br>

Investigating the issue from the Workspace ONE Access dashboard reports page under resource usage. I could see that it was failing to launch the ICA file due to communication errors between the Workspace ONE Access Connector and the Citrix StoreFront Server.
<img src="{{ site.baseurl }}/images/WS1-ICA-Request-Failed.jpg"> <br><br>

Everything from the Workspace ONE admin console and the Connector install was configured correctly as we could at least display the Citrix Virtual Apps on the user portal and attempt to launch them. So this would seem to suggest that something had been overlooked during the configuration between the Connector and StoreFront during the initial installation. It had to be something small as everything else was correctly configured.
Now something I had read about previously was nagging in the back of my mind with regards to the Trusted Domains configuration in Citrix.<br><br>

We needed to launch the management console to check the configuration for Trusted Domains.
Select Stores in the left pane of the Citrix StoreFront management console, and in the Actions pane, click Manage Authentication Methods. Click the drop down box and select Configure Trusted Domains.
<img src="{{ site.baseurl }}/images/WS1-ICA-TrustedDomain.jpg"> <br><br>

It was here that it was confirmed that the customer had only entered the NETBIOS name for the Trusted Domain. This I believed was the issue so we needed to update the Trusted Domains to include the FQDN.
Enter the appropriate FQDN in the Trusted Domains field for the installation. It is important to remember here that although it is still acceptable to use the NETBIOS name it won't be recognised when using the 21.08 Access Connector. It is important to set the default Trusted Domain as the full FQDN of the environment.
<img src="{{ site.baseurl }}/images/WS1-ICA-TrustedDomain-Config.jpg"> <br><br>

Once the customer had completed the above configuration changes to their StoreFront Trusted Domains they were then able to receive and launch the ICA files for the relevant published Virtual Apps. Success and one happy customer. It's always the small things that catch us out.<br><br>
To save others the hassle of searching for the relevant document that states quite clearly the required for the full FQDN I have included here for reference [Workspace ONE Access Connector 21.08][def1]


[def1]: https://docs.vmware.com/en/VMware-Workspace-ONE-Access/21.08/ws1-access-resources/GUID-66F24F8D-72BE-43EA-A81C-B041AD631E4A.html