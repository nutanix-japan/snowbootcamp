.. _snow_alerts:

######################################
ServiceNow Alerts and CMDB Integration
######################################

Since July 2019, ServiceNow has offered native support for discovering Nutanix infrastructure running AHV or ESXi as part of its Configuration Management Database (CMDB). This provides ServiceNow/Nutanix customers with complete visibility of Nutanix clusters, hosts, VMs, categories, CVMs, storage pools, and storage containers with ServiceNow - including the ability to:

   - Generate full cluster topologies
   - Report on storage utilization
   - Near real-time visibility with event-driven discovery

Nutanix X-Play support for sending alerts to ServiceNow complements the CMDB integration by binding the event created by the Nutanix alert with the Configuration Item (VM, host, etc.) auto-discovered by ServiceNow.

   .. figure:: images/0.png

Configuring a ServiceNow environment to begin discovering your Nutanix resources only takes a few minutes, `as covered by Paul Harb here <https://www.youtube.com/watch?v=G1EqR0Vt1wo>`_. However, it does depend on a number of pre-requisites covered in the section below.

In this exercise, you will implement a Playbook using Prism X-Play to filter and send alerts to ServiceNow. You will then trigger the alert and review the data inside of ServiceNow.

Your Environment
================

.. raw:: html

   <strong><font color="red">To save time, and to ensure a consistent configuration for all users within the shared environment, your ServiceNow Developer Instance has already been pre-staged with all components necessary to complete the following exercise, including:</font></strong><br><br>

Subscription Plugins
--------------------

Certain plugins are available as subscriptions separate from the core ServiceNow platform. The following plug-ins have been activated for the labs:

- `Discovery <https://docs.servicenow.com/bundle/rome-it-operations-management/page/product/discovery/reference/r-discovery.html>`_ - Finds applications and devices on the network and updates the Configuration Management Database (CMDB).

- `Event Management <https://docs.servicenow.com/bundle/rome-it-operations-management/page/product/event-management/concept/c_EM.html>`_ - Provides alert aggregation and root cause analysis (RCA) for discovered services, application services, and automated alert groups.

- `IntegrationHub <https://docs.servicenow.com/bundle/rome-servicenow-platform/page/administer/integrationhub/concept/integrationhub.html>`_ - IntegrationHub enables execution of third-party APIs as a part of a flow when a specific event occurs in ServiceNow. These integrations (referred to as spokes) are easy to configure and enable you to quickly add powerful actions without the need to write a script. For example, you can post a message and incident details in a Slack channel when a high priority incident is created.

Native Integrations
-------------------

ServiceNow introduced support for Nutanix infrastructure running AHV and VMware ESXi in July 2019 through their **Discovery and Service Mapping Patterns** and **CMDB CI Class Models** plugins available for free through the ServiceNow Store.

   .. figure:: images/2.png

MID Server
----------

Auto-Discovery of Nutanix resources leverages the same MID Server connection as the Calm Plugin to communicate to and from the ServiceNow platform.

   .. figure:: images/3.png

Creating An Alert Policy
========================

While you may typically want to send many, or all, Nutanix alerts from your clusters to ServiceNow, we will target an alert specific to your VMs due to working in a shared environment.

#. Within **Prism Central**, select :fa:`bars` **> Activity > Alerts > Alert Policies > User Defined**.

   .. figure:: images/4.png

#. Click **Create Alert Policy**.

#. Fill out the following fields:

   - **Entity Type** - VM
   - **Entity** - All VMs in a Category
   - **Category** - CalmApplication:User##-SnowReq1 (ex. CalmApplication:User01-SnowReq1)
   - **Metric** - Memory Usage

#. Under **Static Threshold**, select **Alert Critical if** and specify **>= 75%**.

#. Change **Trigger alert if conditions persist for** to **0 minutes** to minimize time spent waiting to generate alerts in the lab.

   .. figure:: images/5.png

   This will create a critical alert within Prism Central every time one of your VMs with the User:\ *##* category exceeds 75% memory utilization.

#. Ensure **Enable Policy** is selected, and then click **Save**.

Send Alerts to ServiceNow
=========================

Nutanix Playbooks, or X-Play, allow administrators to easily automate tasks within the datacenter - including issuing alerts or notifications, performing VM lifecycle operations, or executing scripts and APIs.

#. Within **Prism Central**, select :fa:`bars` **> Operations > Playbooks**.

#. Click **Create Playbook**.

   .. note::

      You may first need to click **Get Started** to clear the Playbooks welcome dialog.

#. Select **Alerts Matching Criteria** as the trigger.

   .. figure:: images/6.png

   This type of trigger provides more flexibility for selecting multiple alerts to send to ServiceNow, while only requiring the creation of a single Playbook.

#. Select **Specific Alert Policies**, and then enter your previously created **User:**\ *##* **- VM Memory Usage** alert policy.

   .. figure:: images/7.png

#. Click :fa:`plus` **Add Action**, and then select **Send Alert to ServiceNow**.

#. Enter the *ServiceNow Instance Name* (ex. dev12345), and *ServiceNow admin Credentials*.

   .. figure:: images/8.png

#. Click **Save & Close**.

#. Specify **User**\ *##*\ **Alerts** as the **Name**, and then toggle the *Playbook Status* to **Enabled**.

   .. figure:: images/9.png

#. Click **Save**.

   Now that you have your alert configured, it's time to sit back, relax, and wait for an alert to trigger.

   .. figure:: images/10.png

   Hmmm. Maybe we should apply some artificial load instead!

#. SSH into your *User##*\ -CentOS VM as **root** and run the following commands to begin consuming free memory:

   ::

      yum -y install stress
      stress --vm-bytes $(awk '/MemAvailable/{printf "%d\n", $2 * 0.9;}' < /proc/meminfo)k --vm-keep -m 1

   .. note::

      Wouldn't it be great if **uninstalling** stress were that easy?

#. You can easily monitor the ramp in memory utilization by selecting **View by > Performance**.

   .. figure:: images/11.png

   .. note::

      The alert could take as long as 15 minutes to trigger, good time to stretch and grab a drink!

#. Once the alert appears in Prism Central, cancel the stress command in your SSH session by pressing ``Ctrl+C``.

   .. figure:: images/12.png

#. Return to your ServiceNow instance, ensuring you are logged in as admin.

#. In the **Filter Navigator** field in the upper-left, search for **Dashboards**. Select **Self-Service > Dashboards**.

   .. figure:: images/14.png

#. Under the **All** tab, search for **Nutanix** and select the built-in Nutanix Dashboard.

   This provides as overview of all of the Nutanix objects discovered by ServiceNow through Prism Central API. You can access additional details about resources within the dashboard, such as **Hosts**, by clicking them.

#. Within the dashboard, select the **Nutanix VM Summary** chart to view all currently discovered Nutanix VMs.

#. Search for and select your *User##*\ -CentOS VM to view associated details from the **Configuration Management Database**.

   .. figure:: images/15.png

   .. note::

      If you do not see your VM, you may need to force a Discovery of the Nutanix data. The staged configuration syncs data only once daily.

      Search for **Discovery Schedules** in the **Filter Navigator** and select the **Nutanix** schedule.

      Click **Discover Now**.

      .. figure:: images/16.png

      Return to the Dashboard and search for your VM again.

#. In the **Filter Navigator** field in the upper-left, search for **All Alerts**.

   If there are multiple alerts, you can easily identify yours by clicking the **Filter** icon and looking for **Resource starts with** *User##* (ex. User01).

   .. figure:: images/13.png

#. Select your alert to view all of the information that was sent from Prism, tagged to your resource in the CMDB.

   .. figure:: images/17.png

   With the alert data inside of ServiceNow, IT has visibility into which dependent services could be impacted, historical related incidents, knowledgebase articles, and the ability to open an incident to track remediation activities.

Takeaways
=========

- ServiceNow offers integrated support for discovery of Nutanix infrastructure

- X-Play provides a built-in action for sending Nutanix alerts to ServiceNow

- Sending alert data to ServiceNow allows for tracking incident and remediation history as part of the CMDB
