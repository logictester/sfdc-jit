.. SafeNet Trusted Access documentation master file, created by
   sphinx-quickstart on Wed Mar 24 16:12:19 2021.
   You can adapt this file completely to your liking, but it should at least
   contain the root `toctree` directive.

======================================================================
Salesforce Just in Time (JIT) Provisioning with SafeNet Trusted Access
======================================================================

.. toctree::
   :maxdepth: 3
   :hidden:

   index


Overview
========

This guide documents the procedure to enable Just in Time (JIT) Provisioning of users from SafeNet Trusted Access to Salesforce. This procedure allows automatic user account creation in Salesforce after successful, SAML based, authentication using SafeNet Trusted Access

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




Step 1: Configure STA Auth Node for RDGW
========================================
