.. _alert-data-fields:

Alert Data Fields
=================

| :ref:`elasticsearch` receives :ref:`nids` alerts from :ref:`suricata` via :ref:`elastic-agent` or :ref:`logstash` and parses them using:
| ``/opt/so/conf/elasticsearch/ingest/suricata.alert``
| ``/opt/so/conf/elasticsearch/ingest/common.nids``
| ``/opt/so/conf/elasticsearch/ingest/common``

You can find these online at:

https://github.com/Security-Onion-Solutions/securityonion/blob/2.4/main/salt/elasticsearch/files/ingest/suricata.alert

https://github.com/Security-Onion-Solutions/securityonion/blob/2.4/main/salt/elasticsearch/files/ingest/common.nids

https://github.com/Security-Onion-Solutions/securityonion/blob/2.4/main/salt/elasticsearch/files/ingest-dynamic/common

You can find parsed :ref:`nids` alerts in :ref:`alerts`, :ref:`dashboards`, :ref:`hunt`, and :ref:`kibana` via their predefined queries and dashboards or by manually searching for:

| ``event.module:"suricata"``
| ``event.dataset:"alert"``

Those alerts should have the following fields:

| ``source.ip``
| ``source.port``
| ``destination.ip``
| ``destination.port``
| ``network.transport``
| ``rule.gid``
| ``rule.name``
| ``rule.rule``
| ``rule.rev``
| ``rule.severity``
| ``rule.uuid``
| ``rule.version``
