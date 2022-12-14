.. _snow_calm:

#######################
SNOW Developer Instance
#######################

Before you can run this bootcamp, you will need to complete the staging of the ServiceNow (SNOW) environment. These steps are in addition to selecting the ServiceNow Runbook option in RX. This cluster must be staged with the *ServiceNow Bootcamp* runbook.

If you already have a Developer Instance that was staged for a previous Bootcamp, you can skip to `Launching the SNOW Bootcamp Blueprint`_.

You can deploy and prep your ServiceNow Developer Instance before you have access to your HPOC reservation. You ServiceNow Developer Instance will remain active as long as there is some activity (e.g. logging into the instance) within the last 10 days.

You can use the same ServiceNow Developer Instance to support multiple bootcamps, as long as the instance remains active, and you do not release/wipe your Developer Instance after your bootcamp.

A single Developer Instance can only support a single Prism Central deployment. If you are using multiple clusters they must all be joined to the same Prism Central, otherwise you'll need to use multiple SNOW Developer Instances.

.. _deploy_snow:

Deploying Your SNOW Developer Instance
======================================

#. Go to the `ServiceNow Developer Portal <https://developer.servicenow.com/>`_ and click **Sign up and Start Building** to create an account if you do not already have one.

   .. figure:: images/1.png

#. Once you have signed in, click **Request Instance**.

   .. figure:: images/2.png

#. Select the **Rome** release, and then click **Request**.

   After a few moments you'll get a popup with your *Instance URL* and *Password*. Note down both of these values!

   .. figure:: images/3.png

   .. note::

      It's best to do this several days before your bootcamp as there may not be immediate availability for the *Rome* instance type. As a result, you will have to join a wait list to get access.

      We are currently working on a version of the staging automation that supports *Quebec*, but need to wait until a compatible version of the Nutanix Calm plug-in is released.

#. Click **Return to the Developer Site**.

   Before you can run the *SNOW-Bootcamp-Rome* Blueprint to stage your environment, you must first activate three plug-ins on your Developer Instance. While the staging automation installs multiple plug-ins, these plug-ins can only be activated through the Developer portal.

#. Select the drop-down menu in the upper right-hand corner of the screen, and then then click **Activate Plugin**.

   .. figure:: images/4.png

#. Search for *Discovery*, and then select **Activate > Activate Plugin**.

   .. figure:: images/5.png

   This plug-in will take 45-60 minutes to install. You cannot proceed with other activations while this is in progress.

   .. note::

      If you receive an email indicating that activation of the Discovery plug-in failed, you can safely ignore this.

      *Why does it happen?*

      Why does anything happen? I'm a lab guide, not a doctor.

#. Once *Discovery* finishes activating, repeat this process to activate the *Event Management* plug-in.

   This plug-in will take ~30 minutes to install.

#. Once *Event Management* finishes activating, repeat this process to activate the *ServiceNow IntegrationHub Installer* plug-in.

   This plug-in will take ~5 minutes install. Once you have started the installation, you can proceed to the next section to run the *SNOW-Bootcamp-Rome* blueprint.

   If your HPOC environment is not yet available, ensure you are logging into your Developer instance once every couple of days, so that you do not lose your instance due to inactivity.

Launching the SNOW Bootcamp Blueprint
=====================================

#. Sign into the `ServiceNow Developer Portal <https://developer.servicenow.com/>`_, and confirm your *Instance Status* is *Online* as shown below.

   .. figure:: images/6.png

   Your instance can go to sleep due to periods of inactivity, and it is important to wake the instance before starting staging. If the status is *Waking Instance*, wait a few minutes, and then click **Refresh Status**.

#. Log into **Prism Central**, and then select :fa:`bars` **> Services > Calm**.

#. Select **Blueprints** from the left-hand menu.

#. Check the **SNOW-Bootcamp-Rome** blueprint, and then click **Actions > Launch**.

   .. figure:: images/7.png

#. Review the notes provided for each runtime variable, and then fill out the following fields:

   - **Name of the Application** - SNOW MID Server
   - **Prism Central Admin Password** - *Your Prism Central Admin Password*
   - **ServiceNow Admin Password** (Reminder: This is the admin password from step 3 of :ref:`deploy_snow`) - *Your ServiceNow Admin Password*
   - **ServiceNow Developer Instance URL** - *Your ServiceNow Developer Instance URL*
   - **New SNOW Instance?** -

      - Select **TRUE** if you *have not* used this same ServiceNow Developer instance to stage a ServiceNow Bootcamp previously
      - Select **FALSE** if you *have* used this same ServiceNow Developer instance to stage a ServiceNow Bootcamp previously

   .. note::

      At the end of the Blueprint, it will set your **ServiceNow Admin Password** to the same value as your **Prism Central Admin Password** if they are different. This is done to simplify the lab.

   .. note::

      Selecting **TRUE** will install all of the plug-ins and configuration parameters that can be re-used across multiple stagings, before configuring your environment for your HPOC.
      
      Selecting **FALSE** will simply clear out your previous HPOC cluster CMDB/Calm data, and re-configure for your current environment.

#. Click **Deploy** to launch the Blueprint.

   If this is a **New SNOW Instance = TRUE** staging, it will take ~2 hours to complete. **New SNOW Instance = FALSE** stagings take ~25 minutes to clean up, and re-configure the environment.

   If the Blueprint fails to deploy successfully, we suggest you confirm:
   
      - You are using the correct passwords.
      - The SNOW instance type is *Rome*.

   .. note::

      The ServiceNow staging uses Selenium to automate the web UI of ServiceNow, and can be broken by changes to UI elements that are out of our control, and may require a slight tweak to the Blueprint to succeed.

   Report issues to #technology-bootcamps on Slack. Include cluster details and the specific error output from the *Audit* tab of your Calm application, as shown below.

   .. figure:: images/8.png

#. Once the application reaches **RUNNING** status, your environment is ready for users to complete labs! *Have fun!*