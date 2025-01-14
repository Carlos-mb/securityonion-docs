.. _listing-accounts:

Listing Accounts
================

OS
--

Operating System (OS) user accounts are stored in ``/etc/passwd``.  You can get a list of all OS accounts using the following command:

::

  cut -d: -f1 /etc/passwd
  
If you want a list of user accounts (not service accounts), then you can filter ``/etc/passwd`` for accounts with a UID greater than 999 like this:

::

  cat /etc/passwd | awk -F: '$3 > 999 {print ;}' | cut -d: -f1 
  
SOC
---

You can get a list of users in :ref:`soc` by navigating to the :ref:`administration` interface and then clicking ``Users``:

.. image:: images/81_users.png
  :target: _images/81_users.png

To see detail on an individual user account, click the button on the left side of the row to expand the user account:

.. image:: images/82_users_detail.png
  :target: _images/82_users_detail.png

For more information about the Users page, please see the :ref:`administration` section.
