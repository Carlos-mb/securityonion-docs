.. _cloud-azure:

Azure Cloud Image
=================

Azure users can deploy an official Security Onion virtual machine image found on the Azure Marketplace:
https://securityonion.net/azure

.. warning::

   Existing 2.4 RC1 or newer Security Onion Azure Image installations should use the :ref:`soup` command to upgrade to newer versions of Security Onion. Attempting to switch to a newer image from the Azure Marketplace could cause loss of data and require full grid re-installation. Upgrading from Security Onion 2.3 or beta versions of 2.4 is unsupported.
    
.. note::

   Azure has put on hold their Virtual TAP preview feature, which means in order to install a Security Onion sensor in the Azure cloud you will need to use a packet broker offering from the Azure Marketplace. See more information here: https://docs.microsoft.com/en-us/azure/virtual-network/virtual-network-tap-overview

.. note::

   This section does not cover network connectivity to the Security Onion node. This can be achieved through configuring an external IP for the node’s management interface, or through the use of a VPN connection via OpenVPN. 

.. note::

   This section does not cover how to set up a virtual network in Azure. For more details about setting up a virtual network, please see https://docs.microsoft.com/en-us/azure/virtual-network/. Ensure that all Security Onion nodes can access the manager node over the necessary ports. This could require adding rules to your Azure Virtual Network and/or VMs in order to satisfy the Security Onion :ref:`firewall` Node Communication requirements.

Requirements
############

Before proceeding, determine the grid architecture desired. Choose from a single-node grid versus a distributed, multi-node grid. 

Security Onion recommends using either Premium SSD disks, or the more expensive Ultra SSD disks, with suitable IOPS and throughput matched to your expected network monitoring requirements.

Single Node Grid
----------------

For simple, low-volume production monitoring, a single node grid can be used.

Listed below are the minimum suggested single-node instance quantities, sizes, and storage requirements for either standalone or evaluation installations (choose one, not both). Note that when using virtual machines with the minimum RAM requirements you may need to enable memory swapping.

Standalone:

- Quantity: 1
- Type: Standard_D4as_v4
- Storage: 200GB Premium SSD

Evaluation

- Quantity: 1
- Type: Standard_D8as_v4
- Storage: 200GB Premium SSD
  
Distributed Grid
----------------

For high volume production monitoring, choose a multi-node grid architecture. At least two search nodes are recommended for redundancy purposes.

Listed below are the minimum suggested distributed grid instance quantities, sizes, and storage requirements. Prefer increasing VM memory over enabling swap memory, for best performance. High volume networks will need more powerful VM types with more storage than those listed below.

VPN Node

- Quantity: 1
- Type: Option 1: Standard_B1s - Lower cost for use with low vpn traffic volume
- Type: Option 2: Standard_D4as_v4 w/ accelerated networking - Higher cost for high vpn traffic volume
- Storage: 64GB Premium SSD
  
Manager

- Quantity: 1
- Type: Standard_D4as_v4
- Storage: 256GB Premium SSD
  
Search Nodes

- Quantity: 2 or more
- Type: Standard_D4as_v4
- Storage: 256GB Premium SSD
  
Sensor monitoring the VPN ingress

- Quantity: 1
- Type: Standard_D4as_v4
- Storage: 512GB Premium SSD

Create Monitoring Interface 
###########################

To setup a Security Onion sensor node in Azure, follow the prerequisite steps below prior to creating the sensor VM.

Create a Security Group for Sniffing Interface 
----------------------------------------------

Security Groups act like a firewall for your Azure virtual machines, controlling both inbound and outbound traffic. You should consider whether a security group is needed for your virtual network, and specifically for the interface that you will be using to sniff the traffic.  This security group will need to be as open as possible to ensure all traffic destined to the sniffing interface will be allowed through.  To create a security group, follow these steps:

- In the Azure Dashboard search for: ``Network security groups``.
- Select: ``Create``
- Provide a name, such as ``so-monitoring-security-group``.
- Select the appropriate resource group and region. 
- Select ``Review + Create``
- Review the summary
- Select: ``Create``
- Select: ``Go to resource``
- Adjust the Inbound security rules to ensure that all incoming monitoring traffic is allowed.

Create Sniffing Interface
-------------------------

Prior to launching the Security Onion sensor virtual machine you will need to create the interface that will be used to monitor your virtual network.  This interface will be attached to the Security Onion sensor virtual machine as a secondary interface.  To create a sniffing interface, follow these steps:

- In the Azure Dashboard search for: ``Network interfaces``. 
- Select: ``Create``
- Provide a name, such as ``so-monitoring-interface``.
- Choose the resource group, region, virtual network, subnet, security group from the steps above, and IP settings.
- Select: ``Review + Create``
- Review the summary
- Select: ``Create``

Create Security Onion Instances
###############################

Instance Creation
-----------------

To configure a Security Onion instance (repeat for each node in a distributed grid), follow these steps:

- In the Azure Dashboard search for: ``Virtual machines``
- Select: ``Create`` and then ``Virtual machine``
- Choose or create a new Resource group.
- Enter a suitable name for this virtual machine, such as ``so-vm-manager``.
- Choose the desired Region and Availability options. (Use ``East US 2`` for Ultra SSD support, if needed.)
- Choose the ``Security Onion 2 VM Image``. If this option is not listed on the Image dropdown, select ``See all images`` and search for ``onion``.
- Choose the appropriate Size based on the desired hardware requirements. For assistance on determining resource requirements please review the Requirements section above.
- Change the Username to ``onion``. Note that this is not mandatory -- if you accidentally leave it to the default ``azureuser``, that's ok, you'll simply use the ``azureuser`` username any place where the documentation states to use the ``onion`` username.
- Select an existing SSH public key if one already exists, otherwise select the option to ``Generate new key pair``.
- Choose ``Other`` for Licensing type.
- Select ``Next: Disks``
- Ensure ``Premium SSD`` is selected.
- For single-node grids, distributed sensor nodes, or distributed search nodes: If you would like to separate the ``/nsm`` partition into its own disk, create and attach a data disk for this purpose, with a minimum size of 100GB, or more depending on predicted storage needs. Note that the size of the ``/nsm`` partition determines the rate that old packet and event data is pruned. Separating the /nsm partition can provide more flexibility with scaling up the grid node sizes, but requires a little more setup, which is described later.
- Select ``Next: Networking``
- Choose the virtual network for this virtual machine.
- Choose a public IP if you intend to access this virtual machine directly (not recommended for production grids).
- Choose appropriate security group settings. Note that this is typically not the same security group used for the sensor monitoring interface.
- Accelerated networking will be automatically enabled if the virtual machine size supports it.
- Select: ``Review + create``
- Review the summary. If a ``Validation failed`` message appears, correct the missing inputs under each tab section containing a red dot to the right of the tab name.
- Select. ``Create`` and download the new public key, if you chose to generate a new key.
- If this VM is a single-node grid, or is sensor node:

   - Stop the new VM after deployment completes.
   - Edit the VM and attach the monitoring network interface created earlier.
   - Start the VM.

Note that you'll need to reference the SSH public key when using SSH to access the new VMs. For example:

::

    chmod 600 ~/Downloads/onion.pem
    ssh -i ~/Downloads/onion.pem onion@11.22.33.44

Manager Setup
#############

After SSH'ing into the node, setup will begin automatically. Follow the prompts, selecting the appropriate install options. Most distributed installations will use the ``hostname`` or ``other`` web access method, due to the need for both cluster nodes inside the private network, and analyst across the public Internet to reach with the manager. This allows for custom DNS entries to define the correct IP (private vs public) depending on whether its a cluster node or an analyst user.

Search Node Setup
#################

Follow standard Security Onion search node installation, answering the setup prompts as applicable. 

Remote Sensor Setup
###################

Setup the VPN (out of scope for this guide) and connect the sensor node to the VPN.
When prompted to choose the management interface, select the VPN tunnel interface, such as ``tun0``. Use the internal IP address of the manager inside Azure when prompted for the manager IP.

Azure Sensor Setup
##################

SSH into the sensor node and run through setup to set this node up as a sensor. Choose ``eth0`` as the main interface and ``eth1`` as the monitoring interface.

.. note::

   Azure has put on hold their Virtual TAP preview feature, which means in order to install a Security Onion sensor in the Azure cloud you will need to use a packet broker offering from the Azure Marketplace. See more information here: https://docs.microsoft.com/en-us/azure/virtual-network/virtual-network-tap-overview

Verify Monitoring Traffic
-------------------------

To verify the Azure sensor is receiving the correct data on the sniffing interface run the following command on the Security Onion Azure sensor instance:

::

    sudo tcpdump -nni eth1 


To verify :ref:`zeek` is properly decapsulating and parsing the traffic you can verify logs are being generated in the ``/nsm/zeek/logs/current`` directory:

::

    ls -la /nsm/zeek/logs/current/
