.. _vmware:

VMware
======

Overview
--------

In this section, we'll cover creating a virtual machine (VM) for our Security Onion ISO image in VMware Workstation Pro and VMware Fusion. These steps should be fairly similar for most VMware installations. If you don't already have VMware, you can download VMware Workstation Player from https://www.vmware.com/products/player/playerpro-evaluation.html.

.. note::

   With the sniffing interface in ``bridged`` mode, you will be able to see all traffic to and from the host machine's physical NIC. If you would like to see **ALL** the traffic on your network, you will need a method of forwarding that traffic to the interface to which the virtual adapter is bridged. This can be achieved with a tap or SPAN port.

ESXi
----

If you're using VMware ESXi, then you're likely familiar with VM creation and installation and so we won't detail that here. There are a few things specific to ESXi that you might want to be aware of:

- you may need to set your monitoring interface in the vSwitch to VLAN ID 4095 to allow all traffic through. You can read more about this at https://github.com/Security-Onion-Solutions/securityonion/discussions/7185.

- if you're trying to monitor multiple network interfaces, then you may need to enable the ``Allow MAC Changes`` option at both the vSwitch and Port Group levels. You can read more about this at https://github.com/Security-Onion-Solutions/securityonion/discussions/2676.
   
Workstation Pro
---------------

VMware Workstation is available for many different host operating systems, including Windows and several popular Linux distros. Follow the steps below to create a VM in VMware Workstation Pro for our Security Onion ISO image:

#. From the VMware main window, select File >> New Virtual Machine.
#. Select Typical installation >> Click ``Next``.
#. Installer disc image file >> SO ISO file path >> Click ``Next``.
#. Choose Linux, CentOS 7 64-Bit and click ``Next``.
#. Specify virtual machine name and click ``Next``.
#. Specify disk size (minimum 200GB), store as single file, click ``Next``.
#. Customize hardware and increase Memory and Processors based on the :ref:`hardware` section.
#. Network Adapter (NAT or Bridged -- if you want to be able to access your Security Onion machine from other devices in the network, then choose Bridged, otherwise choose NAT to leave it behind the host) -- in this tutorial, this will be the management interface.
#. Add >> Network Adapter (Bridged) - this will be the sniffing (monitor) interface.
#. Click ``Close``.
#. Click ``Finish``.
#. Power on the virtual machine and then follow the installation steps for your desired installation type in the :ref:`installation` section.

Fusion
------

VMware Fusion is available for Mac OS. For more information about VMware Fusion, please see https://www.vmware.com/products/fusion.html.

Follow the steps below to create a VM in VMware Fusion for our Security Onion ISO image:

#. From the VMware Fusion main window, click ``File`` and then click ``New``.
#. ``Select the Installation Method`` appears. Click ``Install from disc or image`` and click ``Continue``.
#. ``Create a New Virtual Machine`` appears. Click ``Use another disc or disc image...``, select our ISO image, click ``Open``, then click ``Continue``.
#. ``Choose Operating System`` appears. Click ``Linux``, click ``CentOS 7 64-bit``, then click ``Continue``.
#. ``Choose Firmware Type`` appears. Click ``Legacy BIOS`` and then click ``Continue``.
#. ``Finish`` screen appears. Click the ``Customize Settings`` button.
#. ``Save As`` screen appears. Give the VM a name and click the ``Save`` button.
#. ``Settings`` window appears. Click ``Processors & Memory``.
#. ``Processors & Memory`` screen appears. Increase processors and memory based on the :ref:`hardware` section. Click the ``Add Device...`` button.
#. ``Add Device`` screen appears. Click ``Network Adapter`` and click the ``Add...`` button.
#. ``Network Adapter 2`` screen appears. This will be the sniffing (monitor) interface. Select your desired network adapter configuration. Click the ``Show All`` button.
#. ``Settings`` screen appears. Click ``Hard Disk (SCSI)``.
#. ``Hard Disk (SCSI)`` screen appears. Increase the disk size to at least ``200GB`` depending on your use case. Click the ``Apply`` button.
#. Close the ``Settings`` window.
#. At the window for your new VM, click the ``Play`` button to power on the virtual machine.
#. Follow the installation steps for your desired installation type in the :ref:`installation` section.

VMware Tools
------------

If using a graphical desktop, you may want to install ``open-vm-tools-desktop`` to enable more screen resolution options and other features. For example, using our ISO image or standard Rocky Linux 9:

::

   sudo yum install open-vm-tools-desktop

