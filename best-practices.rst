.. _best-practices:

Best Practices
==============

Security Onion provides lots of options and flexibility, but for best results we recommend the following best practices.

Installation
------------

- download our Security Onion ISO image for the quickest and easiest installation experience (see the :ref:`download` section)

- for production deployments, prefer dedicated hardware to VMs when possible (see the :ref:`hardware` section)

- if VMs must be used, ensure that resources are properly dedicated to VMs to avoid resource contention

- use local storage and avoid NFS, NAS, iSCSI, etc.

- adequately spec your hardware to meet your current usage and allow for growth over time

- prefer taps to span ports when possible

- make sure that any network firewalls have the proper firewall rules in place to allow ongoing operation and updates (see the :ref:`firewall` section)

Configuration
-------------

- make sure that both hostname and IP address are correct during installation

- avoid changing hostname and IP address after installation

- Linux is case sensitive where other OSs might not be, so we recommend using lowercase for things like hostnames, usernames, etc.

Avoid Third Party Software and Modifications
--------------------------------------------

- Security Onion is a free and open platform based on standard Linux distros, but we recommend treating it as an appliance and avoid installing third party software as this may conflict with our components and cause issues when updating

- avoid installing automation tools such as Puppet and Chef as these may conflict with our existing :ref:`salt` automation

- avoid installing monitoring tools such as Zabbix as this may conflict with our existing :ref:`grafana` monitoring

- avoid changing file permissions or umask settings

- hardening guidelines may break functionality, so if you must apply those hardening guidelines, we recommend testing thoroughly before deploying to production

Stay Up To Date
---------------

- join our discussion forum at https://securityonion.net/discuss or subscribe to one of our social media channels to be notified of Security Onion updates

- keep your deployment updated as we frequently fix bugs and add new features

- if possible, test updates on a test deployment before deploying to production
