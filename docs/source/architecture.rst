.. _Architecture :

Architecture
************

This section of the document focuses on the overall architecture of PEER, beginning from a macro level and extending to the depth of the methods, source files and database tables that are presently being used by the application.  

.. Note:: In the interest of time, the present documentation generally does not include function calls, nor list the inputs and outputs based on each method.  Future enhancement to the technical documentation should consider adding this inventory.  

This descriptions that follow generally focus on the PEER open source code, and include (for reference purposes) information respecting the resources that are provided through API calls to PA Connect and PrivacyLayer, both of which are independent services licensed by Genetic Alliance from Private Access for inclusion in Genetic Alliance's SaaS-based version of PEER.

.. _General orientation:

General orientation
===================

The overall architecture of the PEER application is portayed in the following illustrations:

.. _PEER Architecture:

PEER Architecture
-----------------

At the highest level, PEER is comprised of two components (represented by the purple boxes in the following visualization), one that comprises all of the application's administrative backend functions, and a second for consumer-facing participant portals.

.. image:: https://s3.amazonaws.com/peer-downloads/images/TechDocs/PEER+High-Level+Architecture.png
     :alt: High-Level PEER Architecture Illustration  

.. _PA Architecture:

Private Access Services
-----------------------

The foregoing components are complemented by services that PEER acquires via secure API from Private Access.  Generally speaking, these outsourced services consist of two components. 

The first - referred to as *PA Connect* - consists of identity, authentication, single sign-on rights, and role-based access controls  for all PEER users.  And the second - called *PrivacyLayer* - provides privacy directives that may be set by institutional data holders and/or individual participants respecting who can access their information and for what purposes, and a number of related functions.  

These two components, and their respective functions are represented in the following visualization: 

.. image:: https://s3.amazonaws.com/peer-downloads/images/TechDocs/Private+Access+High-Level+Architecture.png
     :alt: High-Level Private Access Services Illustration  


Primary databases
=================

At the present time, PEER functions from four primary databases:

 * **peer_surveys** - contains all administrative functions for surveys
 * **peer_surveys_published** - contains all PST Surveys information
 * **dbPPMS** - contains (for the production environment) the administrative components of PEER, as well as Private Access' Privacy Preferences Management System (or PPMS)
 * **dbPPMS_D** - contains (for the beta environment) the administrative components of PEER, as well as the PPMS

.. Attention:: As part of migrating the PEER source code to open source, the PEER and Private Access components will be divided into separate databases so that the data tables that are exclusively used by PEER will be physically separated from the data tables that are used by the Private Access service.  At the conclusion of this work, we anticipate that the dbPPMS and dbPPMS_D will be used exclusively by Private Access (and not included in the OSS), and a third database for the administrative components of PEER and the API calls to Private Access will be reflected in the top level architecture.



.. _PEER Admin:

PEER Admin component
====================

The administration component of PEER is used by properly authorized and authenticated users to create and manage participant portals, as well as (to the extent permitted by each participant's privacy preference setttings) to discover, view and export data that is acquired through the operation of such portals.  

As illustrated above, the administrative component of PEER is comprised of the five functional areas shown in green in the above illustration. These include:

  * Search data 
  * Export data 
  * Super admin functions
  * Account management
  * Portal management

These areas are in turn broken into a number of fourth tier functions (illustrated by the yellow colored boxes above), and in some cases even further articulated into fifth (teal colored boxes), sixth (grey shaded boxes) and more granular functions (not shown on illustration, but discussed in the written description below).  

In order to assist future developers wishing to extend and/or modify PEER's features, the documentation below defines the methods, source files and database tables that are employed in providing these functions. In addition, the documentation provided for those PEER components that directly handle account data, profile data or survey data responses includes a description of the functions called by the methods and the inputs and/or output these use or provide.


Administative home
------------------

Upon logging into PEER as an administrative user or researcher, the :ref:`Admin-related APIs` calls the following method, which in turn retrieves a list of the portals to which the administrator has access.  This populates the "My Portals" sub-menu with a list of these portals from which the user may select the portal of interest.

.. _Method AH-01:

AH-01: getAllPortals
^^^^^^^^^^^^^^^^^^^^

**Source files:**
  
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/controller/PortalsController.java

  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/models/TblUserAccount.java

  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/dao/TblUserAccountDao.java

  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/dao/impl/TblUserAccountDaoImpl.java
  
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/service/PortalService.java
  
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/service/impl/PortalServiceImpl.java
  
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/dao/impl/TblLandingPagesDaoImpl.java
  
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/models/TblPeerAccount.java
  
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/dao/impl/TblPeerAccountDaoImpl.java
  
**Database tables:**
  
  * dbPPMS_D.user_account
  * dbPPMS_D.tblLandingPages  
  * dbPPMS_D.tblPeerAccount

.. Attention:: Opening the administrative home page calls the getAllPortals method from the API /private-access-adminportal/services/portal/getallportals.  The same method call is used for Search Registry Data (see below). This should be disambiguated during code cleanup.

.. _Search data:

Search data
-----------

Any authorized PEER user (*i.e.*, any authenticated persons with the proper administrative and/or researcher privileges) is able to initiate search queries respecting PEER data.  Together with the individual who is the subject of the data (or alternatively who is the authorized agent on the subject's behalf) when searching for his or her own information, these users making inquiries of PEER data are collectively referred to as "data seekers".  

One of the ways in which PEER is unique is because it enforces a privacy policy that a data seeker is only able to attain search results for the data he or she has been pre-authorized or is expressly consented to receive.  PEER is programmed to enforce this participant-in-control policy, and in the master version of the PEER code, receives access mediation advice based on an automated service call made via API to the PrivacyLayer service (for more information, *see* :ref:`PrivacyLayer`).  

Search inquiries are presently initiated from the Search Data menu, which is located on the administrative dashboard, and is limited to a single PEER portal.

.. Note:: One of the roadmap items for PEER that it would be desireable to enable before opening the PEER code to the open source community is cross-portal search.  A search UI has been developed and all of the foundational elements are in place to enable this, but the budget for the implementation of this feature has not existed. 

For additional information regarding the methods and corresponding function calls that are used in searching data and related functions, see *Searching data in PEER*.  

Export data
-----------


Super-admin functions
---------------------


Account management
------------------


Portal management
-----------------



.. _PA connect:

PA Connect service
==================

The identity and authorization component of PEER is administered through an API call to Private Access' oAuth 2.0-based PA Connect service.  

As in the illustrated above :ref:`PA Architecture` diagram, this service component of Private Access is comprised of two functional areas:

  * Profile
  * Account services

These areas are in turn broken into a number of fourth tier functions illustrated in yellow in the above :ref:`PA Architecture`, and further articulated into the following fifth (teal colored boxes) functions.  In order to assist future developers wishing to extend and/or modify PEER's features, the documentation below defines the methods, source files and database tables that are employed in providing these functions. 

Organization
------------

.. _Profile info:

Profile
-------

Profile information
^^^^^^^^^^^^^^^^^^^

Notifications
^^^^^^^^^^^^^

Privacy settings
^^^^^^^^^^^^^^^^

Proxy authorizations
^^^^^^^^^^^^^^^^^^^^


User accounts
-------------

Account services
----------------

Account details
^^^^^^^^^^^^^^^

Additional settings
^^^^^^^^^^^^^^^^^^^

Audit log
^^^^^^^^^

Challenge questions
^^^^^^^^^^^^^^^^^^^

Update password
^^^^^^^^^^^^^^^

.. _PrivacyLayer :

PrivacyLayer service
*********************

The consumer-centric access controls component of PEER are based on an API call seeking privacy directives from Private Access' PrivacyLayer (R) service.  

As illustrated above, this service component of Private Access is also comprised of two functional areas:

  * PrivacyLayer services
  * Privacy directives

PrivacyLayer service
--------------------

PD filter services
------------------

Filter contact service
^^^^^^^^^^^^^^^^^^^^^^

Filter discover service
^^^^^^^^^^^^^^^^^^^^^^^

Filter export service
^^^^^^^^^^^^^^^^^^^^^


Privacy directives
------------------

Organization
------------

Researcher
----------

Subject
-------

Data element
------------

This component was implemented as part of the data segmentation project undertaken to demonstrate the use of PrivacyLayer in conjunction with whole genome sequence data.

Privacy directive settings
--------------------------

Privacy directives (PDs) are...

Find/analyze
^^^^^^^^^^^^

View
^^^^

Export/link
^^^^^^^^^^^

Contact 
^^^^^^^

