.. _virtualbox:

VirtualBox
==========

In this section, we'll cover installing Security Onion on VirtualBox. You can download a copy of VirtualBox for Windows, Mac OS X, or Linux at https://www.virtualbox.org. 

Creating VM
-----------

First, launch VirtualBox and click the "New" button. Provide a name for the virtual machine ("Security Onion" for example) and specify the type ("Linux") and version (Oracle Linux 9.x), then click "Continue." We'll next define how much memory we want to make available to our virtual machine based on the :ref:`hardware` section.

Next, we'll create a virtual hard drive. Specify "Create a virtual hard drive now" then click "Create" to choose the hard drive file type "VDI (VirtualBox Disk Image)" and "Continue." For storage, we have the options of "Dynamically allocated" or "Fixed size." For a client virtual machine, "Dynamically allocated" is the best choice as it will grow the hard disk up to whatever we define as the maximum size on an as needed basis until full, at which point Security Onion's disk cleanup routines will work to keep disk space available. If you happen to be running a dedicated sensor in a virtual machine, I would suggest using "Fixed size," which will allocate all of the disk space you define up front and save you some disk performance early on. Once you've settled on the storage allocation, click "Continue" and provide a name from your hard disk image file and specify the location where you want the disk file to be created if other than the default location. For disk size, you'll want at least 200GB so you have enough capacity for retrieving/testing packet captures and downloading system updates. Click "Create" and your Security Onion VM will be created.

At this point, you can click "Settings" for your new virtual machine so we can get it configured. Mount the Security Onion ISO file so our VM can boot from it to install Linux. Click the "Storage" icon, then under "Controller: IDE" select the "Empty" CD icon. To the right, you'll see "CD/DVD Drive" with "IDE Secondary" specified with another CD icon. Click the icon, then select "Choose a virtual CD/DVD disk file" and browse to where you downloaded the Security Onion ISO file, select it then choose "Open." Next click "Network" then "Adapter 2." You'll need to click the checkbox to enable it then attach it to "Internal Network." Under the "Advanced" options, set "Promiscuous Mode" to "Allow All." Click "Ok" and we are ready to install the operating system.

Hit the "Start" button with your new virtual machine selected and after a few seconds the boot menu will load. 

Follow the installation steps for your desired installation type in the :ref:`installation` section.

.. tip::

  You'll notice two icons on the top right in VirtualBox Manager when you select your virtual machine: Details and Snapshots. Click "Snapshots" then click the camera icon and give your snapshot a name and description. Once we have a snapshot, we'll be able to make changes to the system and revert those changes back to the state we are preserving.

Guest Additions
---------------
| If you want to install VirtualBox Guest Additions, please see:
| https://wiki.centos.org/HowTos/Virtualization/VirtualBox/CentOSguest
