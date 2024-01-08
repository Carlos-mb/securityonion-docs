.. _strelka:

Strelka
=======

From https://github.com/target/strelka:

    Strelka is a real-time file scanning system used for threat hunting, threat detection, and incident response. Based on the design established by Lockheed Martin's Laika BOSS and similar projects (see: related projects), Strelka's purpose is to perform file extraction and metadata collection at huge scale.

Depending on what options you choose in Setup, it may ask if you want to use :ref:`zeek` or :ref:`suricata` for metadata. Whichever engine you choose for metadata will then extract files from network traffic. Strelka then analyzes those files and they end up in ``/nsm/strelka/processed/``.

Security Onion checks file hashes before sending to Strelka to avoid analyzing the same file multiple times in a 48 hour period.

Alerts
------

Strelka scans files using YARA rules. If it detects a match, then it will generate an alert that can be found in :ref:`alerts`, :ref:`dashboards`, :ref:`hunt`, or :ref:`kibana`. Here is an example of Strelka detecting Poison Ivy RAT:

.. image:: images/strelka-alert-1.png
  :target: _images/strelka-alert-1.png

Drilling into that alert, we find more information about the file and the YARA rule:

.. image:: images/strelka-alert-2.png
  :target: _images/strelka-alert-2.png

.. image:: images/strelka-alert-3.png
  :target: _images/strelka-alert-3.png

.. image:: images/strelka-alert-4.png
  :target: _images/strelka-alert-4.png

You can read more about YARA rules in the :ref:`local-rules` section.

Logs
----

Even if Strelka doesn't detect a YARA match, it will still log metadata about the file. You can find Strelka logs in :ref:`dashboards`, :ref:`hunt`, and :ref:`kibana`. Here's an example of the default Strelka dashboard in :ref:`dashboards`:

.. image:: images/strelka.png
  :target: _images/strelka.png

Configuration
-------------

You can configure Strelka by going to :ref:`administration` --> Configuration --> strelka.

.. image:: images/config-item-strelka.png
  :target: _images/config-item-strelka.png

Diagnostic Logging
------------------

Strelka diagnostic logs are in ``/nsm/strelka/log/``. Depending on what you’re looking for, you may also need to look at the :ref:`docker` logs for the containers:

::

        sudo docker logs so-strelka-backend
        sudo docker logs so-strelka-coordinator
        sudo docker logs so-strelka-filestream
        sudo docker logs so-strelka-frontend
        sudo docker logs so-strelka-manager

More Information
----------------

.. note::

    For more information about Strelka, please see https://github.com/target/strelka.
