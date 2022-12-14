.. title:: Self-Service IaaS with Nutanix & ServiceNow

.. toctree::
   :maxdepth: 2
   :caption: Self-Service IaaS
   :name: _iaas
   :hidden:

   policies/policies
   snowcalm/snowcalm
   alerts/alerts
   webhook/webhook

.. toctree::
   :maxdepth: 2
   :caption: Staging Notes
   :name: _staging
   :hidden:

   staging/staging

###########################################
Self-Service IaaS with Nutanix & ServiceNow
###########################################

This bootcamp is designed to showcase Nutanix as an ideal platform for self-service Infrastructure As A Service (IaaS), leveraging the platform benefits, including its integration with the IT service management platform, ServiceNow (SNOW).

In this scenario, you are working with a party supply manufacturing and distribution business that has been heavily impacted by the COVID-19 pandemic. They are looking to pivot away from brick and mortar, and ramp up their e-commerce offerings. These include entertainment and gaming services to spice up Zoom parties, and planning services to re-invigorate safe, and socially distant in-person gatherings. They are hiring new app developers, and signing contracts with consultants daily. Unfortunately, the existing IT team has been unable to keep up with the demands of the growing team.

In addition to quickly providing more capacity for workloads, they must also provide self-service access for developers to maintain peak efficiency and bring new offerings to market quickly. As their business now solely depends on the uptime of their infrastructure, backup and disaster recovery are critical design components - as is ensuring workloads remain secure regardless of where they run in the environment. Finally, automating infrastructure remediation for user issues will help the IT scale operations to support their new influx of internal customers.

Environment Staging
===================

To let you experience the most fun and interesting parts of the lab, as well as accommodate several simultaneous users, multiple components have already been staged for you. *Let's explore!*

   The larger and more diverse an organization’s IT environment is, the more they need robust IT process to mitigate and manage risk. The need to follow agreed-to and approved processes is paramount when organizations are running critical business applications on which things like corporate profit, human lives, or financial markets may depend.

   IT organizations want to find a simple way for all end users to get IT services via a single portal, and they want everything automated! With many platforms in the mix, ServiceNow is where they all come together so IT organizations can have one set of processes for all platforms.

   To simplify and accelerate developing new apps and integrations for the platform, ServiceNow `provides free developer instances of its entire platform <https://developer.servicenow.com/>`_ to users. These instances are great for labs and demos too!

   In addition to your Nutanix cluster, each group of users will share a pre-staged ServiceNow Developer Instance. We'll cover more about the environment throughout the labs.

   .. figure:: images/3.png


What's New (Last updated 2-23-22)
=================================

Labs are for the following software versions:

- AOS - 5.20.3 LTS
- PC  - pc.2021.9.0.4

Agenda
======

- Introductions
- Lab Setup

Introductions
=============

- Name
- Familiarity with Nutanix

Initial Setup
=============

- Take note of the passwords provided.
- Log into your virtual desktops (connection info below).

Environment Details
===================

Nutanix Bootcamps run within the Nutanix Hosted POC environment. Your cluster includes all necessary images, networks, and VMs required to complete the exercises.

Credentials
-----------

.. note::

  The *<CLUSTER-PASSWORD>* is unique to each cluster. The leader of the Bootcamp provides this.

.. list-table::
   :widths: 25 35 40
   :header-rows: 1

   * - Credential
     - Username
     - Password
   * - Prism Element
     - admin
     - *<CLUSTER-PASSWORD>*
   * - Prism Central
     - admin
     - *<CLUSTER-PASSWORD>*
   * - Controller VM
     - nutanix
     - *<CLUSTER-PASSWORD>*
   * - Prism Central VM
     - nutanix
     - *<CLUSTER-PASSWORD>*

Each cluster has a dedicated domain controller VM (*AutoAD*), responsible for providing AD services for the *NTNXLAB.LOCAL* domain. The domain is populated with the following Users and Groups:

.. list-table::
   :widths: 25 35 40
   :header-rows: 1
   
   * - Group
     - Username(s)
     - Password
   * - Administrators
     - Administrator
     - nutanix/4u
   * - SSP Admins
     - adminuser01-adminuser25
     - nutanix/4u
   * - SSP Developers
     - devuser01-devuser25
     - nutanix/4u
   * - SSP Consumers
     - consumer01-consumer25
     - nutanix/4u
   * - SSP Operators
     - operator01-operator25
     - nutanix/4u
   * - SSP Custom
     - custom01-custom25
     - nutanix/4u
   * - Bootcamp Users
     - user01-user25
     - nutanix/4u

Access Instructions
-------------------

Attendees can access the Nutanix Hosted POC environment in several different ways:

Lab Access User Credentials
^^^^^^^^^^^^^^^^^^^^^^^^^^^

PHX Based Clusters:
**Username:** PHX-POCxxx-User01 (up to PHX-POCxxx-User20), **Password:** *<PROVIDED-BY-INSTRUCTOR>*

RTP Based Clusters:
**Username:** RTP-POCxxx-User01 (up to RTP-POCxxx-User20), **Password:** *<PROVIDED-BY-INSTRUCTOR>*

Frame VDI
^^^^^^^^^

Login to: HTTPS://frame.nutanix.com/x/labs

**Nutanix Employees** - Use your **NUTANIXDC** credentials
**Non-Employees** - Use **Lab Access User** credentials

Parallels VDI
^^^^^^^^^^^^^

PHX Based Clusters Login to: HTTPS://xld-uswest1.nutanix.com

RTP Based Clusters Login to: HTTPS://xld-useast1.nutanix.com

**Nutanix Employees** - Use your **NUTANIXDC** credentials
**Non-Employees** - Use **Lab Access User** credentials

Employee Pulse Secure VPN
^^^^^^^^^^^^^^^^^^^^^^^^^

Download the client:

PHX Based Clusters Login to: HTTPS://xld-uswest1.nutanix.com

RTP Based Clusters Login to: HTTPS://xld-useast1.nutanix.com

**Nutanix Employees** - Use your **NUTANIXDC** credentials
**Non-Employees** - Use **Lab Access User** credentials

Install the client.

In Pulse Secure Client, **Add** a connection:

For PHX:

- **Type** - Policy Secure (UAC) or Connection Server
- **Name** - X-Labs - PHX
- **Server URL** - xlv-uswest1.nutanix.com

For RTP:

- **Type** - Policy Secure (UAC) or Connection Server
- **Name** - X-Labs - RTP
- **Server URL** - xlv-useast1.nutanix.com