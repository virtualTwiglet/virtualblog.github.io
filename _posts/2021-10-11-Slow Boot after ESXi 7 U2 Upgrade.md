---
layout: post
title: Slow Boot after ESXi 7 U2 Upgrade
---
Working with a customer which was in the process of migrating away from SD Cards and moving to BOSS devices as VMware are phasing out support for booting from SD Cards and stand alone boot devices as per kb article [VMware KB Article 85685](https://kb.vmware.com/s/article/85685). So whilst migrating to the BOSS devices the customer was also taking the opportunity to upgrade vSphere.

However during this upgrade we encountered a bizarre issue whereby after completing an in place upgrade of a host it would take around an hour to complete the boot process.

During testing we discovered if a fresh install of ESXi was completed the hour boot process was not observed.
Obviously a fresh install wasn't an option for this customer as they required all of the vSwitch and Storage configurations configured (a significant amount of Standard vSwitches) required maintaining in the upgraded version. So a fresh install of ESXi was not an option.

After some digging around I managed to locate the VMware KB article that needs to be followed whilst upgrading to ESXi 7.2 [VMware KB Article 84249](https://kb.vmware.com/s/article/84249)

A condensed version of the KB is as follows:  
1.You need to copy boot.cfg from bootbank to /altbootbank and edit it 

  *SSH to the ESXi host in question  
  *cp /bootbank/boot.cfg /altbootbank/boot.cfg  
  *vi /altbootbank/boot.cfg  
  *Change the first line 'bootstate=0' > 'bootstate=3'  
  *Change last line 'updated=N' > 'update=N-1' (eg update=3 > updated=2)  

2.Save file  
3.Reboot and test  

Hopefully this will have resolved the hour long boot process.
