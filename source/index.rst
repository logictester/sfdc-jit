.. SafeNet Trusted Access documentation master file, created by
   sphinx-quickstart on Wed Mar 24 16:12:19 2021.
   You can adapt this file completely to your liking, but it should at least
   contain the root `toctree` directive.

===========================================================================
Salesforce Just-in-Time (JIT) User Provisioning with SafeNet Trusted Access
===========================================================================

.. toctree::
   :maxdepth: 3
   :hidden:

   index


Overview
========

This guide documents the procedure to enable Just-in-Time (JIT) Provisioning of users from SafeNet Trusted Access to Salesforce. This procedure allows automatic user account creation in Salesforce after successful, SAML based, authentication using SafeNet Trusted Access

.. note::

   This guide assumes Salesforce is federated to SafeNet Trusted Access using SAML, the integration can be found `here <https://resources.safenetid.com/help/Salesforce/Salesforce/Index.htm>`_


Prerequisites
=============

  - Salesforce is configured using My Domain configuration
  - Salesforce is federated to SafeNet Trusted Access using SAML (Single Sign-On)
  - A user with a SafeNet Trusted Access authenticator is enrolled
  - Users can authenticate using SafeNet Trusted Access


Solution Overview
=================

With Just-in-Time (JIT) provisioning, SafeNet Trusted Access passes user information to your Salesforce org in a SAML assertion to automatically create user accounts.
SafeNet Trusted Access sends user information to your org in an :guilabel:`Attributes` statement in the SAML assertion.
When a user logs in to an org with standard JIT provisioning enabled, Salesforce pulls user data from the identity provider and stores it in a new :guilabel:`User` object.


Configuration Steps
===================

The configuration requires the following steps:

  **In Salesforce**

  - Modify the configured **Single Sign-on** policy to enable **Provisioning**
  - Identify the **Profile** to be configured for provisioning

  **In SafeNet Trusted Access**

  - Modify **Salesforce** application in STA to add **SAML Return Attributes**


Salesforce Configuration
========================

Modify the configured **Single Sign-on** policy to enable **Provisioning**
**************************************************************************

In order to enable Just in Time (JIT) Provisioning in Salesforce, we have to modify the existing **Single Sign-on** policy

In Salesforce Console, modify the policy by following these steps:

1. Login to Salesforce as a **System Administrator**
2. Navigate to :guilabel:`Identity` and click on :guilabel:`Single Sign-On Settings`
3. Click :guilabel:`Edit` to edit your existing SAML Configuration

.. thumbnail:: _images/sso_settings.png

4. Change :guilabel:`SAML Identity Type` to :guilabel:`Assertion contains the Federation ID from the User object`

.. thumbnail:: _images/sso_id.png

5. Under **Just-in-time User Provisioning**, enable :guilabel:`User Provisioning Enabled`
6. Make sure **Standard** is selected

.. thumbnail:: _images/sso_jit.png

7. Click :guilabel:`Save` to save the configuration

.. note::

   For greater control over the provisioning process, Salesforce supports **Custom SAML JIT with Apex handler**, more information can be found `here <https://developer.salesforce.com/docs/atlas.en-us.apexref.meta/apexref/apex_interface_Auth_SamlJitHandler.htm>`_

.. warning::

   Once the federation **SAML Identity Type** is changed, users without **Federation ID** will fail to authenticate using SafeNet Trusted Access. To overcome this, open the user's account object under **Users** and set the **Federation ID** in **Single Sign On Information** to the user's email address

   .. thumbnail:: _images/fed_id.png


Salesforce is ready for Just-in-Time (JIT) User Provisioning.


Identify the **Profile** to be configured for provisioning
**********************************************************

To set the provisioning of users to the correct Salesforce Profile we need to identify the Profile ID in Salesforce. The easiest way to achieve this is by opening the desired profile and copying the Profile ID from the URL

In Salesforce Console, open and identify the Profile by following these steps:

1. Login to Salesforce as a **System Administrator**
2. Navigate to :guilabel:`Users` and click on :guilabel:`Profiles`
3. Find the Profile you would like to be used for Provisioning (for example: **Standard User**)

.. thumbnail:: _images/profile.png

4. Click on the Profile Name (for example: **Standard User**)
5. In the browser address bar, look at the end of the URL, the Profile ID is the value following **address=%2F**, starting with **00**

.. thumbnail:: _images/profile_id.png

6. Copy the value and save it for later use in SafeNet Trusted Access Configuration


SafeNet Trusted Access configuration
====================================

.. note:: Open SafeNet Trusted Access Console (you can use the following direct links based on your availability zone, opens in a new tab)

          |US Zone|

            .. |US Zone| raw:: html

              <a href="https://sta.us.safenetid.com" target="_blank">US Zone SafeNet Trusted Access Console</a>

          |EU Zone|

            .. |EU Zone| raw:: html

              <a href="https://sta.eu.safenetid.com" target="_blank">EU Zone SafeNet Trusted Access Console</a>

          |Classic Zone|

            .. |Classic Zone| raw:: html

              <a href="https://sta.safenetid.com" target="_blank">Classic Zone SafeNet Trusted Access Console</a>


Modify SafeNet Trusted Access - Salesforce application
******************************************************

In order to be able to provide the needed information for user creation in Salesforce, SafeNet Trusted Access - Salesforce application we've created to establish SAML based authentication, has to be modified by adding SAML Return Attribues.

In the STA Console, modify the Salesforce application by following these steps:

1. Go to the :guilabel:`Applications` tab
2. Click :guilabel:`Salesforce` Application
3. Under **Return Attribues**, click on :guilabel:`Add Attribue`
4. Add the following Attributes and Mappings:

.. note::

   Return Attribues are key sensative (use :guilabel:`copy` to copy the values)

   +---------------------------+--------------------------+-------------------------+
   | Return Attribute          | User Attribute (Mapping) | Custom Value            |
   +---------------------------+--------------------------+-------------------------+
   | .. code::                 |                          |                         |
   |                           |                          |                         |
   | User.Username             | Email address            |                         |
   +---------------------------+--------------------------+-------------------------+
   | .. code::                 |                          |                         |
   |                           |                          |                         |
   | User.LastName             | Last Name                |                         |
   +---------------------------+--------------------------+-------------------------+
   | .. code::                 |                          |                         |
   |                           |                          |                         |
   | User.Email                | Email address            |                         |
   +---------------------------+--------------------------+-------------------------+
   | .. code::                 |                          |                         |
   |                           |                          |                         |
   | User.FederationIdentifier | Email address            |                         |
   +---------------------------+--------------------------+-------------------------+
   | .. code::                 |                          |                         |
   |                           |                          |                         |
   | User.Alias                | SAS User ID              |                         |
   +---------------------------+--------------------------+-------------------------+
   | .. code::                 |                          |                         |
   |                           |                          | Salesforce Profile ID   |
   | User.ProfileId            | Single Custom Value...   |                         |
   |                           |                          | (saved in this section) |
   +---------------------------+--------------------------+-------------------------+
