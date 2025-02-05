.. _hardware:

Hardware Requirements
=====================

The :ref:`architecture` section should have helped you determine how many machines you will need for your deployment. This section will help you determine what kind of hardware specs each of those machines will need.

CPU Architecture
----------------

Security Onion only supports x86-64 architecture (standard Intel or AMD 64-bit processors).

.. warning::

   We do not support ARM or any other non-x86-64 processors!

Minimum Specs
-------------


================       ====== ===== ========= ======   
 Node Type              CPUs   RAM   Storage   NICs  
================       ====== ===== ========= ======     
Import                    2    4GB    50GB      1
Eval                      4    8GB    200GB     2
Standalone                4    16GB   200GB     2
Manager                   4    16GB   200GB     1
ManagerSearch             8    16GB   200GB     1
Search node               4    16GB   200GB     1
Sensor                    4    12GB   200GB     2
Heavy node                4    16GB   200GB     2
IDH node                  2    1GB    12GB      1
Fleet node                4    4GB    200GB     1
Receiver node             2    8GB    200GB     1
================       ====== ===== ========= ======   

.. warning::

   Please note these are the absolute bare minimum requirements. Your requirements may increase drastically as you enable more services, monitor more traffic, and consume more logs. For more information, please see the detailed sections below.

Import
------

An Import installation runs the minimal processes required to import PCAP or EVTX files and view the results. As such, it has the lowest hardware requirements as shown in the table above. You can read more about Import in the :ref:`first-time-users` section.

Eval
----

An Eval installation runs the minimal processes required for a single machine to sniff live network traffic from a TAP or span port and view the results. Therefore, its hardware requirements are higher than Import as shown in the table above. Eval is designed for temporary installations or homelab installations on a budget. Unlike a full Standalone installation, Evaluation is NOT designed for production usage.

In order to minimize RAM usage, Eval does not run :ref:`logstash` or :ref:`redis` at all. Also, Eval defaults to using :ref:`suricata` for writing full packet capture to disk (instead of :ref:`stenographer`).

Production Deployments
----------------------

For best results, we recommend purchasing new hardware that meets the hardware requirements detailed below.

.. tip::

   If you're planning to purchase new hardware, please consider official Security Onion appliances from Security Onion Solutions (https://securityonionsolutions.com). Our custom appliances have already been designed for certain roles and traffic levels and have Security Onion pre-installed. Purchasing from Security Onion Solutions will save you time and effort **and** help to support development of Security Onion as a free and open platform!

Storage
-------

We only support local storage. Remote storage like SAN/iSCSI/FibreChannel/NFS increases complexity and points of failure, and has serious performance implications. You may be able to make remote storage work, but we do not provide any support for it. By using local storage, you keep everything self-contained and you don't have to worry about competing for resources. Local storage is usually the most cost efficient solution as well.

NIC
---

You'll need at least one wired network interface dedicated to management (preferably connected to a dedicated management network). We recommend using static IP addresses where possible.

If you plan to sniff network traffic from a tap or span port, then you will need one or more interfaces dedicated to sniffing (no IP address). The installer will automatically disable NIC offloading functions such as ``tso``, ``gso``, and ``gro`` on sniffing interfaces to ensure that :ref:`suricata` and :ref:`zeek` get an accurate view of the traffic.

Make sure you get good quality network cards, especially for sniffing. Most users report good experiences with Intel cards. 

Security Onion is designed to use wired interfaces. You may be able to make wireless interfaces work, but we don't recommend or support it.

UPS
---

As with most computer systems, you'll want to avoid power outages or other ungraceful shutdowns. Please consider a UPS (Uninterruptible Power Supply) for production deployments.

Elastic Stack
-------------

We recommend placing all Elastic storage (/nsm/elasticsearch) on SSD or fast spinning disk in a RAID 10 configuration.

Please see the :ref:`architecture` section for detailed deployment scenarios.

Standalone Deployments
----------------------

In a standalone deployment, the manager components and the sensor components all run on a single box so your hardware requirements will reflect that. You'll need at minimum 16GB RAM, 4 CPU cores, and 200GB storage. At the bare minimum of 16GB RAM, you will need swap space to avoid issues. We recommend a minimum of 24GB of RAM if you plan on monitoring even a small amount of network traffic. More network traffic means higher hardware requirements.

This deployment type is recommended for evaluation purposes, POCs (proof-of-concept) and small to medium size single sensor deployments. Although you can deploy Security Onion in this manner, it is recommended that you separate the backend components and sensor components.

- CPU: Used to parse incoming events, index incoming events, search metatadata, capture PCAP, analyze packets, and run the frontend components. As data and event consumption increases, a greater amount of CPU will be required.
- RAM: Used for :ref:`logstash`, :ref:`elasticsearch`, disk cache for Lucene, :ref:`suricata`, :ref:`zeek`, etc. The amount of available RAM will directly impact search speeds and reliability, as well as ability to process and capture traffic.
- Disk: Used for storage of indexed metadata. A larger amount of storage allows for a longer retention period. It is typically recommended to retain no more than 30 days of hot :ref:`elasticsearch` indices.

Please refer to the :ref:`architecture` section for detailed deployment scenarios.

Manager node with local log storage and search
----------------------------------------------

In an enterprise distributed deployment, a manager node will store logs from itself and forward nodes. It can also act as a syslog destination for other log sources to be indexed into :ref:`elasticsearch`. An enterprise manager node should have 8 CPU cores at a minimum, 16-128GB RAM, and enough disk space (multiple terabytes recommended) to meet your retention requirements.

- CPU: Used to parse incoming events, index incoming events, and search metadata. As consumption of data and events increases, more CPU will be required.
- RAM: Used for :ref:`logstash`, :ref:`elasticsearch`, and disk cache for Lucene. The amount of available RAM will directly impact search speeds and reliability.
- Disk: Used for storage of indexed metadata. A larger amount of storage allows for a longer retention period. It is typically recommended to retain no more than 30 days of hot :ref:`elasticsearch` indices.

Please refer to the :ref:`architecture` section for detailed deployment scenarios.

Manager node with separate search nodes
---------------------------------------

This deployment type utilizes search nodes to parse and index events. As a result, the hardware requirements of the manager node are reduced. An enterprise manager node should have at least 4-8 CPU cores, 16GB RAM, and 200GB to 1TB of disk space. Many folks choose to host their manager node in their VM farm since it has lower hardware requirements than sensors but needs higher reliability and availability.

- CPU: Used to receive incoming events and place them into :ref:`redis`. Used to run all the front end web components and aggregate search results from the search nodes.
- RAM: Used for :ref:`logstash` and :ref:`redis`. The amount of available RAM directly impacts the size of the :ref:`redis` queue.
- Disk: Used for general OS purposes and storing :ref:`kibana` dashboards.

Please refer to the :ref:`architecture` section for detailed deployment scenarios.

Search Node
-----------

Search nodes increase search and retention capacity with regard to :ref:`elasticsearch`. These nodes parse and index events, and provide the ability to scale horizontally as overall data intake increases. Search nodes should have at least 4-8 CPU cores, 16-64GB RAM, and 200GB of disk space or more depending on your logging requirements.

- CPU: Used to parse incoming events and index incoming events. As consumption of data and events increases, more CPU will be required.
- RAM: Used for :ref:`logstash`, :ref:`elasticsearch`, and disk cache for Lucene. The amount of available RAM will directly impact search speeds and reliability.
- Disk: Used for storage of indexed metadata. A larger amount of storage allows for a longer retention period. It is typically recommended to retain no more than 30 days of hot :ref:`elasticsearch` indices.

Please refer to the :ref:`architecture` section for detailed deployment scenarios.

Forward Node (Sensor)
---------------------

A forward node runs sensor components only, and forwards metadata to the manager node. All PCAP stays local to the sensor, and is accessed through use of an agent.

- CPU: Used for analyzing and storing network traffic. As monitored bandwidth increases, a greater amount of CPU will be required. See below.
- RAM: Used for write cache and processing traffic.
- Disk: Used for storage of PCAP and metadata. A larger amount of storage allows for a longer retention period.

Please refer to the :ref:`architecture` section for detailed deployment scenarios.

Heavy Node (Sensor with Elasticsearch components)
-------------------------------------------------

A heavy node runs all the sensor components AND Elastic components locally. This dramatically increases the hardware requirements. In this case, all indexed metadata and PCAP are retained locally. When a search is performed through :ref:`kibana`, the manager node queries this node's :ref:`elasticsearch` instance. You'll need at minimum 16GB RAM, 4 CPU cores, and 200GB storage. At the bare minimum of 16GB RAM, you will need swap space to avoid issues. We recommend a minimum of 24GB of RAM if you plan on monitoring traffic. The more traffic you plan on monitoring this RAM requirement will also increase.

- CPU: Used to parse incoming events, index incoming events, and search metadata. As monitored bandwidth (and the amount of overall data/events) increases, a greater amount of CPU will be required.
- RAM: Used for :ref:`logstash`, :ref:`elasticsearch`, and disk cache for Lucene. The amount of available RAM will directly impact search speeds and reliability.
- Disk: Used for storage of indexed metadata. A larger amount of storage allows for a longer retention period. It is typically recommended to retain no more than 30 days of hot :ref:`elasticsearch` indices.

Please refer to the :ref:`architecture` section for detailed deployment scenarios.

Receiver Node
-------------

Since receiver nodes only run :ref:`logstash` and :ref:`redis`, they don't require much CPU or disk space. However, more RAM means you can set a larger queue size for :ref:`redis`.

Intrusion Detection Honeypot (IDH) Node
---------------------------------------

For an :ref:`idh` node, the overall system requirements are low: 1GB RAM, 2 CPU cores, 1 NIC, and 100GB disk space.

Sensor Hardware Considerations
------------------------------

The following hardware considerations apply to sensors. If you are using a heavy node or standalone deployment type, please note that it will dramatically increase CPU/RAM/Storage requirements.

Virtualization
~~~~~~~~~~~~~~

We recommend dedicated physical hardware (especially if you're monitoring lots of traffic) to avoid competing for resources. Sensors can be virtualized, but you'll have to ensure that they are allocated sufficient resources.

CPU
~~~

:ref:`suricata` and :ref:`zeek` are very CPU intensive. The more traffic you are monitoring, the more CPU cores you'll need. A very rough ballpark estimate would be 200Mbps per :ref:`suricata` worker or :ref:`zeek` worker. So if you have a fully saturated 1Gbps link and are running :ref:`suricata` for :ref:`nids` alerts and :ref:`zeek` for metadata, then you'll want at least 5 :ref:`suricata` workers and 5 :ref:`zeek` workers. This means you'll need at least 10 CPU cores for :ref:`suricata` and :ref:`zeek` with additional CPU cores for :ref:`stenographer` and/or other services. If you are monitoring a high amount of traffic and/or have a small number of CPU cores, you might consider using :ref:`suricata` for both alerts and metadata. This eliminates the need for :ref:`zeek` and allows for more efficient CPU usage.

RAM
~~~

RAM usage is highly dependent on several variables:

-  the services that you enable
-  the **kinds** of traffic you're monitoring
-  the **actual amount of traffic** you're monitoring (example: you may be monitoring a 1Gbps link but it's only using 200Mbps most of the time)
-  the amount of packet loss that is "acceptable" to your organization

For best performance, over provision RAM so that you can fully disable swap.

The following RAM estimates are a rough guideline and assume that you're going to be running :ref:`suricata`, :ref:`zeek`, and :ref:`stenographer` (full packet capture) and want to minimize/eliminate packet loss. Your mileage may vary!

- If you just want to quickly evaluate Security Onion in a VM, the bare minimum amount of RAM needed is 12GB. More is obviously better!
- If you're deploying Security Onion in production on a small network (100Mbps or less), you should plan on 16GB RAM or more. Again, more is obviously better!
- If you're deploying Security Onion in production to a medium network (100Mbps - 1000Mbps), you should plan on 16GB - 128GB RAM or more.
- If you're deploying Security Onion in production to a large network (1000Mbps - 10Gbps), you should plan on 128GB - 256GB RAM or more.
- If you're buying a new server, go ahead and max out the RAM (it's cheap!). As always, more is obviously better!

Storage
~~~~~~~

Sensors that have full packet capture enabled need LOTS of storage. For example, suppose you are monitoring a link that averages 50Mbps, here are some quick calculations: 50Mb/s = 6.25 MB/s = 375 MB/minute = 22,500 MB/hour = 540,000 MB/day. So you're going to need about 540GB for one day's worth of pcaps (multiply this by the number of days of pcap you want to keep). The more disk space you have, the more PCAP retention you'll have for doing investigations after the fact. Disk is cheap, get all you can!

Packets
~~~~~~~

You'll need some way of getting packets into your sensor interface(s). If you're just evaluating Security Onion, you can replay :ref:`pcaps`. For a production deployment, you'll need a SPAN/monitor port on an existing switch or a dedicated TAP. We recommend dedicated TAPs where possible. If collecting traffic near a NAT boundary, make sure you collect from inside the NAT boundary so that you see the true internal IP addresses.

Inexpensive tap/span options (listed alphabetically):

- `Dualcomm <https://www.dualcomm.com/collections/network-tap>`_
- `Midbit SharkTap <https://www.midbittech.com>`_
- `Mikrotik <https://mikrotik.com/product/RB260GS>`_
- `Netgear GS105Ev2 <https://www.netgear.com/support/product/GS105Ev2>`_

Enterprise Tap options (listed alphabetically):

-  `APCON <https://www.apcon.com/products>`__
-  `Arista <https://www.arista.com/>`__
-  `cPacket <https://cpacket.com>`__
-  `Garland <https://www.garlandtechnology.com/products>`__
-  `Gigamon <https://gigamon.com>`__
-  `KeySight / Ixia / Net Optics <https://www.keysight.com/us/en/cmp/2020/network-visibility-network-test.html>`__
-  `Profitap <https://www.profitap.com>`__

Further Reading
~~~~~~~~~~~~~~~

.. note::

   For large networks and/or deployments, please also see https://github.com/pevma/SEPTun.
