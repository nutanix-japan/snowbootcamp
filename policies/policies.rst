.. _snow_preparingenv:

########################
Preparing Calm Blueprint
########################

Before we can use ServiceNow to provide self-service access to users, you must first define a Nutanix Calm project, and an application Blueprint.

Creating A Calm Project
=======================

Nutanix Calm allows you to build, provision, and manage your applications across both private (AHV, ESXi) and public cloud (AWS, Azure, GCP) infrastructure.

In order for non-infrastructure administrators to access Calm, allowing them to create or manage applications, users or groups must first be assigned to a *Project*, which acts as a logical container to define user roles, infrastructure resources, and resource quotas. Projects define a set of users with a common set of requirements or a common structure and function, such as (in our example) a team of developers collaborating on the Fiesta application.

#. Within **Prism Central**, select :fa:`bars` **> Services > Calm**.

#. Select **Projects** from the left-hand menu, and then click :fa:`plus` **Create Project**.

   .. figure:: images/12.png

#. Specify *User##*\ -Project (ex. User01-Project) as your *Project Name*, and then click **Create**.

#. Under *Users, Groups, & Roles*, click **Add Users**.

   .. figure:: images/24.png

#. Click :fa:`plus` **Add User** and fill out the following fields:

   - **Name** - operator## (ex. operator01)
   - **Role** - Operator

   .. figure:: images/25.png

#. Click **Save Users and Project**.

#. Under **Accounts**, click **Add Accounts**.

#. Click **Select Account > NTNX_LOCAL_AZ** to configure your Nutanix cluster.

#. Click **Add/Edit Clusters and Subnets**.

   .. figure:: images/26.png

#. Select your cluster from the dropdown menu and select your **Secondary** virtual network. Only selected subnets will appear available for assignment to Calm workloads.

   .. figure:: images/27.png

#. Click **Confirm**.

   .. note::

      If multiple networks are selected, you can select the **Default** network by clicking the :fa:`star`.

#. Leave **Quotas** blank and click **Save Accounts and Project**.

#. Click **Create Environment**, and fill out the following fields:

   - Enter *User##*\ -Environment in the *Name* field, and click **Next**.

   - Click on the **Select Account** drop-down, and choose **NTNX_LOCAL_AZ**.

   - Click anywhere in the *VM Configuration* box to expand it.

   - Click on the *Linux* link, and perform the following within the *Linux* section.

      - Within the *VM Configuration* section, specify the following:

         - **vCPUs** - 2
         - **Cores per vCPU** - 1
         - **Memory (GiB)** - 4

      - Scroll down to the *Disks* section, and choose **CentOS7.qcow2** from the *Image* drop-down.

      - Scroll down to the *Network Adapters (NICs)* section, and click the :fa:`plus-circle`.

         - Choose **Secondary** from the *NIC 1* drop-down.

#. Click **Next**.

#. Click :fa:`plus` **Add Credentials**, fill out the following fields, and click **Done**.

   - **Credential Name** ``ROOT``
   - **Username:** ``root``
   - **Password** ``nutanix/4u``

#. Click **Save Environment & Project**.

Uploading A Calm Blueprint
==========================

A Blueprint is the framework for every application that you model by using Nutanix Calm. Blueprints are templates that describe all the steps that are required to provision, configure, and execute tasks on the services and applications that are created. A Blueprint also defines the lifecycle of an application, and its underlying infrastructure, starting from the creation of the application to the actions that are carried out on a application (updating software, scaling out, etc.) until the termination of the application.

You can use Blueprints to model applications of various complexities, from simply provisioning a single virtual machine, to provisioning and managing a multi-node/multi-tier application.

For the purposes of this exercise, you will upload an existing Blueprint of a single VM application deployment. Within the customer environment, this Blueprint could represent a pre-configured environment for a developer.

#. `Download the Single VM CentOS Blueprint by right-clicking here and saving. <https://raw.githubusercontent.com/nutanixworkshops/snowbootcamp/master/plugins/CentOS%20VM.json>`_

#. Select **Blueprints** from the left-hand toolbar. 

   .. figure:: images/16.png

#. Click **Upload Blueprint**, and then select the **CentOS VM.json** file downloaded in Step 1.

#. Update the **Blueprint Name** to include your *User##*, and then select the *User##*\ -Project.

   .. figure:: images/17.png

#. Click **Upload**.

   Before the Blueprint can be used, the networks, disk images, and credentials must be configured for your environment. Additionally, you will incorporate the categories associated with your data protection and network isolation policies.

#. Within your **CentOS VM** Blueprint, click **VM Details >**.

#. Select the **Account** drop-down, and observe that in this environment, Nutanix AHV is the only option.

   While Calm provides the ability to define deployment requirements for multiple different cloud providers within a single Blueprint, one of the key advantages of Nutanix Clusters is being able to utilize a single configuration (Nutanix AHV) regardless of whether the app is being provisioned on-premises, or in your elastic, public cloud hosted cluster.

#. Click **VM Configuration >**.

   Here you'll see the specifications for the VM being provisioned. Observe that a Calm macro, or variable, is being used to customize the VM name by prepending the user's initials.

#. Click the **Runtime** icon for both vCPUs and memory, to allow for customization of these values at the time of launching the Blueprint.

   We will use this in a later exercise to allow a ServiceNow administrator to create multiple catalog offerings from the same Blueprint.

   .. figure:: images/18.png

#. Under **Disks > Disk (1) > Image** select **CentOS7.qcow2** to clone from the existing disk stored within the Prism Image Service.

   .. figure:: images/19.png

#. Under *NICs*, select **Secondary**.

   If you had multiple clusters available as part of your project, this selection would control the Cluster on which the Blueprint would be provisioned. Configuring it as a runtime variable would allow a ServiceNow administrator additional flexibility in defining the self-service offering to provision the Blueprint to multiple different Nutanix clusters.

#. Click **Advanced Options**.

#. Under *Credentials*, click **Add/Edit Credentials**. Specify `nutanix/4u` as the *Password*.

   This will be configurable for the user at runtime, but Calm requires a default value be provided before the Blueprint can be launched.

   .. figure:: images/22.png

#. Click **Done > Save**.

   .. note::

      You should no longer see any red error alerts for the Blueprint, but warning alerts related to missing variable values are expected and will not impact the Blueprint.

Takeaways
=========

- Calm Projects allow you to define pools of resources for specific users and groups.

- Calm Blueprints enable repeatable application deployments and lifecycle operations.