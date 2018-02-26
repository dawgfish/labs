
Skip to content
Enterprise
This repository

    Pull requests
    Issues
    Gist

    @brad-gough

2
1

    1

global-enablement/tech-summit
Code
Issues 0
Pull requests 0
Projects 0
Wiki
Pulse
Graphs
tech-summit/hycu/index.rst
ddf9462 an hour ago
@michael-jastad michael-jastad Update index.rst
@michael-jastad
@brad-gough
209 lines (157 sloc) 9.74 KB
-----------------------
Comtrade HYCU
-----------------------

Overview
++++++++

Welcome to the Nutanix lab for HYCU!  In this lab you will go through installing HYCU, configuring HYCU, backing up vm's, and restoring vm's.  The goal of this lab is for you to understand how HYCU works in order to demo and pitch this with your clients.

Pre-requisites
+++++++++++++

 	- Nutanix cluster running AHV
	
 	- Prism Cluster URL, id with admin privileges and password
	
 	- Image of HYCU uploaded to Prism Image configuration
	
 	- Windows and/or Linux VMs
	
 	- Administrator Login credentials for your Windows VMs 
	
 	- Windows Microsoft SQL Server
	
 	- IPv4 address and Hostname for HYCU Controller
	
 	- Subnet mask, Default gateway, DNS Server and Search domain for HYCU controller
	


EXERCISE 1 - INITIAL DEPLOYMENT
+++++++++++++++++++++++++++++++

Objective:	Create HYCU backup controller VM

Prerequisites:	HYCU VM available in Prism Image Configurator 

 	Start by adding a new virtual machine in Prism by clicking “Create VM”
	
 	Configure VM name and compute details (Picture 1)
	
 	- 2 vCPUs
	
 	- 2 CORES
	
 	- 4 GB Memory
	
 	- Attach HYCU disk image to the new VM (Picture 2 & 3)
	
 	- Type = DISK
	
 	- Operation = Clone from Image service
	
 	- BUS Type = SCSI
	
 	- Image name = <name of HYCU image downloaded>
	
 	- Add network card to the HYCU VM
	
 	- VLAN ID = <select from pull down> <Recommendation: use default>
	
 	- Power the just created HYCU VM on 
  
  .. figure:: https://s3.us-east-2.amazonaws.com/s3.nutanixtechsummit.com/hycu/images/image1.png
  
  .. figure:: https://s3.us-east-2.amazonaws.com/s3.nutanixtechsummit.com/hycu/images/image2.png
  
  .. figure:: https://s3.us-east-2.amazonaws.com/s3.nutanixtechsummit.com/hycu/images/image3.png
  
  
  EXERCISE 2 - NETWORK AND CONTROLLER SETUP
  +++++++++++++++++++++++++++++++++++++++++
  
  Objective:	Network setup for HYCU backup controller
  
  Prerequisites: HYCU VM is created, and powered ON
  
   	In Prism, Select the HYCU VM from VMs list and click on “Launch console”
 	When the console launches, you will be greeted with the initial Network setup screen in order to setup the HYCU static   	 network

  parameters:
 	- Hostname = <Part of pre-requisites>
	
 	- IPv4 address = <Part of pre-requisites>
	
 	- Subnet mask = <Part of pre-requisites>
	
 	- Default gateway = <Part of pre-requisites>
	
 	- DNS server = <Part of pre-requisites>
	
 	- Search domain = <Part of pre-requisites>
	
 	- Fill and confirm with “Enter“ button press. 
	
	.. Note :: that you can switch between lines by pressing the “Tab“ button.
 	Once configured, and confirmed, wait a minute for the console to save its settings, and initialize completely
	
 	.. Note:: Write down the URL you will use to access the HYCU UI** ( Picture 7 )
	
	.. figure:: https://s3.us-east-2.amazonaws.com/s3.nutanixtechsummit.com/hycu/images/image7.png

  EXERCISE 3 - ADD CLUSTER
  ++++++++++++++++++++++++
  
  Objective:	Add cluster to HYCU
  
  Prerequisites: HYCU successfully installed, and running

  Login to the HYCU Web Interface by navigating to https://your_configured_ip_address:8443

   And you will be greeted with the Hycu login screen ( Picture 8 )

   Default login credentials to HYCU are:
   
	- Username: admin
	
	- Password: admin   
	
	.. Note :: It is recommended to change these as soon as you log in for the first time.

 	After your first successful login, start by adding the Nutanix cluster to HYCU. From the top right corner, 	 		Administration icon, select Add Nutanix Clusters (Picture 9)
	
 	Fill out the fields in the “Add Nutanix Cluster” form, and confirm with “Save” button. (Picture 10)
	
 	- Cluster Prism Element URL = <collected during pre-requisites>
	
 	- User = <collected during pre-requisites>
	
 	- Password = <collected during pre-requisites>
	
 	Upon successful entry, you should see your cluster added (Picture 11)
 	Close the Windows by clicking the “Close button” and make sure your VM’s have been successfully discovered by HYCU
	
	.. figure:: https://s3.us-east-2.amazonaws.com/s3.nutanixtechsummit.com/hycu/images/image8.png
	
	.. figure:: https://s3.us-east-2.amazonaws.com/s3.nutanixtechsummit.com/hycu/images/image9.png
	
	.. figure:: https://s3.us-east-2.amazonaws.com/s3.nutanixtechsummit.com/hycu/images/image10.png
	
	.. figure:: https://s3.us-east-2.amazonaws.com/s3.nutanixtechsummit.com/hycu/images/image11.png
	

EXERCISE 4 - ADD TARGET
+++++++++++++++++++++++

Objective: Add target to store backups and restore points

Prerequisites:	HYCU VM Configured and Nutanix Cluster Added

 	Login to HYCU UI
	
 	Select “Targets” from the left-hand pane
	
 	Click “+ New" button in the top right corner (Picture 12)

Target type: NFS
Even though Nutanix storage container's can be used as an NFS target, HYCU recommend's using Volume Groups as an iSCSI target.

 	Create a new container from Prism with at least 100GB of storage (Picture 13)
	
 	Expose that container as a HYCU NFS target (Picture 14)
	
 	Make sure target was successfully added (Picture 15)

Target type: SMB

 	Setup a shared directory on one of the Windows machines
	
 	Expose that shared directory as a HYCU SMB target (Picture 16).

Target type: iSCSI

Nutanix volume group can be used as an iSCSI target

 	Create new Nutanix volume group from Prism with at least 100GB of storage (Picture 17).
	
 	Register new Client to Volume Group by using HYCU IP address or ISCSI Initiator Name* (Picture 18)
	
 	Expose that Volume Group as HYCU iSCSI target (Picture 19). IQN of the iSCSI storage device is located in Nutanix Volume Group properties - Target IQN Prefix (Picture 20)

.. Note :: To get iSCSI Initiator Name, in HYCU from the top right corner, Administration icon, select iSCSI Initiator.
  
.. Note ::	The target will be used to store backups made by HYCU, and it will also be a place restores will be carried out from. Supported targets are:
	- SMB
	
	- NFS
	
	- iSCSI 
	
	- Amazon S3 and S3 Compatible Storage solutions
	
	- Azure

.. figure:: https://s3.us-east-2.amazonaws.com/s3.nutanixtechsummit.com/hycu/images/image12.png

.. figure:: https://s3.us-east-2.amazonaws.com/s3.nutanixtechsummit.com/hycu/images/image13.png

.. figure:: https://s3.us-east-2.amazonaws.com/s3.nutanixtechsummit.com/hycu/images/image14.png

.. figure:: https://s3.us-east-2.amazonaws.com/s3.nutanixtechsummit.com/hycu/images/image15.png

.. figure:: https://s3.us-east-2.amazonaws.com/s3.nutanixtechsummit.com/hycu/images/image16.png

.. figure:: https://s3.us-east-2.amazonaws.com/s3.nutanixtechsummit.com/hycu/images/image17.png

.. figure:: https://s3.us-east-2.amazonaws.com/s3.nutanixtechsummit.com/hycu/images/image18.png

.. figure:: https://s3.us-east-2.amazonaws.com/s3.nutanixtechsummit.com/hycu/images/image19.png

.. figure:: https://s3.us-east-2.amazonaws.com/s3.nutanixtechsummit.com/hycu/images/image20.png

EXERCISE 5 - VM BACKUP
++++++++++++++++++++++

Objective:	Complete a few backup procedures

Prerequisites:	HYCU VM Configured, Nutanix Cluster Added, Backup Target Added

 	Login to the HYCU UI
	
 	Click on “Virtual Machines“ on the left-hand pane.
	
	HYCU synchronizes machines on regular intervals, but you can also trigger synchronization manually by clicking the 	    	    Synchronize button in the top left corner.

	Two types of backups are available.
	
	- VM backup
	
	- Application Aware backup

	This time, we will focus on a full VM backup.

 	Highlight the VM machine you want to backup - As shown in Picture 21, click on “Polices” on the top right, and select 		one of the polices (Picture 22).
	
 	As soon as the policy gets assigned, your first full backup will start, and you can track its status by clicking on 		“Jobs" in the main left-hand pane (Picture 23)
	
 	Once backup completes, if you want to manually trigger an incremental backup, you can start it by clicking on the 		“Backup” button on the top. (Picture 24)
	
 	By hovering your mouse over the backup status column, you can see which type of backup was done, and all of the 		important details of that backup. (Picture 25)

	Congratulations, you've just completed your first HYCU backup!!!

.. figure:: https://s3.us-east-2.amazonaws.com/s3.nutanixtechsummit.com/hycu/images/image21.png

.. figure:: https://s3.us-east-2.amazonaws.com/s3.nutanixtechsummit.com/hycu/images/image22.png

.. figure:: https://s3.us-east-2.amazonaws.com/s3.nutanixtechsummit.com/hycu/images/image23.png

.. figure:: https://s3.us-east-2.amazonaws.com/s3.nutanixtechsummit.com/hycu/images/image24.png

.. figure:: https://s3.us-east-2.amazonaws.com/s3.nutanixtechsummit.com/hycu/images/image25.png


EXERCISE 6 - VM RESTORE
+++++++++++++++++++++++

Objective:	Restore VM and/or file system 

Prerequisites:	HYCU VM Configured, Nutanix Cluster Added, Backup Target Added and you have completed at least one full backup

 	Login to HYCU UI
	
 	Click on “Virtual Machines” in the left hand pane
	
 	Find the one you need to restore either by scrolling through available choices, or by filtering it by name (top right     	  corner, just below the Owner button).
	
 	Highlight the VM you desire. Now all of the restore points related to that VM will appear. Select the restore point you 	desire
	
 	Now click on the “Restore VM” on the menu that appears above.  (Picture 26)
	
 	You can restore the VM to the original location with same name as the original VM, or to a new location or with a new 		name.
	
 	Let’s restore it to a new container. Deselect “Restore with original settings” option
	
 	Select a container where the VM will be restored to
	
 	Specify new VM name
	
 	Deselect “POWER VIRTUAL MACHINE ON” option and trigger Restore
	
	Congratulation's your restore is underway!!  Make sure to monitor the progress.

.. figure:: https://s3.us-east-2.amazonaws.com/s3.nutanixtechsummit.com/hycu/images/image26.png


EXERCISE 7 - FILE / FOLDER LEVEL RESTORE
++++++++++++++++++++++++++++++++++++++++

Objective:	Perform a single file restore 

Prerequisites:	HYCU VM Configured, Nutanix Cluster Added, Backup Target Added, and you have completed at least one full backup

Restore's are available even from the file system level, and it’s extremely useful when you have to restore only a few files/folders from a VM. That way, there is no need to restore the entire VM, but rather just those files/folders. Follow the below steps in order to perform a granular restore.

 	Login to HYCU UI
	
 	Click on “Virtual Machines” in the left hand pane
	
 	Find the VM you want to restore the file or folder from by scrolling through available choices, or by filtering it by     	  name (top right corner, just below the Owner button).
	
 	Select the VM
	
 	To restore files back to the original VM you will need to provide VM credentials. 
	
 	To define and assign credentials for the VM click on “Credentials" in the top right corner.
	
 	- Configure administrator credentials (Picture 27)
	
	- Username = <collected as part of pre-requisites>
	
	- Password = <collected as part of pre-requisites>
	
 	Select your desired Virtual machine, click “Credentials” and assign created credential group to the Virtual machine.
	
 	.. Note :: Notice VM discovery will be marked green if credentials were properly verified and HYCU has access to the 		system.
	
 	Select the VM again and then select the latest restore point and click on “Restore Files”.	
 	By default, you can recover files to any shared location.
	
 	Click on the “Restore files” again. Simply check the boxes next to folders/files needed for restore, and confirm with 	  	  next. (Picture 28)
	
 	Select restore to Original or Alternate location, fill out required information (picture 29) and restore the files.
	
	.. figure:: https://s3.us-east-2.amazonaws.com/s3.nutanixtechsummit.com/hycu/images/image27.png
	
	.. figure:: https://s3.us-east-2.amazonaws.com/s3.nutanixtechsummit.com/hycu/images/image28.png
	
	.. figure:: https://s3.us-east-2.amazonaws.com/s3.nutanixtechsummit.com/hycu/images/image29.png
	
  
  EXERCISE 8 – APPLICATION DISCOVERY & BACKUP / RESTORE 
  +++++++++++++++++++++++++++++++++++++++++++++++++++++
  
  Objective:	Perform auto discovery of a SQL Server database and perform backup & restore
  
Prerequisites:	- SQL Server with a single SQL instance
- Credentials for VM access, and Credentials for SQL database access (sysadmin permission)

HYCU will be able to auto discover applications running inside a VM, and offer application level backup / restore. With this application awareness capability, you can now focus on protecting your applications. Follow the below steps in order to perform an application aware backup / restore

 	Select Virtual Machines in the main left menu (Picture 30)
	
 	Click on Credentials on the right-hand side
	
 	Create new credential group, make sure to use credentials with VM & APP access (Picture 31)
	
 	Now, find the VM with SQL server running on it
	
 	Highlight it with a left mouse click, then click on Credentials
	
 	Assign the proper credentials to that VM. The discovery process will then start automatically 
	
 	Once discovery has completed click on Applications in the main left side menu
	
 	Assign your desired Policy to discovered SQL application, and the backup process will start within 5 minutes (Picture 32)
	
 	Start another backup manually by clicking on the Backup on top, and notice it is an incremental backup
	
 	On the same screen, when you click on the application, you will see all of the application restore point's that are   	 	 available.
	
 	You can select any of these restore point's and select the “Restore” icon to perform a granular recovery of the database
	
 	Select either individual database, multiple databases, or full SQL instance (Picture33)
	
 	Notice that HYCU will offer restore to particular point in time for Databases which are configured in full recovery mode
	
	.. figure:: https://s3.us-east-2.amazonaws.com/s3.nutanixtechsummit.com/hycu/images/image30.png
	
	.. figure:: https://s3.us-east-2.amazonaws.com/s3.nutanixtechsummit.com/hycu/images/image31.png
	
	.. figure:: https://s3.us-east-2.amazonaws.com/s3.nutanixtechsummit.com/hycu/images/image32.png
	
	.. figure:: https://s3.us-east-2.amazonaws.com/s3.nutanixtechsummit.com/hycu/images/image33.png

Conclusions
+++++++++++

- A summary of key points from the lab
- Can include highlighting relevant value proposition
- Or key differentiators

    API Training Shop Blog About 

    © 2018 GitHub, Inc. Help Support 

