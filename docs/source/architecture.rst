The overall architecture of PEER is 

.. _Top level:

Top Level
=========

At the highest level, PEER is comprised of two components (Administration and Participant Portal), and multiple service utilities that it acquires via APIs from Private Access.  At the present time, PEER functions from four databases:

 * peer_surveys - contains all administrative functions for surveys
 * peer_surveys_published - contains all PST Surveys information
 * dbPPMS - contains everything else for PEER and Private Access
 * dbPPMS_D - contains 

As part of the migration to Open Source initiative, the databases that are exclusively used by PEER will be physically separated from the data tables that are used by the Private Access service functions.

Administration Component
========================

The administration component is used by authorized persons to create and manage participant portals, as well as view and scrub data that is acquired through the operation of such portals.  The 



Portal administration
---------------------

Portal settings themes etc...

Account administration



Users and permissions
---------------------

Access to the administrative services is provisioned in four tiers:

  * PEER Super Admin (has access to all of the administrative functions for any portal in the PEER system)
  * Portal Super Admin (all rights for their portal(s) only)
  * Staff members (delegated by the Portal Administrator or PEER Administrator with a subset of rights for any portals that the Administrator has the authority to delegate()
  * Recommended researchers (delegated by the Portal Administrator or PEER Administrato
 
by the PEER Administrator, who can either assign portal administration rights to an existing account or approve requests to set up a new account.  Each such account is assigned one or more Portal Administrators who received from individuals requesting to be a Portal Administrator.  to be  credentials assigned 




Participant portal
==================





.. attention: Remove PA Administrative access as a superior level to the PEER Administrator

Calls the Edit Guide API for the selected guide
