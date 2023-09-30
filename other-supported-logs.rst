.. _other-supported-logs:

Other Supported Logs
====================

We include :ref:`elasticsearch` ingest parsers for several log types that don't have :ref:`elastic-agent` integrations.

Example: pfSense
----------------

Security Onion includes :ref:`elasticsearch` ingest parsers for pfSense firewall logs. To enable this, first go to :ref:`administration` --> Configuration --> firewall --> hostgroups. 

.. image:: images/config-item-firewall.png
  :target: _images/config-item-firewall.png
   
Once there, then select the ``syslog`` option and allow the traffic through the firewall. 
   
Next, configure your pfSense firewall to send syslog to the IP address of your Security Onion box. If you are using pfSense 2.6.0 or higher, make sure that ``Log Message Format`` is set to ``BSD (RFC 3164, default)``. You should then be able to see your firewall logs using the ``Firewall`` query in :ref:`dashboards` or :ref:`hunt`.

Example: RITA
-------------

We include :ref:`elasticsearch` ingest parsers for :ref:`rita` logs. To enable this support, add the following in the relevant :ref:`salt` minion pillar and then restart :ref:`elastic-agent` on the minion(s):

::

   rita:
     enabled: True

This will enable the following :ref:`elastic-agent` inputs:

``/nsm/rita/beacons.csv``

``/nsm/rita/exploded-dns.csv``

``/nsm/rita/long-connections.csv``  

``/nsm/rita/open-connections.csv``  

If you are installing :ref:`elastic-agent` on a non-Security Onion node or your filenames differ, you will need to copy the :ref:`elastic-agent` configuration from ``/opt/so/saltstack/default/salt/elastic-agent/etc/elastic-agent.yml`` to ``/opt/so/saltstack/local/salt/elastic-agent/etc/elastic-agent.yml`` (or modify on the non-Security Onion node in the normal Elastic Agent configuration file) and emulate the path/filename accordingly.

Once ingested into Security Onion, you should be able to search for :ref:`rita` logs in :ref:`dashboards` or :ref:`hunt` using ``event.module:rita | groupby event.dataset``. You should be able to see beacon, connection, and dns logs. If the value for ``beacon.score`` in a ``beacon`` record equals ``1``, an alert will be generated and viewable in :ref:`alerts`.
