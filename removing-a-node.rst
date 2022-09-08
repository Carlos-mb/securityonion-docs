.. _removing-a-node:

Removing a Node
===============

There may come a time when you need to remove a node from your distributed deployment. To do this, you'll need to remove the node's configuration from a few different components.

Salt
----

First, log into your manager and list all :ref:`salt` keys:

::

   sudo salt-key

Then remove the node by deleting its key from :ref:`salt` (replacing ``nodename`` with the actual node name):

::

   sudo salt-key -d nodename

Remove the node from any ``.sls`` files in ``/opt/so/saltstack/local/pillar/data/``.

Grafana
-------

If necessary, remove the node's json file from the appropriate subdirectory under ``/opt/so/conf/grafana/grafana_dashboards/`` on the manager and then restart Grafana with:

:: 

  sudo so-grafana-restart

You may also want to purge old Grafana data using ``so-influxdb-clean`` as described in the :ref:`grafana` section.

SOC
---

To remove the node from the SOC Grid page, make sure the node is powered off and then restart SOC:

::

   sudo so-soc-restart
   
Cross Cluster Search
--------------------

If you are removing a search node, you will want to remove it from cross cluster search. To do so, you'll need to update that search node's settings in ``_cluster/settings`` and make sure that any settings are set to ``null``. So you might want to start by doing the following query via :ref:`so-elasticsearch-query`:

::

   sudo so-elasticsearch-query _cluster/settings
   
Then based on that output, update ``_cluster/settings`` by sending that node section back but with all settings set to ``null``. You could use :ref:`so-elasticsearch-query` again or use :ref:`kibana`'s ``Dev Tools`` and paste something like the following text into the window (replacing ``nodename`` with the actual node name and adding any other settings as necessary):

::

    PUT _cluster/settings
    {
      "persistent": {
        "search": {
          "remote": {
            "nodename": {
              "skip_unavailable": null,
              "seeds":null
            }
          }
        }
      }  
    }


.. seealso::

   | For more information, please see:
   | https://www.elastic.co/guide/en/elasticsearch/reference/current/modules-remote-clusters.html#configure-remote-clusters-dynamic
