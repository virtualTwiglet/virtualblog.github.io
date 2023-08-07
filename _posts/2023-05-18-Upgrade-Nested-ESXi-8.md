---
layout: post
title: Upgrade Nested ESXi 8 to latest vSphere Version
---
I have a 3 node nested ESXi 8.0 GA build that I needed to upgrade to the latest version of vSphere 8. However trying to upgrade using vCLS or the offline bundle or ISO kept failing with unsupported errors (old lab kit).

Not one to give in I kept at it until I found that there was another alternative in using the using the online VMware Depot binaries (Huzza!).


So first off lets put the first host in to maintenance mode:


<img src="{{ site.baseurl }}/images/upgrade-esxi/maint-mode.jpg">

Now off to the command line. So using Putty connected to my ESXi host.
First things first needed to allow the host to connect to the internet:


esxcli network firewall ruleset set -e true -r httpClient
<img src="{{ site.baseurl }}/images/upgrade-esxi/enable-http.png">


Lets check for updates available for vSphere 8:
esxcli software sources profile list -d https://hostupdate.vmware.com/software/VUM/PRODUCTION/main/vmw-depot-index.xml grep | -i Esxi-8.0   
Now we know the available updates we can selected the appropriate version and insert in to the following  command line 


Next the command to download and install the updated binaries:


esxcli software profile update -d https://hostupdate.vmware.com/software/VUM/PRODUCTION/main/vmw-depot-index.xml -p ESXi-8.0U1-21495797-standard
<img src="{{ site.baseurl }}/images/upgrade-esxi/vmw-depot-cmd.png">

Once the command is run you won't have anything displayed you just have to wait for this to complete. However I still ran into the issue where the hardware (CPU) wasn't support and the update failed (again).
<img src="{{ site.baseurl }}/images/upgrade-esxi/hardware-warning.png">

However the failure did come back with the option to possibly resolve the failure and allow for the host to be upgraded. So lets add that to the command string and try again.


esxcli software profile update -d https://hostupdate.vmware.com/software/VUM/PRODUCTION/main/vmw-depot-index.xml -p ESXi-8.0U1-21495797-standard --no-hardware-warning
<img src="{{ site.baseurl }}/images/upgrade-esxi/vmw-depot-cmd-hardware.png">

Perfect this time the upgrade completed successfully. All we had to do was reboot and confirm the host was upgraded to the latest version.
<img src="{{ site.baseurl }}/images/upgrade-esxi/reboot.png">

Running vmware -v confirmed that the host had been successfully upgraded.
<img src="{{ site.baseurl }}/images/upgrade-esxi/version.png">

Now to upgrade the remaining hosts!
