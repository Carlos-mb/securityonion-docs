.. _grafana:

Grafana
=======

:ref:`soc` includes a link on the sidebar that takes you to the Grafana app to see system health information. You can also access Grafana from the :ref:`grid` page.

.. image:: images/grafana-0.png
  :target: _images/grafana-0.png

You will start on the ``Security Onion Grid Overview`` dashboard. Depending on what kind of deployment you have, there will be at least one more dashboard available if you click the Dashboards icon on the left and then click ``Manage``. You can then drill into the ``Dashboards`` folder to see all available dashboards.

.. image:: images/grafana-1.png
  :target: _images/grafana-1.png

Data
----

Grafana will have both high-resolution data and downsampled low-resolution data. Some Grafana graphs have dotted lines that show previous data that has been downsampled. High-resolution data will be purged after 30 days, leaving just the downsampled low-resolution data.

You may want to update your Grafana data as shown below. If you have a distributed deployment, you will run all commands on the manager.

If you want to remove some old data prior to downsampling, you can run ``so-influxdb-clean``. This is optional and not required. ``so-influxdb-clean`` will ask how many days or weeks worth of data you want to retain and remove all data older than that.

If you want to downsample all data, run ``so-influxdb-downsample``. This process could take a while depending on system resources and the amount of data that needs to be downsampled. For each measurement, the script will go day by day starting at 7/21/20 and downsample that day's data from the ``autogen`` retention policy into the ``so_long_term`` retention policy.

Once the downsampling is complete and you verify the downsampled data is available in Grafana (other than Processes, Disk I/O, Memory), you can remove the old data and free up space. The ``so-influxdb-drop-autogen`` script will remove the autogen retention policy and thus remove the old data that we previously downsampled.

Accounts
--------

By default, you will be viewing Grafana as an anonymous user. If you want to make changes to the default Grafana dashboards, you will need to log into Grafana with username ``admin`` and the randomized password found via ``sudo salt-call pillar.get secrets``.

Configuration
-------------

You can modify Grafana configuration by going to :ref:`administration` --> Configuration --> grafana. You can modify influxdb configuration by going to :ref:`administration` --> Configuration --> influxdb.

Example
-------
If you want to configure and enable SMTP for Grafana, place the following in the ``global.sls`` file. 
If you have files referenced in the config file, those can be placed in ``/opt/so/saltstack/default/salt/grafana/etc/files/``.
Those files will be then be placed in ``/opt/so/conf/grafana/etc/files`` on the minion and mapped to ``/etc/grafana/config/files/`` within the container.

::

  grafana:
    config:
      smtp:
        enabled: true
        host: smtphost.mydomain:25
        user: myuser
        # If the password contains # or ; you have to wrap it with triple quotes wrapped by single quotes. Ex '"""#password;"""'
        password: mypassword
  #      cert_file: /etc/grafana/config/files/smtp_cert_file.crt
  #      key_file: /etc/grafana/config/files/smtp_key_file.key
  #      skip_verify: false
        from_address: admin@grafana.localhost
        from_name: Grafana
  #      ehlo_identity: dashboard.example.com

More Information
----------------

.. seealso::

  Check out our Grafana Alarms video at https://youtu.be/8FmZ4MRe8Uk.

  For more information about Grafana, please see https://grafana.com/.
