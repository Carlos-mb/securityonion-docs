.. _elastalert:

ElastAlert
==========

From https://elastalert2.readthedocs.io/en/latest/elastalert.html#overview:

    ElastAlert is a simple framework for alerting on anomalies, spikes, or other patterns of interest from data in Elasticsearch.

ElastAlert queries :ref:`elasticsearch` and provides an alerting mechanism with multiple output types, such as Slack, Email, JIRA, OpsGenie, and many more.

Configuration
-------------

You can modify ElastAlert configuration by going to :ref:`administration` --> Configuration --> elastalert.

.. image:: images/config-item-elastalert.png
  :target: _images/config-item-elastalert.png

ElastAlert Rules
----------------

ElastAlert rules are stored in ``/opt/so/rules/elastalert/``.

By default, ElastAlert rules are configured with an output type of ``debug``, which simply outputs to a log file found in ``/opt/so/log/elastalert/``.

ElastAlert logs to :ref:`elasticsearch` indices. You can search those indices in :ref:`dashboards`, :ref:`hunt`, or :ref:`kibana`. :ref:`soc` does not automatically search the ``elastalert`` indices by default so if you want to use :ref:`dashboards` or :ref:`hunt` to search ElastAlert logs, then you'll need to adjust the ``es_index_patterns`` setting in your :ref:`salt` pillar to include ``*:elastalert*``:

::

    es_index_patterns: '*:so-*,*:endgame-*,*:elastalert*'

Slack
~~~~~

To have ElastAlert send alerts to something like Slack, we can simply change the alert type and details for a rule like so:

::

    alert:
    - "slack":
        slack_webhook_url: "https://hooks.slack.com/services/YOUR_WEBHOOK_URI"

Email - Internal
~~~~~~~~~~~~~~~~

To have ElastAlert send to email, we could do something like the following:

::

    alert:
    - "email"
    email:
    - "youremail@yourcompany.com"
    smtp_host: "your_company_smtp_server"
    smtp_port: 25
    from_addr: "elastalert@yourcompany.com"

Email - External
~~~~~~~~~~~~~~~~

If we need to use an external email provider like Gmail, we can add something like the following:

::

    alert:
    - "email"
    email:
    - "youremail@gmail.com"
    smtp_host: "smtp.gmail.com"
    smtp_port: 465
    smtp_ssl: true
    from_addr: "youremail@gmail.com"
    smtp_auth_file: '/opt/elastalert/rules/smtp_auth_file.txt'

Then create a new file called ``/opt/so/rules/elastalert/smtp_auth_file.txt`` and add the following:

::

    user: youremail@gmail.com
    password: yourpassword   

MISP
~~~~

Please see the :ref:`misp` section.

so-elastalert-create
~~~~~~~~~~~~~~~~~~~~

``so-elastalert-create`` is a tool created by `Bryant Treacle <https://github.com/bryant-treacle/so-elastalert-create>`__ that can be used to help ease the pain of ensuring correct syntax and creating Elastalert rules from scratch. It will walk you through various questions, and eventually output an Elastalert rule file that you can deploy in your environment to start alerting quickly and easily.

so-elastalert-test
~~~~~~~~~~~~~~~~~~~~

``so-elastalert-test`` is a wrapper script originally written by Bryant Treacle for ElastAlert's ``elastalert-test-rule`` tool.  The script allows you to test an ElastAlert rule and get results immediately. Simply run ``so-elastalert-test``, and follow the prompt(s).

.. note::

    ``so-elastalert-test`` does not yet include all options available to ``elastalert-test-rule``.

Defaults
~~~~~~~~

With Security Onion's example rules, Elastalert is configured by default to only count the number of hits for a particular match, and will not return the actual log entry for which an alert was generated.

This is governed by the use of ``use_count_query: true`` in each rule file.

If you would like to view the data for the match, you can simply remark this line in the rule file(s). Keep in mind, this may impact performance negatively, so testing the change in a single file at a time may be the best approach.

Timeframe
~~~~~~~~~

Keep in mind, for queries that span greater than a minute back in time, you may want to add the following fields to your rule to ensure searching occurs as planned (for example, for 10 minutes):

::

    buffer_time:   
        minutes: 10   

``allow_buffer_time_overlap: true``

| https://elastalert2.readthedocs.io/en/latest/ruletypes.html#buffer-time
| https://github.com/Yelp/elastalert/issues/805

Diagnostic Logging
------------------

Elastalert diagnostic logs are in ``/opt/so/log/elastalert/``. Depending on what you’re looking for, you may also need to look at the :ref:`docker` logs for the container:

::

	sudo docker logs so-elastalert

More Information
----------------

.. note::

    For more information about ElastAlert, please see https://elastalert2.readthedocs.io/en/latest/.
