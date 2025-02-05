.. _telemetry:

Telemetry
=========

SOC Telemetry
-------------

Starting in Security Onion 2.4.70, :ref:`soc` will send telemetry data to Google Analytics. The purpose of this change is to help the Security Onion development team improve the product. Specifically, by knowing which user-interface features are being used, and how the user interacts with the SOC user interface, the development team can better prioritize new features, improvements to the existing user interface, and begin deprecating features that are rarely used. Deprecating unused features will help developers avoid spending their time and effort maintaining and upgrading areas of the product that aren't widely used. This allows more time to be spent on new features and bug fixes, directly benefiting Security Onion users.

Configuring
~~~~~~~~~~~

During setup, or during non-automated :ref:`soup` invocations, the user must choose to enable or disable SOC telemetry collection.

After installation, grid administrators can enable or disable SOC Telemetry via the configuration interface. Search for ``SOC Telemetry`` in the Configuration screen.

After changing the ``SOC Telemetry`` configuration setting, the grid must be resynchronized. This happens automatically once every 15 minutes, or manually if the grid administrator clicks the Synchronize Grid option, at the top of the Configuration screen. Grid synchronization can take several minutes to complete.

Also, the browser will cache the previous configuration setting. Therefore, to ensure the browser is using the new setting value, make sure grid synchronization completes, and then perform a hard browser refresh on the SOC UI. This can be performed via CTRL+SHIFT+R or CMD+SHIFT+R.

Included Data
~~~~~~~~~~~~~

The primary objective of the SOC Telemetry data collection is to understand what features are being used in :ref:`soc`. Specifically, the data being collected relates to user interface navigation flows. Additional context data, such as the version of the software, the viewed page path, etc. may also be included.

Grid-specific data is intentionally excluded from the data collection, where possible.

Due to the nature of how internet web requests work, the originating IP address and User-Agent information of the web browser as well as host referrer information is known at the time of the data collection.

See the Retention Period section below to learn more about how long this data is retained.

Network Parameters
~~~~~~~~~~~~~~~~~~

The SOC Telemetry data originates from the :ref:`soc` browser running on the analyst workstation. The domains being accessed from the :ref:`soc` browser are:

- ``www.googletagmanager.com``
- ``www.google-analytics.com``

The telemetry data is sent using TLS encryption.

Retention Period
~~~~~~~~~~~~~~~~

Data stored in Google Analytics is configured to be automatically removed after two months. This is the shortest interval that Google Analytics provides.

Operating System Updates Telemetry
----------------------------------

Security Onion periodically checks for package updates to ensure the operating system (OS) and related applications are kept patched and updated. These updates are critical to protecting the OS and installed packages from recently exposed vulnerabilities. This is different from Security Onion product upgrades via :ref:`soup`, which is manually invoked. When the OS package updates begin, the request to the Security Onion repository server(s) can include a limited set of telemetry data.

Configuring
~~~~~~~~~~~

Automatic package updates can be enabled or disabled via the configuration interface. Search for ``patch`` in the Configuration screen.

After changing this configuration setting, the grid must be resynchronized. This happens automatically once every 15 minutes, or manually if the grid administrator clicks the Synchronize Grid option, at the top of the Configuration screen. Grid synchronization can take several minutes to complete.

Included Data
~~~~~~~~~~~~~

The primary objective of the included telemetry data is to understand which versions of Security Onion are deployed and on which platforms. This information helps the Security Onion development team determine how to prioritize support for older versions, and whether there is justification to start testing or stop testing on various operating systems and architectures.

Additional data may be included to provide the development team with information about which features have been enabled. This data can change from release to release as it often relates to new development work.

Also, grids with license keys installed will include the license key identifier. Grids using the standard unprovisioned license do not have a license key identifier.

Due to the nature of how internet web requests work, the originating IP address and User-Agent information of the web browser as well as host referrer information is known at the time of the data collection.

Network Parameters
~~~~~~~~~~~~~~~~~~

The OS Updates Telemetry data originates from the manager node. The domains being accessed from the manager node are:

- ``sigs.securityonion.net``
- ``repo.securityonion.net``
- ``repo-alt.securityonion.net``

The telemetry data is sent using TLS encryption.

Airgap
------

Grids installed within airgapped environments will automatically disable telemetry. In this scenario, the ``SOC Telemetry`` configuration setting will have no effect and the automatic package updates will be disabled. See the :ref:`airgap` section for more information about environments detached from the internet.

.. note::
    
    If a grid is switched from airgap to non-airgap, and if the SOC Telemetry is not explicitly disabled in the grid by an administrator, the SOC app running in the browser will send telemetry.
