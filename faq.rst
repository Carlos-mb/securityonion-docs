.. _faq:

FAQ
===

| 
| 
| `Install / Update / Upgrade <#install-update-upgrade>`__\ 
| `Users / Passwords <#users-passwords>`__\ 
| `Support / Help <#support-help>`__\ 
| `IDS engines <#ids-engines>`__\ 
| `Security Onion internals <#security-onion-internals>`__\ 
| `Tuning <#tuning>`__\ 
| `Miscellaneous <#miscellaneous>`__\ 
| 
| 

Install / Update / Upgrade
--------------------------

Why won't the ISO image boot on my machine?
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Please see the :ref:`trouble-booting` section.

What's the recommended procedure for installing Security Onion?
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Please see the :ref:`installation` section.

What languages are supported?
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

We only support the English language at this time.

How do I install Security Onion updates?
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Please see the :ref:`soup` section.

What connectivity does Security Onion need to stay up to date?
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Please see the :ref:`firewall` section.

What do I need to do if I'm behind a proxy?
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Please see the :ref:`proxy` section.

Can I run Security Onion on Raspberry Pi or some other non-x86 box?
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

No, we only support x86-64 (standard Intel/AMD 64-bit architectures). Please see the :ref:`hardware` section.

`back to top <#top>`__

Users / Passwords
-----------------

What is the password?
~~~~~~~~~~~~~~~~~~~~~

Please see the :ref:`passwords` section.

How do I add a new user account?
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Please see the :ref:`adding-accounts` section.\ 
 
`back to top <#top>`__

Support / Help
--------------

Where do I send questions/problems/suggestions?
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Please see the :ref:`community-support` section.

Is commercial support available for Security Onion?
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Yes, we offer commercial support at https://securityonionsolutions.com.

`back to top <#top>`__
 
IDS engines
-----------

Can Security Onion run in ``IPS`` mode?
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

No, Security Onion does not support blocking traffic. Most organizations have some sort of Next Generation Firewall (NGFW) with IPS features and that is the proper place for blocking to occur. Security Onion is designed to monitor the traffic that makes it through your firewall.

`back to top <#top>`__

Security Onion internals
------------------------

Where can I read more about the tools contained within Security Onion?
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Please see the :ref:`tools` section.

What's the directory structure of ``/nsm``?
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Please see the :ref:`directory` section.

Why does Security Onion use ``UTC``?
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Please see the :ref:`timezones` section.

Why are the ``timestamps`` in Kibana not in UTC?
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Please see the :ref:`timezones` section.

Why is my disk filling up?
~~~~~~~~~~~~~~~~~~~~~~~~~~

In general, Security Onion attempts to make use of as much disk space as you give it. Depending on installation type, it should continue writing data to disk until disk usage reaches 80-90% at which point it should begin purging old data. Most disk space is used by :ref:`elasticsearch` or full packet capture written to disk via :ref:`stenographer` or :ref:`suricata`.

How is my data kept secure?
~~~~~~~~~~~~~~~~~~~~~~~~~~~

Standard network connections to or from Security Onion are encrypted. This includes SSH, HTTPS, :ref:`elasticsearch` network queries, and :ref:`salt` minion traffic. Endpoint agent traffic is encrypted where supported. This includes the :ref:`elastic-agent` which supports encryption with additional configuration. SOC user account passwords are hashed via bcrypt in Kratos and you can read more about that at https://github.com/ory/kratos.

`back to top <#top>`__

Tuning
------

How do I configure email for alerting and reporting?
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Please see the :ref:`email` section.

How do I configure a ``BPF``?
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Please see the :ref:`bpf` section.

How do I filter traffic?
~~~~~~~~~~~~~~~~~~~~~~~~

Please see the :ref:`bpf` section.

How do I exclude traffic?
~~~~~~~~~~~~~~~~~~~~~~~~~

Please see the :ref:`bpf` section.

What are the default firewall settings and how do I change them?
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Please see the :ref:`firewall` section.

What do I need to modify in order to have the log files stored on a different mount point?
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Please see the :ref:`new-disk` section.

 `back to top <#top>`__

Miscellaneous
-------------

Where can I find interesting pcaps to replay?
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Please see the :ref:`pcaps` section.

Why is Security Onion connecting to an IP address on the Internet over port 123?
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Please see the :ref:`ntp` section.

Should I backup my Security Onion box?
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Security Onion automatically backs up some important configuration as described in the :ref:`backup` section. However, there is no automated data backup. Network Security Monitoring as a whole is considered "best effort". It is not a "mission critical" resource like a file server or web server. Since we're dealing with "big data" (potentially terabytes of full packet capture) of a transient nature, backing up the data would be prohibitively expensive. Most organizations don't do any data backups and instead just rebuild boxes when necessary.

How can I add local rules?
~~~~~~~~~~~~~~~~~~~~~~~~~~

Please see the :ref:`detections` section.

Can I connect Security Onion to Active Directory or LDAP?
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
We understand the appeal of integrating with directory services like Active Directory and LDAP, but we typically recommend against joining any security infrastructure (including Security Onion) to directory services. The reason is that when you get an adversary inside your network, one of their first goals is going to be gaining access to that directory. If they get access to the directory, then they get access to everything connected to the directory. For that reason, we recommend that all security infrastructure (including Security Onion) be totally separate from directory services.

`back to top <#top>`__
