.. role:: raw-html-m2r(raw)
   :format: html

Administration
==============

License Management
------------------

Login to ASGARD, navigate to ``Licensing``, click 
``Upload ASGARD Management Center License`` and upload a valid license. 


.. figure:: ../images/install-a-license.png
   :target: ../_images/install-a-license.png
   :alt: image-20200608152010548

   Install a license

After uploading, the license details are displayed.

System Status
-------------

The initial system status page provides a summary of the most important system components. 

It also includes the current resource consumption (disk, CPU and memory) and lists the currently installed ASGARD software version along with available versions of THOR. Additionally, the connection status to the update servers, MASTER ASGARD and Cockpit are shown with a graph that shows asset connections and asset streams.

.. note::
   The THOR version numbers may be missing in a new installation. THOR is not included in the installed packages. THOR is downloaded automatically after the installation and should show up not later than one hour after installation. 


.. figure:: ../images/overview.png
   :target: ../_images/overview.png
   :alt: image-20200608152043704

   Overview

The logs section shows the latest and most relevant logs. Complete logs can be found at ``/var/lib/nextron/asgard2/log``


.. figure:: ../images/logs-section.png
   :target: ../_images/logs-section.png
   :alt: image-20200608152111447

   Logs Section

ASGARD Agent Deployment
-----------------------

In order to register a new endpoint to the ASGARD Management Center, download and install the ASGARD agent on the system you want to register. 

The ASGARD agent can be downloaded from ASGARD directly through the button ``Download ASGARD Agent``. A list of available agents for various operating systems appears. 


.. figure:: ../images/download-asgard-agent.png
   :target: ../_images/download-asgard-agent.png
   :alt: image-20200608152814596

   Download ASGARD Agent


.. figure:: ../images/agents-overview.png
   :target: ../_images/agents-overview.png
   :alt: image-20200608152828507

   Agents Overview

After installation, the endpoints will connect to ASGARD, register automatically and appear in the Asset Management Section in the tab ``Requests``. Please allow two or three minutes for systems to show up. The agents use the hostname to connect to ASGARD, ensure that your endpoints can resolve and reach the ASGARD hostname.

In the requests tab, select the agents you want ASGARD to manage and click ``Accept``. After that, the endpoint shows up in the asset tab and is now ready to be managed or scanned.


.. figure:: ../images/accepting-asgard-agent-requests.png
   :target: ../_images/accepting-asgard-agent-requests.png
   :alt: image-20200608152952182

   Accepting ASGARD Agent Requests

A registered agent will poll to the ASGARD Management Center at a given interval between 10 seconds and 600 seconds – depending on the number of connected endpoints (see :ref:`chapter 6.1 Performance Tuning <usage/commandline:Performance Tuning>` for details). If ASGARD has scheduled a task for the endpoint (for example: run THOR scan) it will be executed directly after the poll.

Asset Management
----------------

Overview
^^^^^^^^

Management of all endpoints registered with ASGARD can be performed in Asset Management. The assets will be presented as a table with an individual ASGARD ID, their IP addresses and host names.


.. figure:: ../images/asset-view.png
   :target: ../_images/asset-view.png
   :alt: image-20200608153056012

   Asset View

By clicking the control buttons in the Actions column, you can start a new scan, run a response playbook, open a command line or switch the endpoints ping rate to a few seconds instead of a maximum of 10 minutes. 

.. figure:: ../images/available-actions.png
   :target: ../_images/available-actions.png
   :alt: Asset Actions

   Available Actions (left to right): Run Scan, Run Task, Connect To Remote Console, Show Timeline, Enable/Disbale Fast Poll Mode

.. note::

    * The internal ping between the ASGARD agent and ASGARD is based on HTTPS not ICMP
    * Depending on the user's role some of the control buttons may be disabled

Column Visibility
^^^^^^^^^^^^^^^^^

Users can select various columns and adjust their view according to their needs.

.. figure:: ../images/available-columns-in-asset-management.png
   :target: ../_images/available-columns-in-asset-management.png
   :alt: Asset Columns

   Available columns in Asset Management

Asset Labels
^^^^^^^^^^^^

Labels are used to group assets. These groups can then be used in scans or tasks. 

You can add multiple labels to an asset or a group of assets. This is done by selecting the particular assets in the left column, typing the label name (e.g. New_Label) and clicking the blue ``Add Labels`` button. 

.. note::
   Don't use labels with white space characters as it could cause issues in syncs with Analysis Cockpit, exports / imports or other underlying legacy functions. 

.. figure:: ../images/add-labels.png
   :target: ../_images/add-labels.png
   :alt: Asset Labling

   Add labels

In order to remove labels, select your assets, click the yellow ``Remove Labels`` button and type the name of the label you want to remove for these assets.

.. figure:: ../images/remove-labels.png
   :target: ../_images/remove-labels.png
   :alt: Asset Labling

   Remove labels

The asset management section has extensive filtering capabilities, e.g. it is easy to select only Linux endpoints that have been online today and have a particular label assigned. 

Export Asset List 
~~~~~~~~~~~~~~~~~

The Import/Export Section allows you to export your assets to a .csv file. 

Import Labels
~~~~~~~~~~~~~

The import function allows you to add or remove labels on assets based on columns in that CSV file. 

The import function processes the values in the columns ``Add Labels ...`` and ``Remove Labels ...`` only. In order to change labels, use the already exported list, add values in these columns and re-import it by using the 
``Apply Labels from CSV`` button. Separate multiple labels with comma. Leading or ending white space characters will be stripped from the labels. 

.. figure:: ../images/asset-label-import.png
   :target: ../_images/asset-label-import.png
   :alt: Asset Labling via CSV

   Asset Labling via CSV

Scan Control
------------

Managing Scan Templates
^^^^^^^^^^^^^^^^^^^^^^^

Scan templates are the most convenient way to make use of THOR's rich set of scan options. Starting with ASGARD 1.10., it is possible to define scan parameters for THOR 10 and store them in different templates for later use in single scans and grouped scans. 

Imagine you want to use dedicated scan options for different system groups (e.g. Linux Servers, Domain Controllers, Workstations, etc.) and make sure to use exactly the same set of scan options every time you scan a particular group of systems. With ASGARD you can now add a scan template for every group.

A popular use case for scan templates is providing additional resource control – for example telling THOR to set the lowest process priority for itself and never use more that 50% of a single CPU. 

Please keep in mind, that we have already optimized THOR to use the most relevant scan options for a particular system (based on type, numbers of CPUs and system resources) and a comprehensive resource control is enabled by default. 

For more details please refer to the `THOR manual <https://thor-manual.nextron-systems.com/en/latest/>`_. Only use the scan templates if you want to deviate from the default for a reason.

Scan templates are protected from being modified by ASGARD users without the "Manage Scan Templates" - permission and can also be restricted from being used by ASGARD users in case the flag "ForceStandardArgs" is set for this user. (see user management section for details).

.. figure:: ../images/scan-templates-overview.png
   :target: ../_images/scan-templates-overview.png
   :alt: image-20200608153256353

   Scan Templates Overview

In order to create a scan template, navigate to "Scan Control" > "Scan Templates" and click the "Add Scan Template" button. The "Add Scan Template" dialogue appears. After choosing a scanner you will find the most frequently used options on the top of this page in the "Favorite Flags" category. You can view all THOR options by clicking on the other categories.

.. figure:: ../images/add-scan-template.png
   :target: ../_images/add-scan-template.png
   :alt: image-20200608153228887

   Add Scan Template

After choosing a scanner you will find the most frequently used options on the top of this page in the "Favorite Flags" category. View all THOR options by clicking on the other categories. By clicking on the star symbols you can also edit your favorites. 

.. figure:: ../images/scan-flags.png
   :target: ../_images/scan-flags.png
   :alt: image-20200608153228887

   Scan Flags

By checking the "Default" box, you can make this scan template the default template for every new scan. Not Checking the "Enforce" box restricts this scan template from being used by any ASGARD user with the "ForceStandardArgs" restriction set. After clicking the "Add" button on the bottom of the template page, an overview of all existing scan templates is shown. 

.. figure:: ../images/image39.png
   :target: ../_images/image39.png
   :alt: image-20200608153244186

   Restricting Scan Templates

By clicking the ``Import Scan Template`` button and choosing a file you can import a scan template.

Scan a Single System
^^^^^^^^^^^^^^^^^^^^

Create a Single Scan
~~~~~~~~~~~~~~~~~~~~

The creation of a scan is performed within the Asset Management. There is a button for each asset to create a new scan and to show all past scans. 

Just click on the "THOR" button in the Action column in the Asset Management view.

.. figure:: ../images/scan-control-scan-creation.png
   :target: ../_images/scan-control-scan-creation.png
   :alt: image-20200608153403808

   Scan Control - Scan Creation

Within this form, you can choose the max. runtime, module, scanner, scan flags, signatures and template can be selected.

After the desired parameters have been set, the scan can be started by clicking the ``Add Task`` button.

Stopping a Single Scan
~~~~~~~~~~~~~~~~~~~~~~

To stop a single scan, navigate to the "Single Scans" tab in Scan Control section and click the "stop" (square) button for the scan you want to stop.

.. figure:: ../images/stopping-a-single-scan.png
   :target: ../_images/stopping-a-single-scan.png
   :alt: image-20200608153951250

   Stopping a Single Scan

Download Scan Results 
~~~~~~~~~~~~~~~~~~~~~

After the scan completion, you can download the scan results via the download button in the actions column.

The download button has the following options: 

* Download THOR Log (the text log file)
* Download HTML Report (as \*.gz compressed file; available for successful scans only)
* Show HTML Report (opens another tab with the HTML report)

.. figure:: ../images/download-scan-results.png
   :target: ../_images/download-scan-results.png
   :alt: Download Scan Results

   Scan Control - Download Scan Results

Scan Groups of Systems
^^^^^^^^^^^^^^^^^^^^^^

Create Grouped Scans
~~~~~~~~~~~~~~~~~~~~

A scan for a group of systems can be created in the "Scan Control > Group Scans" tab. Click the ``Add Group Scan`` button in the upper right corner.

.. figure:: ../images/scan-control-create-group-scan.png
   :target: ../_images/scan-control-create-group-scan.png
   :alt: image-20200608154115029

   Scan Control – Create Group Scan

As with the single scans, various parameters can be set. Aside from the already mentioned parameters, the following parameters can be set:

**Description**

Freely selectable name for the group scan.

**Asset Labels**

Here you can define which assets will be affected by the group scan. In case more than one label is chosen: An asset must have at least one chosen label attached to it to be affected by the scan. If no label is selected, all known assets will be scanned.

**Limit** 

ASGARD will not send additional scans to the agents when the client limit is reached. Therefore you need to set a limit higher than the number of hosts you want to scan. If you are using MASTER ASGARD, this limit is applied on each single selected ASGARD.

**Rate**

The number of scans per minute that a scan should run. This is where the network load can be controlled. Additionally, it is recommended to use this parameter in virtualized and oversubscribed environments in order to limit the number of parallel scans on your endpoints.

**Expires**

After this time frame, no scan orders will be issued to the connected agents. 

**Scheduled Start**

Select a date for a scheduled start of the scan.

After the group scan has been saved or saved and started, you will automatically be forwarded to the list of grouped scans. 

List of all Group Scans
~~~~~~~~~~~~~~~~~~~~~~~

The list of all group scans contains, among other items, the unique Scan-ID and the name.

.. figure:: ../images/scan-control-group-scans-list.png
   :target: ../_images/scan-control-group-scans-list.png
   :alt: image-20200608154224747

   Scan Control – Group Scans – List

In addition, information can be found about the chosen scanner, the chosen parameters, the start and completion times and the affected assets (defined by labels). Additional columns can be added by clicking on "Column Visibility".

The Status field can have the following values: 

**Started:** Scan is started, ASGARD will issue scans with the given parameters

**Stopped:** No additional scan jobs are being issued. All single scans that are currently running will continue to do so.

**Completed:** The group scan is completed. No further scan jobs will be issued.

Starting a Group Scan
~~~~~~~~~~~~~~~~~~~~~

A group scan can be started by clicking on the "play" button in the "Actions" column of a group scan. Subsequently, the scan will be listed as "Started".

Starting a Scheduled Group Scan
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

The Scheduled Group Scan section shows all scans that are to run on a frequent basis along with their periodicity. All group scans that have been started through the scheduler will show up on top of the Group Scan section the moment they are started. New scheduled tasks can be created by clicking the ``Add Scheduled Group Scan`` button.

.. figure:: ../images/scan-control-scheduled-group-scan.png
   :target: ../_images/image49.png
   :alt: image-20200608154452406

   Scan Control – Scheduled Group Scan 

.. figure:: ../images/scan-control-new-scheduled-group-scan.png
   :target: ../_images/image48.png
   :alt: image-20200608154442195

   Scan Control – New Scheduled Group Scan 

Details of a Group Scan
~~~~~~~~~~~~~~~~~~~~~~~

Further information about a group scan can be observed from the detail page of the group scan. Click the scan you are interested in and the details section will appear.

.. figure:: ../images/scan-control-group-scans-details.png
   :target: ../_images/scan-control-group-scans-details.png
   :alt: image-20200608154545029

   Scan Control – Group Scans – Details

Aside from information about the group scan in the "Details" tab, there is a graph that shows the number of assets started and how many assets have already completed the scan in the "Charts" tab. In the "Tasks" tab you get information about the scanned assets.

Integrating Custom IOCs
^^^^^^^^^^^^^^^^^^^^^^^

The tab "IOC management" gives you the opportunity to easily integrate custom signatures into your scans. 

In order to create your own custom IOC Group, navigate to ``Scan Control`` > ``IOC Management`` > ``IOC Groups``
and click ``Add IOC Group`` in the upper right corner. Select a name, a description and a ruleset for your IOC Group.

.. figure:: ../images/add-ioc-group.png
   :target: ../_images/add-ioc-group.png
   :alt: image-20200608160335401

   Add IOC Group

To add IOCs to this group, just click the entry in the table and two blue buttons will show up. You can click the ``Import IOCs`` button to import your own signatures in any of THOR’s IOC formats (e.g. files for keyword IOCs, YARA Files and SIGMA files). Refer to the 
`THOR manual (custom signatures) <https://thor-manual.nextron-systems.com/en/latest/usage/custom-signatures.html>`_ for a complete list and file formats. Browse to the file you want to add and click upload. This adds your IOC file to the default ruleset. 

.. figure:: ../images/import-iocs.png
   :target: ../_images/import-iocs.png
   :alt: image-20200608160335401

   Import IOCs

However, you can also click the ``Add IOC(s)`` button to add some IOCs manually. Select the type, score and description, enter some values and click the ``Add IOC`` button.

.. figure:: ../images/add-ioc.png
   :target: ../_images/add-ioc.png
   :alt: image-20200608160335401

   Add IOCs

You can add those IOC Groups to Rulesets which can be generated in the ``Scan Control`` > ``IOC Management`` > ``Ruleset`` tab by clicking the 
``Add Ruleset`` button in the upper right corner. Select name and description and click the 
``Add Ruleset`` button.

.. figure:: ../images/add-ruleset.png
   :target: ../_images/add-ruleset.png
   :alt: image-20200608160335401

   Add Ruleset

After that click on an entry in the table to expand this entry. There you get information about all IOC Groups which have been added to this ruleset. Additionally you can add or remove IOC Groups by clicking one of the three buttons shown below.

.. figure:: ../images/add-remove-ioc-group.png
   :target: ../_images/add-remove-ioc-group.png
   :alt: image-20200608160335401

   Buttons to Add/Remove IOC Groups

Those rulesets can be selected in the "Custom Signature" field while creating a new scan job. If a ruleset is selected, the scan will include all custom IOCs included in IOC Groups which have been added to this ruleset. You can also select more than one ruleset.

.. figure:: ../images/select-ruleset.png
   :target: ../_images/select-ruleset.png
   :alt: image-20200608160335401

   Select Ruleset while creating a scan job

Please note, ASGARD does not provide a syntax check for your IOC files. Should THOR be unable to parse your IOC files for the scan, THOR will skip the particular file with syntax issues and send an error message in the scan log. All other files with correct syntax will be used for scanning. THOR will report files that can be parsed and are used for scanning in the scan log. 

Integrating IOCs through MISP
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

ASGARD provides an easy to use interface for integrating IOCs from a connected MISP into THOR scans. In order to add rules from a MISP, navigate to "Scan Control > MISP Signatures > Events", select the IOCs and add them to the desired ruleset by using the button in the upper right corner. 

Contrary to the custom IOC handling, there is no default ruleset for MISP. You must create at least one ruleset (see tab "MISP Rulesets") before you can add MISP rules.


.. figure:: ../images/misp-events.png
   :target: ../_images/misp-events.png
   :alt: image-20200608160546503

   MISP events 

In order to create a ruleset, click ``Add MISP Ruleset`` in the "Scan Control > MISP Signatures > Rulset" tab. Select a name and the type of IOCs you want to use in this ruleset. By default, all types are selected, but there may be reasons for deselecting certain categories. For example, filename IOCs tend to cause false positives and may be deselected for that reason. The picture below shows the dialogue for adding a MISP ruleset.

.. figure:: ../images/addon-a-new-misp-rulset.png
   :target: ../_images/addon-a-new-misp-rulset.png
   :alt: image-20200608160621066

   Adding a new MISP ruleset

In order to use a MISP ruleset in a scan: Add the ruleset in the ``MISP Signatures`` field when creating your scan.


.. figure:: ../images/adding-a-misp-rulset-to-a-scan.png
   :target: ../_images/adding-a-misp-rulset-to-a-scan.png
   :alt: image-20200608160636062

   Adding a MISP Ruleset to a Scan 

Response Control
----------------

Opening a Remote Shell on an endpoint
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

In order to open a remote shell on an endpoint, open the Asset Management section and click the "command line" button in the Actions column.

.. figure:: ../images/opening-a-remote-shell-from-the-asset-view.png
   :target: ../_images/opening-a-remote-shell-from-the-asset-view.png
   :alt: image-20200608154926650

   Opening a Remote Shell from the Asset View

Depending on your configuration it may take between 10 seconds and 10 minutes for the remote shell to open. Please note that all actions within the remote shell are recorded and can be audited. All shells open with root privileges or system privileges.

.. figure:: ../images/remote-shell.png
   :target: ../_images/remote-shell.png
   :alt: image-20200608154959812

   Remote Shell

In order to replay a remote console session, navigate to `Response Control`, select the task that represents your session and click the play button. 

.. figure:: ../images/replay-remote-shell-session.png
   :target: ../_images/replay-remote-shell-session.png
   :alt: image-20200608155013219

   Replay Remote Shell Session

ASGARD users can only see their own remote shell session. Only users with the `RemoteConsoleProtocol` permission are able to replay all sessions from all users.

Response Control with pre-defined playbooks
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

In addition to controlling THOR scans, ASGARD Management Center contains extensive response functions. Through ASGARD, you can start or stop processes, modify and delete files or registry entries, quarantine endpoints, collect triage packages and execute literally any command on connected systems. All with one click and executed on one endpoint or groups of endpoints.

It is also possible to download specific suspicious files. You can transfer a suspicious file to the ASGARD Management Center and analyze it in a Sandbox. 


.. figure:: ../images/built-in-playbooks.png
   :target: ../_images/built-in-playbooks.png
   :alt: image-20200608155058550

   Built-in Playbooks

To execute a predefined response action on a single endpoint, navigate to the Asset Management view and click the "play" button in the Actions Column. This will lead you to a dialogue where you can select the desired action. 

.. figure:: ../images/execute-playbook-on-single-endpoint.png
   :target: ../_images/execute-playbook-on-single-endpoint.png
   :alt: image-20200608155132686

   Execute Playbook on Single Endpoint

In this example, we collect a full triage package.

ASGARD ships with pre-defined playbooks for the following tasks:


* Collect full triage pack (Windows only)
* Isolate endpoint (Windows only)
* Collect system memory
* Collect file
* Collect directory
* Execute command and collect stdout and stderr

Nextron provides additional playbooks via ASGARD updates.

.. warning::
    The collection of memory can set the systems under  high load and impacts the systems response times during the transmission of  collected files. Consider all settings carefully!   Also be aware that memory dumps may fail due to  kernel incompatibilities or conflicting security mechanisms. Memory dumps  have been successfully tested on all supported Windows operating systems with  various patch levels. The memory collection on Linux systems depends on  kernel settings and loaded modules, thus we cannot guarantee a successful  collection.   Additionally, memory dumps require temporary free  disk space on the system drive and consume a significant amount of disk space  on ASGARD as well. The ASGARD agent checks if there is enough memory on the  system drive and adds a 50% safety buffer. If there is not enough free disk  space, the memory dump will fail.  

Response Control for Groups of Systems
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Response functions for groups of systems can be defined in the "Group Tasks"` tab or the "New Scheduled Group Task" tab.

.. figure:: ../images/execute-playbook-on-group-of-endpoints.png
   :target: ../_images/execute-playbook-on-group-of-endpoints.png
   :alt: image-20200608155449158

   Execute Playbook on Group of Endpoints

Response Control with custom playbooks
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

You can add your own custom playbook by clicking the ``Add Playbook`` button in the 
"Response Control > Playbooks" tab. 

.. figure:: ../images/add-custom-playbook.png
   :target: ../_images/add-custom-playbook.png
   :alt: image-20200608160106096

   Add Custom Playbook

This lets you define a name and a description for your playbook. After clicking the ``Add Playbook`` button, 
click on your new playbook and start adding steps by clicking the ``Add Step`` button.


.. figure:: ../images/add-playbook-entry.png
   :target: ../_images/add-playbook-entry.png
   :alt: image-20200608160150424

   Add Playbook Entry

You can have up to 16 entries in each playbook that are executed in a row. Every entry can be either "download something from ASGARD to the endpoint", "execute a command line" or "Upload something from the endpoint to ASGARD". If you run a command line the stdout and stderr are reported back to ASGARD. 


Service Control
---------------

Service Control is ASGARD's way of deploying real-time services on endpoints.

Service Controller Installation
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

To install asgard2-service-controller on an asset you need to install the asgard2-agent first. If you already have installed asgard2-agent on an asset and accepted it in ASGARD, you can use the **"Install ASGARD Service Controller"** playbook to deploy the service controller on an asset or you can manually download and execute the asgard2-service-controller installer from the ASGARD downloads page.

.. figure:: ../images/sc-install.png
   :target: ../_images/sc-install.png
   :alt: Install Service Controller

   Install Service Controller

Service Controller Update
^^^^^^^^^^^^^^^^^^^^^^^^^

If an ASGARD update comes with a new service controller version, you need to update the service controller on the already rolled-out assets. You can do this using an "Update Agent" task. For a single asset the task can be run in ``Asset Management`` > ``Assets`` > ``Run Task`` (play button action) or analogous as a (scheduled) group task under ``Response Control`` > ``(Scheduled) Group Tasks`` > ``Add (Scheduled) Group Task``.

.. figure:: ../images/sc-update.png
   :target: ../_images/sc-update.png
   :alt: Update Service Controller

   Update Service Controller

.. note::
    If you don't see the **Update Agent** module, you need to enable **Show Advanced Tasks** in ``Settings`` > ``Advanced``



LogWatcher Service
^^^^^^^^^^^^^^^^^^

The LogWatcher real-time service monitors the Windows Event Log using predefined rules in the Sigma format and creates an alert that is forwarded to ASGARD Analysis Cockpit if a match was found.

What is Sigma
~~~~~~~~~~~~~

From the `project website <https://github.com/SigmaHQ/sigma>`_:

    Sigma is a generic and open signature format that allows you to describe relevant log events in a straightforward manner. The rule format is very flexible, easy to write and applicable to any type of log file. The main purpose of this project is to provide a structured form in which researchers or analysts can describe their once developed detection methods and make them shareable with others.

    Sigma is for log files what `Snort <https://www.snort.org/>`_ is for network traffic and `YARA <https://github.com/VirusTotal/yara>`_ is for files.

Prerequisites
~~~~~~~~~~~~~

In order to make full use of ASGARD LogWatcher you need a Windows Audit Policy and Sysmon, both with a reasonable configuration, in place. We expect organizations to take care of providing a sane configuration by their own. This section helps in giving starting points, if needed.

Windows Audit Policy
""""""""""""""""""""

The default audit policy of Windows is not suitable for security monitoring and needs to be configured. There are Microsoft recommendations available `online <https://docs.microsoft.com/en-us/windows-server/identity/ad-ds/plan/security-best-practices/audit-policy-recommendations>`_.

Also auditing the command line for process creation events should be enabled. Documentation for that task is available `here <https://docs.microsoft.com/en-us/windows-server/identity/ad-ds/manage/component-updates/command-line-process-auditing>`_.

Sysmon Configuration Template
"""""""""""""""""""""""""""""

There are some best practise configurations available. See them as a good starting point to develop your own configuration. If you do not have a Sysmon configuration yet, there are several options we suggest:

1. The Nextron Systems fork of SwiftOnSecurity's `sysmon-config <https://github.com/Neo23x0/sysmon-config>`_
2. The `SwiftOnSecurity sysmon-config <https://github.com/SwiftOnSecurity/sysmon-config>`_
3. Olaf Hartong's `sysmon-modular <https://github.com/olafhartong/sysmon-modular>`_

In general we suggest our own configuration, as we test our rules with it and include changes from the upstream configuration. But depending on your preferences, either of those listed configurations is a good starting point for writing your own configuration.

.. warning::
    Do not deploy those configurations to your production environment without prior testing.

    It is expected that some tools you use will be the source of huge log volume and should be tuned in the configuration depending your environment.

Sysmon Installation
"""""""""""""""""""

`Sysmon <https://docs.microsoft.com/en-us/sysinternals/downloads/sysmon>`_ is part of Microsoft Sysinternals and therefore has to be installed as a third party tool. The preferred way to distribute Sysmon and its configuration is using your organization's device management. If you do not have access to one, you can use ASGARD's playbook feature to distribute Sysmon and update its configuration. Documentation which describes the playbook creation and that offers maintenance scripts can be found in our `asgard-playpooks repository <https://github.com/NextronSystems/asgard-playbooks>`_.

Operation
~~~~~~~~~
This chapter explains how to configure LogWatcher using Sigma rules.

Overview
""""""""

Under ``Service Control`` -> ``Services`` the Overview of all Assets with an installed service controller is shown. Clicking on the entry opens a drop-down menu with details and additional information.

.. figure:: ../images/sc-overview.png
   :target: ../_images/sc-overview.png
   :alt: Assets with installed Service Controller

   Assets with installed Service Controller

Enable Service for an Asset
"""""""""""""""""""""""""""
To enable the LogWatcher service for an asset, navigate to ``Service Control`` > ``Services``, select the asset's checkbox and choose ``Change Configuration``. Then choose the desired service configuration by clicking ``Use``.

.. figure:: ../images/sc-change-configuration.png
   :target: ../_images/sc-change-configuration.png
   :alt: Enable a Service Configuration

   Enable a Service Configuration

If you have not configured a service yet, you need to do so beforehand.

Creating a Service Configuration
""""""""""""""""""""""""""""""""

A service configuration is used to group assets of similar type and assign them a set of rules (in form of rulesets). 

Go to ``Service Control`` > ``Service Configurations`` > ``Create Service Configuration``, enter a name, choose the **LogWatcher** service type and add the rulesets that should apply for this service configuration (i.e. group of assets).

.. figure:: ../images/sc-service-configuration.png
   :target: ../_images/sc-service-configuration.png
   :alt: Create a Service Configuration

   Create a Service Configuration

If you have not configured a ruleset yet, you need to do so beforehand.

Creating a Ruleset
""""""""""""""""""

Rulesets are used to group rules to manageable units. As an asset can only have one service configuration, rulesets are used to determine which rules are used in which service configuration. To create a ruleset go to ``Service Control`` > ``Sigma: Rulesets`` > ``Create Ruleset``. 

.. figure:: ../images/sc-create-ruleset.png
   :target: ../_images/sc-create-ruleset.png
   :alt: Create a Ruleset

   Create a Ruleset

After creating a ruleset, go to ``Service Control`` > ``Sigma: Rules`` to choose the rules that should be added to this ruleset by selecting the checkboxes and then ``Add to Rulesets``. A rule can be assigned to multiple rulesets.

.. figure:: ../images/sc-add-to-ruleset.png
   :target: ../_images/sc-add-to-ruleset.png
   :alt: Add a Rule to Rulesets

   Add a Rule to Rulesets

.. note::
    You need to commit and push your changes after editing a ruleset. ASGARD has to restart the service controller to read new configurations. In order to prevent multiple restarts in the case of a user performing several configuration changes in succession, the user has to initiate the reloading of the new configuration by going to ``Service Control`` > ``Sigma: Rulesets`` and performing the **Commit and Push** action (gear wheels). The need for committing and pushing is indicated in the *Uncommitted Changes* column.

    .. figure:: ../images/sc-uncommitted-changes.png
       :target: ../_images/sc-uncommitted-changes.png
       :alt: Uncommitted Changes Indicator
    
       Uncommitted Changes Indicator

Choosing which Rules to activate
""""""""""""""""""""""""""""""""

It is not advised to enable all available rules on an asset. We suggest to start with all "critical" rules. Only then advancing to the "high" rules. "Medium" rules should not be enabled in bulk or "low"/"informational" at all . Single medium rules, which increase an organisation's detection coverage and do not trigger a bigger number of false positives can be added to the active configuration, but should be tested rule by rule.

In order to easily add rules to a ruleset you can use the column filters to select the desired rules and add the bulk to a ruleset. As an example you can add all rules of level "critical" to a ruleset:

    .. figure:: ../images/sc-choose-rules1.png
       :target: ../_images/sc-choose-rules1.png
       :alt: Add all critical rules to a ruleset
    
       Add All Critical Rules to a Ruleset

Another great way to pivot the sigma rule database is the usage of MITRE ATT&CK® IDs.

    .. figure:: ../images/sc-choose-rules2.png
       :target: ../_images/sc-choose-rules2.png
       :alt: Search by MITRE ATT&CK® ID
    
       Search by MITRE ATT&CK® ID

Or you can just search the title or description field of the rules (the description column is not shown by default and has to be added using the ``Columns`` button).

    .. figure:: ../images/sc-choose-rules3.png
       :target: ../_images/sc-choose-rules3.png
       :alt: Search by Rule Title or Description
    
       Search by Rule Title or Description
       
Adding Custom Rules
"""""""""""""""""""

Custom rules can be added using the sigma format complying with the `specification <https://github.com/SigmaHQ/sigma/wiki/Specification>`_. You can upload single files or a ZIP compressed archive. This can be done at ``Service Control`` > ``Sigma: Rules`` > ``Add Rules``.

    .. figure:: ../images/sc-custom-rule.png
       :target: ../_images/sc-custom-rule.png
       :alt: Adding Custom Rules
    
       Adding Custom Rules

Evidence Collection 
-------------------

Collected Evidences
^^^^^^^^^^^^^^^^^^^

ASGARD provides two forms of collected evidence: 

1. Playbook output (file or memory collection, command output)
2. Sample quarantine (sent by THOR via Bifrost protocol during the scan)

All collected evidence can be downloaded in the "Collected Evidence" section.

.. figure:: ../images/collected-evidence-list.png
   :target: ../_images/collected-evidence-list.png
   :alt: Collected Evidence List

   Collected Evidence List

Bifrost Quarantine
^^^^^^^^^^^^^^^^^^

If Bifrost is used with your THOR scans, all collected samples show up here. You will need the "ResponseControl" permission in order to view or download the samples. See section ``User Roles`` within the ``User Management`` section for details.


.. figure:: ../images/bifrost-collections.png
   :target: ../_images/bifrost-collections.png
   :alt: image-20200608160703244

   Bifrost Collections 

Generate Download Links
-----------------------

The ``Downloads`` section lets you create and download a full THOR package including scanner, custom IOCs and MISP rulesets along with a valid license for a specific host. This package can then be used for systems that cannot be equipped with an ASGARD agent for some reason. For example, this can be used on air gapped networks. Copy the package to a USB stick or a CD ROM and use it where needed.


.. figure:: ../images/download-thor-package.png
   :target: ../_images/download-thor-package.png
   :alt: Generate THOR Package Download Link

   Download THOR package and license workstation named 'myhost123'

While selecting different options in the form, the download link changes.

After you have generated a download tokane and have selected the correct scanner, operating system and target hostname (not FQDN), you can copy the download link and use it to retrieve a full scanner package including a license file for that host. These download links can be sent to administrators or team members that don’t have access to ASGARD management center. Remember that the recipients of that link still need to be able to reach ASGARD’s web server port (443/tcp). The token can be used to download THOR or a THOR license whithout an ASGARD account. Attention: If you disable the token, anybody can download THOR from this ASGARD or can generate licenses.

.. note::
   The scanner package will not contain a license file if you don’t set a hostname in the ``Target Hostname`` field. If you have an Incident Response license, you must provide it separately.


Use Case 1 - Share th URL without Hostname
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

You can generate download links without an included license by leaving the `hostname` field empty. A valid license (e.g. "Incident Response") must be  placed in the program folder after the download and extraction. 

Use Case 2 - Share th URL with Hostname
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

By including the hostname in the form, a license will be generated and included in the download package You can copy the final download link and send it to anyone, who can use this link to download a package and run scans on a host with that name.

You or the recipient can change the name in that URL to make it usable on other systems.

Note that you may have to adjust the `type` field to get the correct license type (`client` for workstations, `server` for servers) and the THOR version (`win`, `linux`, `osx`) to generate a correct URL. 

.. code:: bash
   
   .../thor10-win?hostname=mywinserver1&type=server...
   .../thor10-win?hostname=mywinwks1&type=client...
   .../thor10-linux?hostname=mylinuxsrv1&type=server...

Use Case 3 - Use the URL in Scripts
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

By default, the generated download link is protected with a token that makes it impossible to download a package or generate a license without knowing that token. This token is specific to every ASGARD instance.  

You can use that URL in Bash or PowerShell scripts to automate scans on systems without an installed ASGARD agent. 

.. code:: powershell 

   $Type = "server"
   $Download_Url = "https://asgard2.nextron:8443/api/v0/downloads/thor/thor10-win?hostname=$($Hostname)&type=$($Type)&iocs=%5B%22default%22%5D&misps=%5B%222%22%5D&token=fQku7OKvDal2SMub4pv2QJOCCDL9P7dh5h"


Licensing
---------

ASGARD requires an Issuer-License in order to scan systems. The Issuer-License contains the number of asset-, server- and workstation systems that can be scanned with ASGARD Management Center. 

ASGARD will automatically issue a valid single-license for a particular system during its initial THOR scan. 

The screenshot below shows the licensing section of an ASGARD.

.. figure:: ../images/asgard-licensing.png
   :target: ../_images/asgard-licensing.png
   :alt: image70

   ASGARD licensing

In addition, ASGARD can create single-licenses that can be used for agentless scanning. In this case the license is generated and downloaded through the Web frontend. 

.. figure:: ../images/generate-licenses.png
   :target: ../_images/generate-licenses.png
   :alt: image70

   Generate licenses

The following systems require a workstation license in order to be scanned: 

* Windows 7 / 8 / 10
* Mac OS

The following systems require a server license in order to be scanned:

* All Microsoft Windows server systems
* All Linux systems

Provide an THOR Incident Response License (optional)
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

In case you have an THOR Incident Response license and want to use it with ASGARD, just upload it through the web based UI. This will remove all endpoint count restrictions from ASGARD. You can scan as many endpoints as you like – regardless of the type (workstation / server). 

Updates
-------

ASGARD Updates
^^^^^^^^^^^^^^

ASGARD will search for ASGARD updates on a daily basis. Available updates will automatically be shown in the section "Updates". 

As soon as an ASGARD update is available, a button ``Install Update`` appears. Clicking this button will start the update process. The ASGARD service will be restarted and the user will be forced to re-login. 

.. figure:: ../images/updating-asgard.png
   :target: ../_images/updating-asgard.png
   :alt: image71

   Updating ASGARD

Updates of THOR and THOR Signatures
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

By default, ASGARD will search for signature updates and THOR updates on an hourly basis. These updates will be set to active automatically. Therefore, a triggered scan will always employ the current THOR version and current signature version. You may disable or modify the automatic THOR and Signature updates by deleting or modifying the entries in this section. In the screenshot below no automatically updates are configured.

.. figure:: ../images/automatic-scanner-and-signature-updates.png
   :target: ../_images/automatic-scanner-and-signature-updates.png
   :alt: image73

   Automatic Scanner and Signature Updates

It is possible to intentionally scan with an old scanner version by clicking on the pencil icon and selecting the respective version from the drop-down menu. 

Please be aware, that this is a global setting and will affect all scans!


.. figure:: ../images/image73-1592213332299.png
   :target: ../_images/image73-1592213332299.png
   :alt: image73

   Selecting a Scanner version manually

Agent Updates
^^^^^^^^^^^^^

If an asset or an gent can be update, there will be a notice shown in the "Update > Agents" tab.

.. figure:: ../images/update-agent.png
   :target: ../_images/update-agent.png
   :alt: image73

   Update Agent


User Management
---------------

Access user management via ``Settings`` > ``Users``. This section allows administrators to add or edit user accounts.

.. figure:: ../images/add-user-account.png
   :target: ../_images/add-user-account.png
   :alt: Add User

   Add User Account

Editing a user account does not require a password although the fields are shown in the dialogue.

Access the user roles in ``Settings`` > ``Roles``. 

Roles
^^^^^

By default, ASGARD ships with the following pre-configured user roles. The pre-configured roles can be modified or deleted. The ASGARD role model is fully configurable.


.. figure:: ../images/user-roles-factory-default.png
   :target: ../_images/user-roles-factory-default.png
   :alt: ASGARD User Roles

   User Roles – Factory Defaults 

Note that all users except users with the right ``Readonly`` have the right to run scans on endpoints. 

The following section describes these predefined rights and restrictions that each role can have.

Rights
^^^^^^

.. list-table:: 
   :header-rows: 1

   * - Admin
   * - Unrestricted

.. list-table:: 
   :header-rows: 1

   * - ManageScanTemplates
   * - Allows scan templates management

.. list-table:: 
   :header-rows: 1

   * - ResponseControl
   * - Run playbooks, including playbooks for evidence collection, to kill processes or isolate an endpoint

.. list-table:: 
   :header-rows: 1

   * - RemoteConsole
   * - Connect to endsystems via remote console

.. list-table:: 
   :header-rows: 1

   * - RemoteConsoleProtocol
   * - Review the recordings of all remote console sessions

Restrictions 
^^^^^^^^^^^^

.. list-table:: 
   :header-rows: 1

   * - ForceStandardArgs
   * - Creat and start scans with predefined arguments or scan templates that are not restricted

.. list-table:: 
   :header-rows: 1

   * - NoInactiveAssets
   * - Cannot view inactive assets in asset management.

.. list-table:: 
   :header-rows: 1

   * - NoTaskStart
   * - Cannot start scans or task (playbooks)

.. list-table:: 
   :header-rows: 1

   * - Readonly
   * - Can't change anything, can't run scans or response tasks. Used to generate read-only API keys

LDAP Configuration
^^^^^^^^^^^^^^^^^^

In order to configure LDAP, navigate to ``Settings`` > ``LDAP``. In the left column you can test and configure the LDAP connection itself.
In the right column, the mapping of LDAP groups to ASGARD groups (and its associated permissions) is defined.

First check if your LDAP server is reachable by ASGARD by clicking "Test Connection".

.. figure:: ../images/ldap-server.png
   :target: ../_images/ldap-server.png
   :alt: Configure the LDAP Server

   Configure the LDAP Server

Then check the bind user you want to use for ASGARD. Read permissions on the bind user are sufficient.
To find out the distinguished name you can use an LDAP browser or query using the Powershell AD module command ``Get-ADUser <username>``.

.. figure:: ../images/ldap-bind.png
   :target: ../_images/ldap-bind.png
   :alt: Configure the LDAP Bind User

   Configure the LDAP Bind User

Next configure the LDAP filters used to identify the groups and users and their preferred attributes in your LDAP structure.
A default for LDAP and AD in a flat structure is given in the **"Use recommended filters"** drop-down menu, but you can
adapt it to your liking. The test button shows you if a login with that user would be successful and which groups ASGARD identified
and could be used for a mapping to ASGARD groups. 

.. figure:: ../images/ldap-filter.png
   :target: ../_images/ldap-filter.png
   :alt: Configure the LDAP User and Group Filters

   Configure the LDAP User and Group Filters

If you need to adapt the recommended configuration or want to customize it, we recommend an LDAP browser such as `ADExplorer <https://docs.microsoft.com/en-us/sysinternals/downloads/adexplorer>`_ from Sysinternals
to browse your LDAP structure. As an example you could use your organisation's e-mail address as a user login name if you change the "User Filter"
to ``(&(objectClass=user)(objectCategory=user)(userPrincipalName=%s))``

.. note::
   You need to save the configuration by clicking ``Update LDAP Config``.
   Using the test buttons only uses the data in the forms, but does not save it, so that you can use it for testing purposes anytime, without changing your working configuration.

After the LDAP configuration is set up, you need to provide role mapping from LDAP groups to ASGARD groups.
This is done in the right column by using the ``Add LDAP Role`` feature.

.. figure:: ../images/ldap-role.png
   :target: ../_images/ldap-role.png
   :alt: LDAP Group to ASGARD Role Mapping

   LDAP Group to ASGARD Role Mapping

.. note::
    All local users, except the built-in **admin** user, get disabled when LDAP is configured.

.. note::
    Enabling LDAP authentication disables personal API keys, password changes and 2FA for all user accounts except **admin**. (LDAP users cannot use said features).

Other Settings
--------------

Syslog Forwarding
^^^^^^^^^^^^^^^^^

Syslog forwarding can be configured in ``Settings`` > ``RSYSLOG``. To add a forwarding for local log source click ``Add Rsyslog Forwarding``. 

.. figure:: ../images/configure-syslog-forwarding.png
   :target: ../_images/configure-syslog-forwarding.png
   :alt: Syslog Forwarding

   Configure Syslog forwarding

The following log sources can be forwarded individually:

.. list-table:: Available Log Sources 
   :header-rows: 1

   * - Log
     - Description
   * - ASGARD Log
     - Everything related to the ASGARD service, processes, task and scan jobs
   * - ASGARD Audit Log
     - Detailed audit log of all user activity within the system
   * - Agent Log
     - All ASGARD agent activities
   * - THOR Log
     - THOR scan results (available if scan config has ``Syslog to ASGARD`` enabled) 

TLS Certificate Installation
^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Instead of using the pre-installed self-signed TLS Certificate, users can upload their own TLS Certificate for ASGARD. 

.. figure:: ../images/generate-csr.png
   :target: ../_images/generate-csr.png
   :alt: image80

   Generate a Certificate Signing Request (CSR)

.. figure:: ../images/upload-tls-certificate.png
   :target: ../_images/upload-tls-certificate.png
   :alt: image80

   Upload a TLS Certificate

In order to achieve the best possible compatibility with the most common browsers, we recommend using the system’s FQDN in both fields ``Common Name`` AND ``Hostnames``.

Please note that the generating a CSR on the command line is not supported.   

This CSR can be used to generate a TLS Certificate. Subsequently, this TLS Certificate can be uploaded in the ``Settings`` > ``TLS`` section.

Manage Services
^^^^^^^^^^^^^^^

The individual ASGARD services can be managed in ``Settings`` > ``Services``. The services can be stopped or restarted with the respective buttons in the ``Actions`` column. 

.. figure:: ../images/manage-services.png
   :target: ../_images/manage-services.png
   :alt: Configuration of Services

   Manage Services

NTP Configuration
^^^^^^^^^^^^^^^^^

The current NTP configuration can be found in the NTP sub-section. 

.. figure:: ../images/ntp-configuration.png
   :target: ../_images/ntp-configuration.png
   :alt: NTP Configuration

   NTP configuration

A Source Pool or Source Server can be removed by clicking the ``X`` button. To create a new Source Pool or Source Server, click ``Add NTP Source`` in the upper right corner. 

Settings for Bifrost
^^^^^^^^^^^^^^^^^^^^

Bifrost allows you to automatically upload suspicious files to your ASGARD during a THOR scan. If an Analysis Cockpit is connected, these files get automatically forwarded to the Analysis Cockpit in order to drop them into a connected Sandbox system. However, the collected files will stay on ASGARD for the amount of time specified in ``Retention time`` (0 days represent an indefinite amount of time). 

.. figure:: ../images/settings-for-bifrost.png
   :target: ../_images/settings-for-bifrost.png
   :alt: image83

   Settings for Bifrost

The collected files can be downloaded in the ``Evidence Collection`` section. All files are zip archived and password protected with the password ``infected``.

In order to automatically collect suspicious files, you have to create a scan with Bifrost enabled. Check the ``Send Suspicious Files to ASGARD`` option to send samples to the system set as ``bifrost2Server``. Use the placeholder 
``%asgard-host%`` to use the hostname of you ASGARD instance as the Bifrost server.

.. figure:: ../images/scan-option-for-bifrost.png
   :target: ../_images/scan-option-for-bifrost.png
   :alt: Bifrost Options

   Scan option for Bifrost 

This will collect all files with a score of 60 or higher and make them available for download in ASGARDs ``Collected Files`` section.

For Details on how to automatically forward to a sandbox system please refer to the Analysis Cockpit manual.

Link Analysis Cockpit
^^^^^^^^^^^^^^^^^^^^^

In order to connect to an Analysis Cockpit, enter the respective hostname of the Analysis Cockpit (use the same FQDN used during installation of the Analysis Cockpit) in the field ``Analysis Cockpit``, enter the one-time code, choose the type and click ``Connect``. 

.. figure:: ../images/linking-the-analysis-cockpit.png
   :target: ../_images/linking-the-analysis-cockpit.png
   :alt: image85

   Linking the Analysis Cockpit 

The Cockpit’s API key can be found on the right side of the Analysis Cockpit's ``Overview`` page.

.. figure:: ../images/image86-1592214154933.png
   :target: ../_images/image86-1592214154933.png
   :alt: image86

   Analysis Cockpit API Key

ASGARD must be able to connect to the Analysis Cockpit on port 443/TCP for a successful integration. Once connected, the Cockpit will show up in ASGARDs overview section along with the "last synced date" (lower left corner). 

Please wait up to five minutes for the status to change on ASGARD’s system status page. It will change from ``Not linked`` to ``Online``.

.. figure:: ../images/connectivity-status.png
   :target: ../_images/connectivity-status.png
   :alt: image87

   Cockpit connectivity status

Link MISP
^^^^^^^^^

In order to connect to a MISP navigate to the ``Settings > Link MISP`` tab.

Insert the MISP’s address along with the API Key and click ``Connect``.


.. figure:: ../images/linking-a-misp-to-asgard.png
   :target: ../_images/linking-a-misp-to-asgard.png
   :alt: image88

   Linking a MISP to ASGARD

The MISP connectivity status is shown in the ``Overview`` section. Please allow five minutes for the connection status to show green and MISP rules to show up in the ``IOC Management`` section.


.. figure:: ../images/connectivity-status.png
   :target: ../_images/connectivity-status.png
   :alt: image87

   MISP connectivity status

Change Proxy Settings
^^^^^^^^^^^^^^^^^^^^^

In this dialogue, you can add or modify ASGARDs proxy configuration. Please note, you need to restart the ASGARD service (Tab Services) afterwards. 


.. figure:: ../images/change-proxy-settings.png
   :target: ../_images/change-proxy-settings.png
   :alt: image89

   Change Proxy Settings

Link MASTER ASGARD
^^^^^^^^^^^^^^^^^^

In order to control your ASGARD with a MASTER ASGARD, you must generate a One-Time Code and use it in the "Add ASGARD" dialogue within the MASTER ASGARD frontend. 


.. figure:: ../images/link-master-asgard.png
   :target: ../_images/link-master-asgard.png
   :alt: image90

   Link MASTER ASGARD

Advanced
^^^^^^^^

The Advanced tab lets you specify additional global settings. The session timeout for web-based UI can be configured. Default is 24 hours. If ``Show Advanced Tasks`` is set, ASGARD will show system maintenance jobs (e.g. update ASGARD Agent on endpoints) within the response control section. 

Inactive assets can be hidden in the Asset Management Section by setting a suitable threshold for ``Hide inactive Assets``. 

Finally, the download for THOR packages can be protected with a token. If unprotected, anybody can request a THOR package with a valid license for a particular host just by sending a https request with the hostname included (for Details see :ref:`chapter 4.10 Generate Download Links <usage/administration:Generate Download Links>`). This may lead to unwanted exhaustion of the ASGARD license pool. 


.. figure:: ../images/advanced-settings.png
   :target: ../_images/advanced-settings.png
   :alt: image91

   Advanced Settings

User Settings
-------------

Changing your password
^^^^^^^^^^^^^^^^^^^^^^

To change your password, navigate to the ``User Settings`` section.

.. figure:: ../images/changing-your-password.png
   :target: ../_images/changing-your-password.png
   :alt: image92

   Changing your password

API Key
^^^^^^^

This section also allows you to set and modify an API key. 

Note that currently an API key always has the access rights of the user context in which it has been generated. If you want to create a restricted API key, add a new restricted user and generate an API key in the new user’s context.  

Uninstall ASGARD Agents 
-----------------------

The following listings contain commands to uninstall ASGARD Agents on endpoints. 

.. note::
   The commands contain names used by the default installer packages. In cases in which you've generated custom installer packages with a custom service and binary name, adjust the commands accordingly. 

Uninstall ASGARD Agents on Windows
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. code:: batch

   sc stop asgard2-agent
   sc delete asgard2-agent
   sc stop asgard2-agent_sc
   sc delete asgard2-agent_sc
   del /F /Q C:\Windows\System32\asgard2-agent

.. note::
   Line 3 and 4 are only necessary if the new service controller (on ASGARD 2.11+) has been installed. 

Uninstall ASGARD Agents on Linux
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

RPMs via ``yum``

.. code:: bash 

   yum remove 'asgard2-agent*'

DPKGs via ``apt-get`` 

.. code:: bash 

   apt-get remove 'asgard2-agent*'

Manual uninstall

.. code:: bash

   /usr/sbin/asgard2-agent-amd64 stop
   /usr/sbin/asgard2-agent-amd64 uninstall
   rm -rf /usr/sbin/asgard2-agent-amd64
   rm -rf /var/tmp/nextron/asgard2-agent
   rm -rf /var/lib/nextron/asgard2-agent

Uninstall ASGARD Agents on macOS
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. code:: bash 

   sudo /var/lib/asgard2-agent/asgard2-agent --uninstall
   sudo rm -rf /var/lib/asgard2-agent/asgard2-agent
