.. _Architecture :

Architecture
************

This section describes the overall architecture of PEER, beginning from a macro level and providing links to other documentation sections that identify the method calls, source files and database tables that are presently being used by the application. A more detailed discussion - including an articulation of the function calls, and listing the inputs and/or outputs based on each method - is provided for those functions that directly handle account data, profile data and/or actual survey response data. 

These descriptions generally focus on the PEER open source code and services that are provided through API calls to PA Connect and PrivacyLayer, both of which are independent services licensed by Genetic Alliance from Private Access for inclusion in Genetic Alliance's SaaS-based version of PEER.

.. _General orientation:

General orientation
===================

The overall architecture of the PEER application is portayed in the following two visualizations:

.. _PEER Architecture:

.. Attention:: Missing "Create Portal" yellow box under "Portal Management"

PEER Architecture
-----------------

At the highest level, PEER is comprised of two major components (each represented by a purple box in the following diagram). The one on the left - referred to as *Admin* - incorporates all of the application's backend functions for administrative users.  And the one on the right - called *Portal* - summarizes the features of PEER's consumer-facing participant portals.

.. image:: https://s3.amazonaws.com/peer-downloads/images/TechDocs/PEER+High-Level+Architecture.png
     :alt: High-Level PEER Architecture Illustration  

.. _PA Architecture:

Private Access Services
-----------------------

The foregoing components are complemented by services that PEER acquires via secure API calls to Private Access.  These outsourced services consist of two components. 

The first - referred to as *PA Connect* - consists of identity, authentication, single sign-on, and role-based access controls for all PEER users.  And the second - called *PrivacyLayer* - provides privacy directives that may be set by institutional data holders and/or individual participants respecting who can access their information and for what purposes, and a number of related functions.  

These components, and their respective functions are represented in the following visualization: 

.. image:: https://s3.amazonaws.com/peer-downloads/images/TechDocs/Private+Access+High-Level+Architecture.png
     :alt: High-Level Private Access Services Illustration  


Primary databases
=================

At the present time, PEER functions from four primary databases:

 * **peer_surveys** - contains all administrative functions for surveys
 * **peer_surveys_published** - contains all PST Surveys information
 * **dbPPMS** - contains (for the production environment) the administrative components of PEER, as well as Private Access' Privacy Preferences Management System (or PPMS)
 * **dbPPMS_D** - contains (for the beta environment) the administrative components of PEER, as well as the PPMS

The documentation contains a section that focuses on the most important data tables used by PEER. 

.. Attention:: As part of migrating the PEER software to open source, the PEER and Private Access components will be divided into separate databases so that the data tables that are exclusively used by PEER will be physically separated from the data tables that are used by the Private Access service.  At the conclusion of this work, we anticipate that the dbPPMS and dbPPMS_D will be used exclusively by Private Access (and not included in the OSS code base), and a third database for the administrative components of PEER and the API calls to Private Access will be reflected in the top level database architecture.


.. _PEER Admin:

PEER Admin component
====================

The administration component of PEER is used by properly authorized and authenticated users to create and manage participant portals, as well as (to the extent permitted by each participant's privacy preference setttings) to discover, view and export data that is acquired through the operation of such portals.  

As illustrated above, the administrative component of PEER is comprised of the five functional areas shown in green in the above illustration. These include:

  * :ref:`Search data` 
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

This component allows authenticated users with special permissions to manage the PEER system not available to portal administrators.  This includes:

  * Add Organization
  * Update Organization Information
  * Edit application-wide content (coming soon)
  * Create PEER administrator accounts
  * Create Private Access administrator accounts

Add Organization
""""""""""""""""

.. _Method SD-XX:

**Method XX:**

  **getAllOrganizationName**

**Inputs**::	 

**Outputs**::

An example is provided below of the JSON data that is received from the foregoing method call::

{  
   "status":"success",
   "message":"success",
   "isSuccess":true,
   "count":XX,
   "data":[  
      {  
         "idorganization":1,
         "tblShaOrganizationType":null,
         "name":"ORGANIZATION_NAME",
         "nameS":null,
         "address1":null,
         "address2":null,
         "address3":null,
         "city":null,
         "state":null,
         "postalCode":null,
         "country":null,
         "phone1":null,
         "phone2":null,
         "fax1":null,
         "fax2":null,
         "email":null,
         "contactName":null,
         "dateCreated":null,
         "createdBy":null,
         "dateUpdated":null,
         "updatedBy":null,
         "urlOrganization":null,
         "urlResearchProtocol":null,
         "moreInfo":null,
         "poaContactName":null,
         "officeHrs":null,
         "logoImageName":null,
         "ordinal":null,
         "selfPDSearchPreference":null,
         "selfPDExportPreference":null,
         "selfPDContactPreference":null,
         "tblShaParentOrganizations":[  

         ],
         "tblShaOrganizationPreference":[  

         ],
         "tblShaOrganizationPrivacyDirective":[  

         ],
         "seekerGroup":null
      }
	  ...
   ]
}

Related Function Calls
^^^^^^^^^^^^^^^^^^^^^^

SD-XX:  ShaOrganizationService.getAllOrganizationName()
"""""""""""""""""""""""""""""""""""""""""""""""""""""""

This function retrieves all organizations in the system.  This list of organizations is used to populate the autocomplete list.

**Inputs**::
  
  * n/a

**Outputs**::

  * List<TblShaOrganization> organizationData

SD-XX-01:  TblShaOrganizationDao.getAllOrganizationName()
"""""""""""""""""""""""""""""""""""""""""""""""""""""""""

This function retrieves all organizations in the system.

**Inputs**::
  
  * n/a

**Outputs**::

  * List<TblShaOrganization> organizationData

SD-XX-02:  TblShaOrganization.getIdorganization()
"""""""""""""""""""""""""""""""""""""""""""""""""

Retrieve the internal ID of an organization.

**Inputs**::
  
  * n/a

**Outputs**::

  * Integer organizationId

SD-XX-03:  TblShaOrganization.setIdorganization()
"""""""""""""""""""""""""""""""""""""""""""""""""

Set the internal ID of an organization in the object model.

**Inputs**::
  
  * Integer organizationId

**Outputs**::

  * n/a

SD-XX-04:  AESCryptoManager.decrypt()
"""""""""""""""""""""""""""""""""""""""""""""""""

Decrypt an encrypted string.

**Inputs**::
  
  * String encryptedString

**Outputs**::

  * String decryptedString

SD-XX-05:  TblShaOrganization.setName()
"""""""""""""""""""""""""""""""""""""""

Set the name property of the TblShaOrganization object model.

**Inputs**::
  
  * String name

**Outputs**::

  * n/a  
  
**Source files:**
  
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/controller/ShaOrganizationController.java
  
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/service/ShaOrganizationService.java
  
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/service/impl/ShaOrganizationServiceImpl.java

  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/models/TblShaOrganization.java
  
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/dao/TblShaOrganizationDao.java
  
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/dao/impl/TblShaOrganizationDaoImpl.java
  
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/util/AESCryptoManager.java
  
**Database tables:**
  
  * dbPPMS_D.tblShaOrganization

.. _Method SD-XX:

**Method XX:**

  **getSeekerGroupNames**

**Inputs**::	 

**Outputs**::

An example is provided below of the JSON data that is received from the foregoing method call::

{  
   "status":"success",
   "message":"success",
   "isSuccess":true,
   "count":4,
   "data":[  
      {  
         "idseekerGroup":1,
         "seekerGroupName":"Advocacy & Support Groups",
         "dateCreated":"05/07/2013"
      },
      {  
         "idseekerGroup":2,
         "seekerGroupName":"Medical Researchers",
         "dateCreated":"05/07/2013"
      },
      {  
         "idseekerGroup":3,
         "seekerGroupName":"Data Analysis",
         "dateCreated":"05/07/2013"
      },
      {  
         "idseekerGroup":4,
         "seekerGroupName":"Specialized Data Sets",
         "dateCreated":"03/08/2013"
      }
   ]
}

Related Function Calls
^^^^^^^^^^^^^^^^^^^^^^

SD-XX:  SeekerGroupService.getSeekerGroup()
"""""""""""""""""""""""""""""""""""""""""""""""""""""""

This function retrieves all record seekers in the system.

**Inputs**::
  
  * n/a

**Outputs**::

  * List<TblSeekerGroup> seekerGroups

SD-XX-01:  SeekerGroupDao.getSeekerGroup()()
""""""""""""""""""""""""""""""""""""""""""""

This function retrieves all record seekers in the system.

**Inputs**::
  
  * n/a

**Outputs**::

  * List<TblSeekerGroup> seekerGroups
  
**Source files:**
  
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/controller/SeekerGroupController.java
  
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/service/SeekerGroupService.java
  
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/service/impl/SeekerGroupServiceImpl.java

  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/dao/TblSeekerGroupDao.java
  
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/dao/impl/TblSeekerGroupDaoImpl.java

**Database tables:**
  
  * dbPPMS_D.tblSeekerGroup
  
.. _Method SD-XX:

**Method XX:**

  **getAllSeekerTemplates**

**Inputs**::	 

**Outputs**::

An example is provided below of the JSON data that is received from the foregoing method call::

{  
   "status":"success",
   "message":"success",
   "isSuccess":true,
   "count":XX,
   "data":[  
      {  
         "idseekerTemplate":3,
         "seekerDisplayName":"All Researchers",
         "fkIdrecordSeekerType":-200,
         "fkIdseeker":0,
         "fkIdrecordHandlerType":-100,
         "ordinal":0,
         "isDefault":false,
         "dateCreated":"06/10/2014",
         "dateUpdated":"06/10/2014",
         "tblWidgetPrivacyDirectives":null
      },
      {  
         "idseekerTemplate":31,
         "seekerDisplayName":"United Mitochondrial Disease Foundation (UMDF)",
         "fkIdrecordSeekerType":-400,
         "fkIdrecordHandler":-200,
         "fkIdorganization":11,
         "fkIdseeker":1,
         "fkIdrecordHandlerType":-200,
         "ordinal":3,
         "moreInfo":"The United Mitochondrial Disease Foundation (UMDF) is a non-profit organization serving to promote research and education for the diagnosis, treatment, and cure of mitochondrial disorders and to provide support to affected individuals and families.",
         "contactEmail":"CONTACT_EMAIL",
         "parentDirective":"",
         "isDefault":false,
         "dateCreated":"07/15/2014",
         "dateUpdated":"10/12/2016",
         "tblWidgetPrivacyDirectives":null
      },
	  ...
   ]
}

Related Function Calls
^^^^^^^^^^^^^^^^^^^^^^

SD-XX:  TblPlseekerTemplateService.getAllSeekerTemplate()
"""""""""""""""""""""""""""""""""""""""""""""""""""""""""

This function retrieves all record seekers templates in the system.

**Inputs**::
  
  * n/a

**Outputs**::

  * List<TblPlseekerTemplate> seekerTemplates

SD-XX-01:  TblPlseekerTemplateDao.getAllSeekerTemplate()
""""""""""""""""""""""""""""""""""""""""""""""""""""""""

This function retrieves all record seekers templates from the database.

**Inputs**::
  
  * n/a

**Outputs**::

  * List<TblPlseekerTemplate> seekerTemplates
  
**Source files:**
  
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/controller/SeekerTemplateController.java
  
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/service/TblPlseekerTemplateService.java
  
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/service/impl/TblPlseekerTemplateServiceImpl.java

  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/dao/TblPlseekerTemplateDao.java
  
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/dao/impl/TblPlseekerTemplateDaoImpl.java

**Database tables:**
  
  * dbPPMS_D.tblPlseekerTemplate

.. _Method SD-XX:

**Method XX:**

  **getDefaultSeekerTemplatesWithOrdinal**

**Inputs**::	 

**Outputs**::

An example is provided below of the JSON data that is received from the foregoing method call::

{  
   "status":"success",
   "message":"success",
   "isSuccess":true,
   "count":X,
   "data":[  
      {  
         "ordinal":15,
         "seekerDisplayName":"Researchers addressing your condition",
         "idseekerTemplate":37,
         "moreInfo":"PEER (the registry platform that\u2019s used by this project, the Platform for Engaging Everyone Responsibly) has a system that matches participants with researchers studying their condition. The \u201CResearchers addressing your condition\u201D setting lets you decide whether or not to include your information in this system, and share data with researchers studying your condition. All researchers invited to join the platform come with research projects or protocols reviewed by an oversight committee called an institutional review board (IRB). The IRB oversight protects participants by making sure studies follow proper ethics guidelines."
      },
	  ...
   ]
}

Related Function Calls
^^^^^^^^^^^^^^^^^^^^^^

SD-XX:  TblPlseekerTemplateService.getDefaultSeekerTemplatesWithOrdinal()
"""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""

This function retrieves the default seeker template from the database.

**Inputs**::
  
  * n/a

**Outputs**::

  * List<Map<String, Object>> seekerTemplates

SD-XX-01:  TblPlseekerTemplateDao.getDefaultSeekerTemplates()
"""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""

This function retrieves the default privacy directives from the database.

**Inputs**::
  
  * n/a

**Outputs**::

  *  List<TblPlseekerTemplate> seekerTemplates

SD-XX-02:  TblPlseekerTemplate.getIdseekerTemplate()
""""""""""""""""""""""""""""""""""""""""""""""""""""

**Inputs**::
  
  * n/a

**Outputs**::

  *  Integer seekerTemplateId

SD-XX-03:  TblPlseekerTemplate.getSeekerDisplayName()
"""""""""""""""""""""""""""""""""""""""""""""""""""""

**Inputs**::
  
  * n/a

**Outputs**::

  *  String seekerTemplateName

SD-XX-04:  TblPlseekerTemplate.getOrdinal()
"""""""""""""""""""""""""""""""""""""""""""

**Inputs**::
  
  * n/a

**Outputs**::

  *  Integer ordinal

SD-XX-05:  TblPlseekerTemplate.getMoreInfo()
""""""""""""""""""""""""""""""""""""""""""""

**Inputs**::
  
  * n/a

**Outputs**::

  *  String moreInfo

  
**Source files:**
  
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/controller/SeekerTemplateController.java
  
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/service/TblPlseekerTemplateService.java
  
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/service/impl/TblPlseekerTemplateServiceImpl.java
  
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/models/TblPlseekerTemplate.java
  
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/dao/TblPlseekerTemplateDao.java
  
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/dao/impl/TblPlseekerTemplateDaoImpl.java

**Database tables:**
  
  * dbPPMS_D.tblPLSeekerTemplate
  
.. _Method SD-XX:

**Method XX:**

  **getNewlyFoundPDSeekerTemplate**

**Inputs**::	 

**Outputs**::

An example is provided below of the JSON data that is received from the foregoing method call::

{  
   "status":"success",
   "message":"success",
   "isSuccess":true,
   "count":X,
   "data":[  
      {  
         "ordinal":15,
         "seekerDisplayName":"Researchers addressing your condition",
         "idseekerTemplate":37,
         "moreInfo":"PEER (the registry platform that\u2019s used by this project, the Platform for Engaging Everyone Responsibly) has a system that matches participants with researchers studying their condition. The \u201CResearchers addressing your condition\u201D setting lets you decide whether or not to include your information in this system, and share data with researchers studying your condition. All researchers invited to join the platform come with research projects or protocols reviewed by an oversight committee called an institutional review board (IRB). The IRB oversight protects participants by making sure studies follow proper ethics guidelines."
      },
	  ...
   ]
}

Related Function Calls
^^^^^^^^^^^^^^^^^^^^^^

SD-XX:  TblPlseekerTemplateService.getNewlyFoundPDSeekerTemplate()
""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""

This function retrieves the default seeker template from the database.

**Inputs**::
  
  * n/a

**Outputs**::

  * TblPlseekerTemplate seekerTemplate

SD-XX-01:  TblPlseekerTemplateDao.findbyname()
""""""""""""""""""""""""""""""""""""""""""""""

This function retrieves the default privacy directives from the database.

**Inputs**::
  
  * n/a

**Outputs**::

  * List<TblPlseekerTemplate> directives

  
**Source files:**
  
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/controller/SeekerTemplateController.java
  
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/service/TblPlseekerTemplateService.java
  
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/service/impl/TblPlseekerTemplateServiceImpl.java
  
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/models/TblPlseekerTemplate.java
  
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/dao/TblPlseekerTemplateDao.java
  
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/dao/impl/TblPlseekerTemplateDaoImpl.java

**Database tables:**
  
  * dbPPMS_D.tblPLSeekerTemplate
  
Update Organization Information
"""""""""""""""""""""""""""""""

Update existing organization information such as organization contact, privacy directives and default privcay settings.  Please refer to the section "Add Organization" for method and function calls as they are the same for updating an existing organization.

Create PEER administrator accounts
""""""""""""""""""""""""""""""""""

Invite a user to become a PEER administrator.

.. _Method SD-XX:

**Method XX:**

Retrieves the list of PEER administrator accounts from the database.

  **getUsers**

**Inputs**::	 

**Outputs**::

An example is provided below of the JSON data that is received from the foregoing method call::

{  
   "status":"success",
   "message":"success",
   "isSuccess":true,
   "count":11,
   "data":[  
      {  
         "firstName":"FIRSTNAME",
         "lastName":"LASTNAME",
         "email":"EMAIL",
         "userAccountId":XXX,
         "invitedUserId":null,
         "peerAccountId":YYY,
         "role":5,
         "roleName":"ROLE_PEER_ADMIN",
         "status":"Accepted",
         "freshAccount":false
      },
	  ...
   ]
}

Related Function Calls
^^^^^^^^^^^^^^^^^^^^^^

SD-XX:  UserAccountService.getUsers()
"""""""""""""""""""""""""""""""""""""

This function retrieves the list of PEER administration users from the database.

**Inputs**::
  
  * n/a

**Outputs**::

  * List<PortalUsers> userAccounts

SD-XX-01:  InvitedUsersDao.getAdminList()
"""""""""""""""""""""""""""""""""""""""""

This function retrieves the list of PEER administration users from the database.

**Inputs**::
  
  * n/a

**Outputs**::

  * List<InvitedUsers> userAccounts

SD-XX-02:  PeerAccountDao.getAdminList()
""""""""""""""""""""""""""""""""""""""""

This function retrieves the list of PEER administration users from the database.

**Inputs**::
  
  * n/a

**Outputs**::

  * List<InvitedUsers> userAccounts 
  
**Source files:**
  
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/controller/PrivateAccessController.java
  
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/service/UserAccountService.java

  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/service/impl/UserAccountServiceImpl.java

  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/dao/InvitedUsersDao.java
  
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/dao/impl/InvitedUsersDaoImpl.java

  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/dao/TblPeerAccountDao.java
  
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/dao/impl/TblPeerAccountDaoImpl.java

**Database tables:**
  
  * dbPPMS_D.invited_users
  * dbPPMS_D.tblPeerAccount
  
.. _Method SD-XX:

**Method XX:**

Retrieves the list of PEER administrator accounts from the database.

  **inviteUser**

**Inputs**::	 

An example is provided below of the JSON data that is sent to the foregoing method call::

{  
   "firstName":"FIRSTNAME",
   "lastName":"LASTNAME",
   "email":"EMAIL",
   "userRoleId":X
}

**Outputs**::

An example is provided below of the JSON data that is received from the foregoing method call::

{  
   "status":"success",
   "message":"success",
   "isSuccess":true,
   "count":1,
   "data":null
}

Related Function Calls
^^^^^^^^^^^^^^^^^^^^^^

SD-XX:  UserAccountService.inviteUser()
"""""""""""""""""""""""""""""""""""""""

This function retrieves the list of PEER administration users from the database.

**Inputs**::
  
  * InviteNewPortalUserRequest inviteRequst
  * String base

**Outputs**::

  * n/a

SD-XX-01:  InviteNewPortalUserRequest.getUserRoleId()
"""""""""""""""""""""""""""""""""""""""""""""""""""""

This function retrieves the ID of the user role.

**Inputs**::
  
  * n/a

**Outputs**::

  * Integer userRoleId

SD-XX-03:  InviteNewPortalUserRequest.getFirstName()
"""""""""""""""""""""""""""""""""""""""""""""""""

This function retrieves the first name of the invited user.

**Inputs**::
  
  * n/a

**Outputs**::

  * String firstName

SD-XX-04:  InviteNewPortalUserRequest.getLastName()
"""""""""""""""""""""""""""""""""""""""""""""""""""

This function retrieves the last name of the invited user.

**Inputs**::
  
  * n/a

**Outputs**::

  * String lastName

SD-XX-05:  InviteNewPortalUserRequest.getEmail()
""""""""""""""""""""""""""""""""""""""""""""""""

This function retrieves the email of the invited user.

**Inputs**::
  
  * n/a

**Outputs**::

  * String email

SD-XX-06:  TblUserAccountDao.findByEmail()
""""""""""""""""""""""""""""""""""""""""""

This function retrieves the user account from the database using the email address.

**Inputs**::
  
  * String emailHash

**Outputs**::

  * TblUserAccount accountData

SD-XX-07:  TblUserAccountDao.findByEmailAndRole()
"""""""""""""""""""""""""""""""""""""""""""""""""

This function retrieves the user account from the database using the email address and role of the user.

**Inputs**::
  
  * String email
  * Integer role

**Outputs**::

  * InvitedUsers account

SD-XX-08:  sendInviteSuccessMail()
""""""""""""""""""""""""""""""""""

This function generates the invitation email.

**Inputs**::
  
  * String email
  * String language
  * String link
  * String role

**Outputs**::

  * n/a

SD-XX-09:  amazonSendMailService.sendMail()
"""""""""""""""""""""""""""""""""""""""""""

This function sends the invitation email to the recipient

**Inputs**::
  
  * PrivateAccessEmail emailMessage

**Outputs**::

  * n/a
 
**Source files:**
  
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/controller/PrivateAccessController.java
  
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/service/UserAccountService.java
  
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/service/impl/UserAccountServiceImpl.java
  
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/models/InviteNewPortalUserRequest.java
  
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/dao/TblUserAccountDao.java
  
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/dao/impl/TblUserAccountDaoImpl.java
  
  OpenID/trunk/private-access-server/private-access-mails/src/main/java/com/privateaccess/mail/PrivateAccessEmail.java

**Database tables:**
  
  * dbPPMS_D.user_account

Create Private Access administrator accounts
""""""""""""""""""""""""""""""""""""""""""""

Please refer to the section "Create PEER administrator accounts" above as the same methods and function calls are made with the role of Private Access Administrator.

Account management
------------------


.. _Method SD-XX:

**Method XX:**

Retrieves the account information from the database for the logged in user.

  **getAccountDetails**

**Inputs**::	 

**Outputs**::

An example is provided below of the JSON data that is received from the foregoing method call::

{  
   "status":"success",
   "message":"success",
   "isSuccess":true,
   "count":1,
   "data":{  
      "userAccountId":XX,
      "shaAccountId":0,
      "loginName":"LOGINNAME",
      "password":"PASSWORD",
      "loginNameS":"LOGINNAMEHASH",
      "dateCreated":"08/01/2014",
      "dateUpdated":"08/01/2014",
      "createdBy":null,
      "updatedBy":null,
      "siteKeyName":"a",
      "firstName":"FIRSTNAME",
      "lastName":"LASTNAME",
      "middleName":null,
      "suffix":null,
      "email":"EMAIL",
      "emailS":"EMAILHASH",
      "dob":null,
      "homePhone":"",
      "cellPhone":"CELLNUMBER",
      "address1":"ADDRESS1",
      "address2":"ADDRESS2",
      "address3":"ADDRESS3",
      "city":"CITY",
      "state":"STATE",
      "postalCode":"ZIPCODE",
      "country":"COUNTRY",
      "fax1":"FAX1",
      "fax2":"FAX2",
      "firstNameS":"FIRSTNAMEHASH",
      "lastNameS":"LASTNAMEHASH",
      "dOBS":"DATEOFBIRTHHASH",
      "stateS":"STATE",
      "postalCodeS":"ZIPCODEHASH",
      "showContent":null,
      "compareFeature":null,
      "latitude":null,
      "longitude":null,
      "isIndividualAccount":true,
      "isResearcherAccount":true,
      "isLegalRights":true,
      "affiliatedOrganization":"",
      "linkToBio":"LINKURL",
      "userOrganizationId":0,
      "isNotBelongToOrganization":true,
      "isBelongToNotListedOrganization":false,
      "userRoleId":XX,
      "isActive":true,
      "encrypted":true
   }
}

Related Function Calls
^^^^^^^^^^^^^^^^^^^^^^

SD-XX:  AdminAccountService.getuserAccountById()
""""""""""""""""""""""""""""""""""""""""""""""""

This function retrieves the account information for the logged in user based on the accountId

**Inputs**::
  
  * Integer accountId

**Outputs**::

  * UserAccount userAccount

**Source files:**

  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/controller/AdminAccountController.java
  
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/service/AdminAccountService.java

  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/service/impl/AdminAccountServiceImpl.java

  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/dao/AdminAccountDao.java

  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/dao/impl/AdminAccountDaoImpl.java

  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/util/AESCryptoManager.java
  
**Database tables:**
  
  * dbPPMS_D.user_account
  
.. _Method SD-XX:

**Method XX:**

Retrieves the list of countries from the database.

  **getCountries**

**Inputs**::	 

**Outputs**::

An example is provided below of the JSON data that is received from the foregoing method call::

{  
   "status":null,
   "message":null,
   "isSuccess":true,
   "count":236,
   "data":[  
      {  
         "idcountry":1,
         "text":"United States of America",
         "code":"USA",
         "isActive":true,
         "isDefault":false,
         "ordinal":null,
         "dateCreated":"05/15/2013",
         "createdBy":null,
         "dateUpdated":"05/15/2013",
         "updatedBy":null,
         "latitude":"38",
         "longitude":"-97",
         "tblCountryMls":null
      },
	  ...
   ]
}

Related Function Calls
^^^^^^^^^^^^^^^^^^^^^^

SD-XX:  AdminAccountService.getuserAccountById()
""""""""""""""""""""""""""""""""""""""""""""""""

**Inputs**::
  
  * Integer accountId

**Outputs**::

  * UserAccount userAccount

**Source files:**

  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/controller/AdminAccountController.java
  
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/service/TblShaCountryService.java

  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/service/impl/TblShaCountryServiceImpl.java

  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/models/TblShaCountry.java

  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/dao/TblShaCountryDao.java

**Database tables:**
  
  * dbPPMS_D.tblShaCountry

.. _Method SD-XX:

**Method XX:**

Update the account information.

  **updateAccountDetails**

**Inputs**::	 

An example is provided below of the JSON data that is sent to the foregoing method call::

{  
  "userAccountId":XX,
  "shaAccountId":0,
  "loginName":"LOGINNAME",
  "password":"PASSWORD",
  "loginNameS":"LOGINNAMEHASH",
  "dateCreated":"08/01/2014",
  "dateUpdated":"08/01/2014",
  "createdBy":null,
  "updatedBy":null,
  "siteKeyName":"a",
  "firstName":"FIRSTNAME",
  "lastName":"LASTNAME",
  "middleName":null,
  "suffix":null,
  "email":"EMAIL",
  "emailS":"EMAILHASH",
  "dob":null,
  "homePhone":"",
  "cellPhone":"CELLNUMBER",
  "address1":"ADDRESS1",
  "address2":"ADDRESS2",
  "address3":"ADDRESS3",
  "city":"CITY",
  "state":"STATE",
  "postalCode":"ZIPCODE",
  "country":"COUNTRY",
  "fax1":"FAX1",
  "fax2":"FAX2",
  "firstNameS":"FIRSTNAMEHASH",
  "lastNameS":"LASTNAMEHASH",
  "dOBS":"DATEOFBIRTHHASH",
  "stateS":"STATE",
  "postalCodeS":"ZIPCODEHASH",
  "showContent":null,
  "compareFeature":null,
  "latitude":null,
  "longitude":null,
  "isIndividualAccount":true,
  "isResearcherAccount":true,
  "isLegalRights":true,
  "affiliatedOrganization":"",
  "linkToBio":"LINKURL",
  "userOrganizationId":0,
  "isNotBelongToOrganization":true,
  "isBelongToNotListedOrganization":false,
  "userRoleId":XX,
  "isActive":true,
  "encrypted":true
}

**Outputs**::

An example is provided below of the JSON data that is received from the foregoing method call::

{  
   "status":"success",
   "message":"success",
   "isSuccess":true,
   "count":1,
   "data":null
}

Related Function Calls
^^^^^^^^^^^^^^^^^^^^^^

SD-XX:  AdminAccountService.save()
""""""""""""""""""""""""""""""""""

Save the updated account information to the database.

**Inputs**::
  
  * UserAccount accountData

**Outputs**::

  * n/a

**Source files:**

  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/controller/AdminAccountController.java

  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/service/TblShaCountryService.java

  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/service/impl/TblShaCountryServiceImpl.java

  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/dao/AdminAccountDao.java

  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/dao/impl/AdminAccountDaoImpl.java

  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/util/AESCryptoManager.java
  
**Database tables:**
  
  * dbPPMS_D.user_account


Portal management
-----------------

.. _Method SD-XX:

**Method XX:**

Retrieves all record seekers from the database.

  **getAllSeekerTemplates**

**Inputs**::	 

**Outputs**::

An example is provided below of the JSON data that is received from the foregoing method call::

{  
   "status":"success",
   "message":"success",
   "isSuccess":true,
   "count":XX,
   "data":[  
      {  
         "idseekerTemplate":XX,
         "seekerDisplayName":"DISPLAYNAME",
         "fkIdrecordSeekerType":-400,
         "fkIdrecordHandler":-200,
         "fkIdorganization":11,
         "fkIdseeker":1,
         "fkIdrecordHandlerType":-200,
         "ordinal":3,
         "moreInfo":"DESCRIPTION_TEXT",
         "contactEmail":"CONTACTEMAIL",
         "parentDirective":"",
         "isDefault":false,
         "dateCreated":"07/15/2014",
         "dateUpdated":"10/11/2016",
         "tblWidgetPrivacyDirectives":null
      },
	  ...
   ]
}

Related Function Calls
^^^^^^^^^^^^^^^^^^^^^^

SD-XX:  TblPlseekerTemplateService.getAllSeekerTemplate()
"""""""""""""""""""""""""""""""""""""""""""""""""""""""""

Retrieves all record seekers from the database.

**Inputs**::
  
  * n/a

**Outputs**::

  * List<TblPlseekerTemplate> seekerTemplates

**Source files:**

  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/controller/SeekerTemplateController.java
  
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/service/TblPlseekerTemplateService.java

  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/service/impl/TblPlseekerTemplateServiceImpl.java

  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/models/TblPlseekerTemplate.java
  
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/dao/TblPlseekerTemplateDao.java
  
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/dao/impl/TblPlseekerTemplateDaoImpl.java
  
**Database tables:**
  
  * dbPPMS_D.tblPLSeekerTemplate

.. _Method SD-XX:

**Method XX:**

Insert new portal data into the database.

  **savePortal**

**Inputs**::	 

{  
   "landingPageName":"PORTAL_NAME",
   "landingPageUrl":"LANDINGPAGEURL",
   "demoLandingPageUrl":"DEMOPAGEURL",
   "createdBy":"XX",
   "isActive":true,
   "tblWidgetPrivacyDirectives":[  
      {  
         "createdBy":"XX",
         "isActive":true,
         "isPrimary":false,
         "dateCreated":"",
         "privacyDirectiveOrder":1,
         "tblPlseekerTemplate":{  
            "idseekerTemplate":137,
            "seekerDisplayName":"PRIVACY_DIRECTIVE_NAME",
            "fkIdrecordSeekerType":-400,
            "fkIdrecordHandler":-200,
            "fkIdorganization":129,
            "fkIdseeker":1,
            "fkIdrecordHandlerType":-200,
            "ordinal":3,
            "moreInfo":"PRIVACY_DIRECTIVE_DESCRIPTION",
            "contactEmail":"EMAIL",
            "parentDirective":"",
            "isDefault":false,
            "dateCreated":"08/05/2015",
            "dateUpdated":"08/05/2015",
            "tblWidgetPrivacyDirectives":null
         },
         "tblWidgetPrivacyDirectiveType":{  
            "idTblWidgetPrivacyDirectiveType":-100
         }
      },
	  ...
   ],
   "allowInstitutionalPD":false,
   "nickName":"Joey Test 2",
   "tblLandingPagesDefaultPrivacyDirectives":[  
      {  
         "fkIDSeekerTemplate":37
      },
      {  
         "fkIDSeekerTemplate":38
      },
      {  
         "fkIDSeekerTemplate":44
      },
      {  
         "fkIDSeekerTemplate":78
      }
   ],
   "defaultProxyAuthorizationImageName":null
}

**Outputs**::

An example is provided below of the JSON data that is received from the foregoing method call::

{  
   "status":"success",
   "message":"success",
   "isSuccess":true,
   "count":1,
   "data":XXX
}

Related Function Calls
^^^^^^^^^^^^^^^^^^^^^^

SD-XX:  PortalService.getPortalDetails()
""""""""""""""""""""""""""""""""""""""""

Retrieve portal information from the database.

**Inputs**::
  
  * Integer portalId

**Outputs**::

  * TblLandingPages portalDetails

SD-XX:  TblLandingPages.getTblWidgetPrivacyDirectives()
"""""""""""""""""""""""""""""""""""""""""""""""""""""""

Retrieve portal privacy directives from the database for a specific portal.

**Inputs**::
  
  * n/a

**Outputs**::

  * Set<TblWidgetPrivacyDirective> privacyDirectives

SD-XX:  TblLandingPages.getTblLandingPagesDefaultPrivacyDirectives()
""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""

Retrieve the default portal privacy directives from the database for a specific portal.

**Inputs**::
  
  * n/a

**Outputs**::

  * Set<TblLandingPagesDefaultPrivacyDirectives> defaultDirectives

SD-XX:  PortalService.deletePortalDefaultPD()
"""""""""""""""""""""""""""""""""""""""""""""

Delete default portal privacy directives from the database.

**Inputs**::
  
  * Set<TblLandingPagesDefaultPrivacyDirectives> privacyDirectives

**Outputs**::

  * n/a

SD-XX:  PortalAdminMappingService.addAccountPortal()
""""""""""""""""""""""""""""""""""""""""""""""""""""

Add user account to this portal access.

**Inputs**::
  
  * Integer accountId
  * Integer landingPageId

**Outputs**::

  * n/a

SD-XX:  WidgetPrivacyDirectiveService.saveWidgetPrivacyDirectives()
"""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""

Add user account to this portal access.

**Inputs**::
  
  * Set<TblWidgetPrivacyDirective> privacyDirectives

**Outputs**::

  * n/a

**Source files:**

  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/controller/PortalsController.java
  
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/service/PortalService.java

  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/service/impl/PortalServiceImpl.java
  
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/models/TblLandingPages.java
  
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/dao/TblLandingPagesDao.java
  
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/dao/impl/TblLandingPagesDaoImpl.java
  
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/models/TblLandingPagesDefaultPrivacyDirectives.java
  
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/service/PortalAdminMappingService.java

  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/service/impl/PortalAdminMappingServiceImpl.java

  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/service/WidgetPrivacyDirectiveService.java

  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/service/impl/WidgetPrivacyDirectiveServiceImpl.java

  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/models/TblWidgetPrivacyDirective.java

  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/dao/TblWidgetPrivacyDirectiveDao.java

  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/dao/impl/TblWidgetPrivacyDirectiveDaoImpl.java

**Database tables:**
  
  * dbPPMS_D.tblLandingPages
  * dbPPMS_D.tblLandingPagesDefaultPrivacyDirectives
  * dbPPMS_D.tblWidgetPrivacyDirective
  
Dashboard Messages
^^^^^^^^^^^^^^^^^^

.. _Method SD-XX:

**Method XX:**

Retrieves the portal information from the database given the portalId.

  **getWidgetInfoByPortalId**

**Inputs**::	 

**Outputs**::

An example is provided below of the JSON data that is received from the foregoing method call::

{  
   "status":"success",
   "message":"success",
   "isSuccess":true,
   "count":1,
   "data":{  
      "portalIds":{  
         "portalDateCreated":"2016-10-20 15:14:34.0",
         "portalName":"PORTAL_NAME",
         "liveWidgetId":"LIVE_WIDGET_ID",
         "demoWidgetId":"DEMO_WIDGET_ID"
      }
   }
}

Related Function Calls
^^^^^^^^^^^^^^^^^^^^^^

SD-XX:  WidgetInfoService.getWidgetInfoDetails()
""""""""""""""""""""""""""""""""""""""""""""""""

**Inputs**::
  
  * Integer portalId

**Outputs**::

  * TblWidgetInfo portalDetails

SD-XX:  WidgetDemoService.getWidgetDemoIDByLiveWidgetId()
"""""""""""""""""""""""""""""""""""""""""""""""""""""""""

**Inputs**::
  
  * Integer portalId

**Outputs**::

  * Integer portalId

  
**Source files:**

  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/controller/WidgetInfoController.java
  
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/service/WidgetInfoService.java

  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/service/impl/WidgetInfoServiceImpl.java

  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/models/TblWidgetInfo.java
  
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/dao/TblWidgetInfoDao.java

  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/dao/impl/TblWidgetInfoDaoImpl.java

  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/service/WidgetDemoService.java

  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/service/impl/WidgetDemoServiceImpl.java
  
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/models/TblWidgetDemo.java

  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/dao/TblWidgetDemoDao.java

  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/dao/impl/TblWidgetDemoDaoImpl.java

**Database tables:**
  
  * dbPPMS_D.tblWidgetInfo
  * dbPPMS_D.tblWidgetDemo

.. _Method SD-XX:

**Method XX:**

Retrieves the portal information from the database.

  **getPortal**

**Inputs**::	 

**Outputs**::

An example is provided below of the JSON data that is received from the foregoing method call::


Related Function Calls
^^^^^^^^^^^^^^^^^^^^^^

SD-XX:  PortalService.getPortalDetails()
""""""""""""""""""""""""""""""""""""""""

**Inputs**::
  
  * Integer portalId

**Outputs**::

  * TblLandingPages portalInfo

**Source files:**

  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/controller/PortalsController.java
  
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/service/PortalService.java

  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/service/impl/PortalServiceImpl.java

  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/models/TblLandingPages.java
  
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/dao/TblLandingPagesDao.java
  
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/dao/impl/TblLandingPagesDaoImpl.java

  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/models/TblWidgetPrivacyDirective.java

  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/dao/TblWidgetPrivacyDirectiveDao.java

  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/dao/impl/TblWidgetPrivacyDirectiveDaoImpl.java

  
**Database tables:**
  
  * dbPPMS_D.tblLandingPages
  * dbPPMS_D.tblWidgetPrivacyDirective
  
.. _Method SD-XX:

**Method XX:**

Retrieves the list of messages for a given portal.

  **assignMessages.php**

**Inputs**::	 

  * String survey_messages
  * Integer demoPortalId
  * Integer livePortalId

**Outputs**::

An example is provided below of the JSON data that is received from the foregoing method call::

{  
   "message":"success",
   "isSuccess":true,
   "data":[  
      {  
         "event":"EVENT_TYPE",
         "message_name":"MESSAGE_NAME",
         "id":"XX",
         "message_id":"MESSAGE_ID",
         "trigger":"TRIGGER",
         "status":"STATUS",
         "is_deleted":"X",
         "is_default":"X",
         "trigger_code":"TRIGGER_CODE",
         "event_code":"EVENT_CODE",
         "survey_id":"X",
         "is_auto_dismissed":"X"
      },
	  ...
   ]
}

Related Function Calls
^^^^^^^^^^^^^^^^^^^^^^

SD-XX:  showMessages()
""""""""""""""""""""""

**Inputs**::
  
  * Object request

**Outputs**::

  * JSON messages

**Source files:**

  admin/assignMessages.php
  includes/functions.php
  
**Database tables:**
  
  * peer_surveys_published.assign_messages
  * peer_surveys_published.triggers

.. _Method SD-XX:

**Method XX:**

Retrieves the list of messages for a given portal.

  **assignMessages.php**

**Inputs**::	 

  * String survey_messages
  * String messageCmd [show_messages, add_messages, change_status, delete_message, get_message]
  * String message_name
  * String message
  * String message_he_living
  * String message_he_deceased
  * String message_she_living
  * String message_she_deceased
  * Integer suppressVariant
  * Integer suppressVariant_he_living
  * Integer suppressVariant_he_deceased
  * Integer suppressVariant_she_living
  * Integer suppressVariant_she_deceased
  * Integer is_default
  * Integer message_id
  * Integer messageAutoDismiss

**Outputs**::

An example is provided below of the JSON data that is received from the foregoing method call::

{  
   "message":"success",
   "isSuccess":true,
   "data":true
}

Related Function Calls
^^^^^^^^^^^^^^^^^^^^^^

SD-XX:  saveMessage()
"""""""""""""""""""""

**Inputs**::
  
  * Object request

**Outputs**::

  * JSON status
  
**Source files:**

  admin/assignMessages.php
  includes/functions.php
  
**Database tables:**
  
  * peer_surveys_published.assign_messages

Features
^^^^^^^^

.. _Method SD-XX:

**Method XX:**

Retrieves the feature information from the database.

  **getFeatureDetails**

**Inputs**::	 

**Outputs**::

An example is provided below of the JSON data that is received from the foregoing method call::

{  
   "status":"success",
   "message":"success",
   "isSuccess":true,
   "count":1,
   "data":{  
      "idFeature":1,
      "featureName":"FEATURE_NAME",
      "featureImageURL":"IMAGE_URL",
      "featureVideoURL":"VIDEO_URL",
      "dateCreated":"DATE_CREATED",
      "dateUpdated":"DATE_UPDATED"
   }
}

Related Function Calls
^^^^^^^^^^^^^^^^^^^^^^

SD-XX:  FeatureService.getFeatureDetails()
""""""""""""""""""""""""""""""""""""""""""

**Inputs**::
  
  * Integer featureId

**Outputs**::

  * TblFeature userAccount

**Source files:**

  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/controller/FeatureController.java
  
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/service/FeatureService.java

  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/service/impl/FeatureServiceImpl.java

  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/models/TblFeature.java
  
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/dao/TblFeatureDao.java
  
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/dao/impl/TblFeatureDaoImpl.java

**Database tables:**
  
  * dbPPMS_D.tblFeature

.. _Method SD-XX:

**Method XX:**

Update the feature infomation.

  **updateFeatureDetails**

**Inputs**::	 

{  
   "idFeature":XX,
   "featureName":"FEATURE_NAME",
   "featureImageURL":"IMAGE_URL",
   "featureVideoURL":"VIDEO_URL"
}

**Outputs**::

An example is provided below of the JSON data that is received from the foregoing method call::

{  
   "status":"success",
   "message":"success",
   "isSuccess":true,
   "count":1,
   "data":1
}

Related Function Calls
^^^^^^^^^^^^^^^^^^^^^^

SD-XX:  featureService.updateFeature()
""""""""""""""""""""""""""""""""""""""

**Inputs**::
  
  * TblFeature featureInfo

**Outputs**::

  * TblFeature featureInfo

**Source files:**

  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/controller/FeatureController.java
  
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/service/FeatureService.java

  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/service/impl/FeatureServiceImpl.java

  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/models/TblFeature.java
  
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/dao/TblFeatureDao.java
  
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/dao/impl/TblFeatureDaoImpl.java

**Database tables:**
  
  * dbPPMS_D.tblFeature

.. _Method SD-XX:

**Method XX:**

Get the features for this portal.

  **listFeatures**

**Inputs**::	 

**Outputs**::

An example is provided below of the JSON data that is received from the foregoing method call::

{  
   "status":"success",
   "message":"success",
   "isSuccess":true,
   "count":126,
   "data":[  
      {  
         "idFeature":XX,
         "featureName":"FEATURE_NAME",
         "featureImageURL":"IMAGE_URL",
         "featureVideoURL":"VIDEO_URL",
         "dateCreated":"DATE_CREATED",
         "dateUpdated":"DATE_UPDATED"
      },
	  ...
   ]
}


Related Function Calls
^^^^^^^^^^^^^^^^^^^^^^

SD-XX:  FeatureService.listFeatures()
"""""""""""""""""""""""""""""""""""""

**Inputs**::
  
  * n/a

**Outputs**::

  * List<TblFeature> features

**Source files:**

  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/controller/FeatureController.java
  
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/service/FeatureService.java

  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/service/impl/FeatureServiceImpl.java

  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/models/TblFeature.java
  
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/dao/TblFeatureDao.java
  
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/dao/impl/TblFeatureDaoImpl.java

**Database tables:**
  
  * dbPPMS_D.tblFeature

Guides
^^^^^^  

.. _Method SD-XX:

**Method XX:**

Retrieves the list of guildes from the database for this portal.

  **getPortalGuides**

**Inputs**::	 

**Outputs**::

An example is provided below of the JSON data that is received from the foregoing method call::

{  
   "status":"success",
   "message":"success",
   "isSuccess":true,
   "count":1,
   "data":{  
      "guide3":X,
      "guide2":X,
      "guide1":X
   }
}

Related Function Calls
^^^^^^^^^^^^^^^^^^^^^^

SD-XX:  WidgetInfoService.getWidgetInfoGuides()
"""""""""""""""""""""""""""""""""""""""""""""""

**Inputs**::
  
  * Integer portalId

**Outputs**::

  * Map<String, Integer> guideInfo

**Source files:**

  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/controller/PortalsController.java
  
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/service/WidgetInfoService.java

  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/service/impl/WidgetInfoServiceImpl.java

  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/models/TblWidgetInfo.java
  
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/dao/TblWidgetInfoDao.java

  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/dao/impl/TblWidgetInfoDaoImpl.java

**Database tables:**
  
  * dbPPMS_D.tblWidgetInfo

.. _Method SD-XX:

**Method XX:**

Retrieve guide information for a specific guide.

  **getGuide**

**Inputs**::	 

**Outputs**::

Related Function Calls
^^^^^^^^^^^^^^^^^^^^^^

SD-XX:  GuideService.getPeerGuideByID()
"""""""""""""""""""""""""""""""""""""""

**Inputs**::
  
  * Integer guideId

**Outputs**::

  * TblPlguide guideInfo

**Source files:**

  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/controller/PeerGuideController.java
  
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/service/PeerGuideService.java

  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/service/impl/PeerGuideServiceImpl.java

  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/models/TblPlguide.java
  
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/dao/TblPlguideDao.java
  
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/dao/impl/TblPlguideDaoImpl.java
  
**Database tables:**
  
  * dbPPMS_D.tblPLGuide

.. _Method SD-XX:

**Method XX:**

Save guide information for a specific guide.

  **saveGuide**

**Inputs**::	 

**Outputs**::

Related Function Calls
^^^^^^^^^^^^^^^^^^^^^^

SD-XX:  GuideService.save()
"""""""""""""""""""""""""""""""""""""""

**Inputs**::
  
  * TblPlguide guideInfo

**Outputs**::

  * n/a

**Source files:**

  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/controller/PeerGuideController.java
  
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/service/PeerGuideService.java

  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/service/impl/PeerGuideServiceImpl.java

  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/models/TblPlguide.java
  
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/dao/TblPlguideDao.java
  
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/dao/impl/TblPlguideDaoImpl.java
  
**Database tables:**
  
  * dbPPMS_D.tblPLGuide
  
Referral Codes
^^^^^^^^^^^^^^

.. _Method SD-XX:

**Method XX:**

Retrieves the portal information for a specific portal.

  **getWidgetInfoByPortalId**

**Inputs**::	 

**Outputs**::

An example is provided below of the JSON data that is received from the foregoing method call::

{  
   "status":"success",
   "message":"success",
   "isSuccess":true,
   "count":1,
   "data":{  
      "portalIds":{  
         "portalDateCreated":"TIMESTAMP",
         "portalName":"PORTAL_NAME",
         "liveWidgetId":"LIVE_PORTAL_ID",
         "demoWidgetId":"DEMO_PORTAL_ID"
      }
   }
}

Related Function Calls
^^^^^^^^^^^^^^^^^^^^^^

SD-XX:  WidgetInfoService.getWidgetInfoDetails()
""""""""""""""""""""""""""""""""""""""""""""""""

**Inputs**::
  
  * Integer accountId

**Outputs**::

  * UserAccount userAccount

SD-XX:  WidgetDemoService.getWidgetDemoIDByLiveWidgetId()
"""""""""""""""""""""""""""""""""""""""""""""""""""""""""

**Inputs**::
  
  * Integer portalId

**Outputs**::

  * Integer portalId

**Source files:**

  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/controller/WidgetInfoController.java

  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/service/WidgetInfoService.java

  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/service/impl/WidgetInfoServiceImpl.java

  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/models/TblWidgetInfo.java
  
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/dao/TblWidgetInfoDao.java

  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/dao/impl/TblWidgetInfoDaoImpl.java

  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/service/WidgetDemoService.java

  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/service/impl/WidgetDemoServiceImpl.java
  
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/models/TblWidgetDemo.java

  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/dao/TblWidgetDemoDao.java

  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/dao/impl/TblWidgetDemoDaoImpl.java
 
**Database tables:**
  
  * dbPPMS_D.tblWidgetInfo
  * dbPPMS_D.tblWidgetDemo
  
.. _Method SD-XX:

**Method XX:**

Retrieves the list of referral codes for a specific portal.

  **listRefralCode**

**Inputs**::	 

  * Boolean isUsed
  * Integer landingPageId
  
**Outputs**::

An example is provided below of the JSON data that is received from the foregoing method call::

{  
   "status":"success",
   "message":"success",
   "isSuccess":true,
   "count":X,
   "data":[  
      {  
         "idrefferalCode":XXX,
         "tblSiteName":null,
         "tblPeerAccount":null,
         "tblLandingPages":null,
         "refferalCode":"test",
         "refferalCodeDesc":"DESCRIPTION",
         "isUsed":false,
         "dateCreated":"DATE_CREATED",
         "dateUpdated":"DATE_UPDATED",
         "startRange":"XX",
         "endRange":"XX",
         "isRange":true,
         "isPreSignInCode":false,
         "preSignInCode":null,
         "isPreSignUpCode":false,
         "preSignUpCode":null,
         "tblRefferalCodePrivacyDirectives":null,
         "tblRefferalCodeSurvey":null,
         "firstUsedDate":null,
         "lastUsedDate":null,
         "totalNumberOfUses":0
      },
	  ...
   ]
}

Related Function Calls
^^^^^^^^^^^^^^^^^^^^^^

SD-XX:  RefferalCodesService.getRefferalCodeByStatus()
""""""""""""""""""""""""""""""""""""""""""""""""""""""

**Inputs**::
  
  * Boolean isUsed
  * Integer landingPageId

**Outputs**::

  * List<TblRefferalCode> referalCodes

**Source files:**

  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/controller/RefferalCodesController.java
  
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/service/RefferalCodesService.java

  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/service/impl/RefferalCodesServiceImpl.java

  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/models/TblRefferalCode.java
  
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/dao/TblRefferalCodeDao.java
  
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/dao/impl/TblRefferalCodeDaoImpl.java
  
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/models/TblRefferalCodePrivacyDirective.java
  
**Database tables:**
  
  * dbPPMS_D.tblRefferalCode
  * dbPPMS_D.tblRefferalCodePrivacyDirective

.. _Method SD-XX:

**Method XX:**

Save a referral code for a specific portal.

  **saveRefralCode**

**Inputs**::	 

An example is provided below of the JSON data that is sent to the foregoing method call::

{  
   "refferalCode":"CODE",
   "refferalCodeDesc":"DESCRIPTION",
   "isRange":[true,false],
   "startRange":"XX",
   "endRange":"XX",
   "tblLandingPages":{  
      "idlandingPage":"XX"
   },
   "isPreSignInCode":[true,false],
   "isPreSignUpCode":[true,false],
   "tblRefferalCodePrivacyDirectives":[  

   ],
   "tblRefferalCodeSurvey":[  

   ]
}

**Outputs**::

An example is provided below of the JSON data that is received from the foregoing method call::

{  
   "status":"success",
   "message":"success",
   "isSuccess":true,
   "count":1,
   "data":null
}

Related Function Calls
^^^^^^^^^^^^^^^^^^^^^^

SD-XX:  RefferalCodesService.saveOrUpdate()
"""""""""""""""""""""""""""""""""""""""""""

**Inputs**::
  
  * TblRefferalCode referralCodeInfo

**Outputs**::

  * n/a

**Source files:**

  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/controller/RefferalCodesController.java
  
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/service/RefferalCodesService.java

  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/service/impl/RefferalCodesServiceImpl.java

  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/models/TblRefferalCode.java
  
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/dao/TblRefferalCodeDao.java
  
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/dao/impl/TblRefferalCodeDaoImpl.java

  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/models/TblRefferalCodePrivacyDirective.java
  
**Database tables:**
  
  * dbPPMS_D.tblRefferalCode
  * dbPPMS_D.tblRefferalCodePrivacyDirective

Remove Portal
^^^^^^^^^^^^^
  
.. _Method SD-XX:

**Method XX:**

Remove a portal from the database.

  **removePortal**

**Inputs**::	 

**Outputs**::

An example is provided below of the JSON data that is received from the foregoing method call::

{  
   "status":"success",
   "message":"success",
   "isSuccess":true,
   "count":1,
   "data":null
}

Related Function Calls
^^^^^^^^^^^^^^^^^^^^^^

SD-XX:  PortalService.removePortal()
""""""""""""""""""""""""""""""""""""

**Inputs**::
  
  * Integer portalId

**Outputs**::

  * n/a

**Source files:**

  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/controller/PortalsController.java
  
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/service/PortalService.java

  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/service/impl/PortalServiceImpl.java

  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/dao/TblLandingPagesDao.java

  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/dao/impl/TblLandingPagesDaoImpl.java

  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/models/TblLandingPages.java
  
**Database tables:**
  
  * dbPPMS_D.tblLandingPages

General Settings
^^^^^^^^^^^^^^^^

.. _Method SD-XX:

**Method XX:**

Retrieves the list of privacy directives for a specific portal.

  **getAllSeekerTemplates**

**Inputs**::	 

**Outputs**::

An example is provided below of the JSON data that is received from the foregoing method call::

{  
   "status":"success",
   "message":"success",
   "isSuccess":true,
   "count":XX,
   "data":[  
      {  
         "idseekerTemplate":XX,
         "seekerDisplayName":"United Mitochondrial Disease Foundation (UMDF)",
         "fkIdrecordSeekerType":XX,
         "fkIdrecordHandler":XX,
         "fkIdorganization":XX,
         "fkIdseeker":XX,
         "fkIdrecordHandlerType":XX,
         "ordinal":XX,
         "moreInfo":"The United Mitochondrial Disease Foundation (UMDF) is a non-profit organization serving to promote research and education for the diagnosis, treatment, and cure of mitochondrial disorders and to provide support to affected individuals and families.",
         "contactEmail":"CONTACT_EMAIL",
         "parentDirective":"",
         "isDefault":false,
         "dateCreated":"DATE_CREATED",
         "dateUpdated":"DATE_UPDATED",
         "tblWidgetPrivacyDirectives":null
      },
	  ...
   ]
}

Related Function Calls
^^^^^^^^^^^^^^^^^^^^^^

SD-XX:  TblPlseekerTemplateService.getAllSeekerTemplate()
"""""""""""""""""""""""""""""""""""""""""""""""""""""""""

This function retrieves all record seekers templates in the system.

**Inputs**::
  
  * n/a

**Outputs**::

  * List<TblPlseekerTemplate> seekerTemplates

**Source files:**

  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/controller/SeekerTemplateController.java
  
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/service/TblPlseekerTemplateService.java
  
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/service/impl/TblPlseekerTemplateServiceImpl.java

  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/dao/TblPlseekerTemplateDao.java
  
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/dao/impl/TblPlseekerTemplateDaoImpl.java

  
**Database tables:**
  
  * dbPPMS_D.tblPlseekerTemplate

.. _Method SD-XX:

**Method XX:**

Retrieves the portal information for a specific portal.

  **getPortal**

**Inputs**::	 

**Outputs**::

An example is provided below of the JSON data that is received from the foregoing method call::

{  
   "status":"success",
   "message":"success",
   "isSuccess":true,
   "count":X,
   "data":{  
      "idlandingPage":XX,
      "landingPageName":"PORTAL_NAME",
      "landingPageUrl":"PORTAL_URL",
      "isActive":true,
      "dateCreated":"DATE_CREATED",
      "dateUpdated":"DATE_UPDATED",
      "createdBy":XX,
      "demoLandingPageUrl":"DEMO_URL",
      "nickName":"NICKNAME",
      "isProxyEnabled":true,
      "isFemaleProfileImage":false,
      "isMaleProfileImage":false,
      "customRequiredStatementDocumentName":null,
      "femaleProfileImageName":null,
      "maleProfileImageName":null,
      "canRequestHealthDoc":false,
      "allowInstitutionalPD":true,
      "canAccessPrivacyPreferences":false,
      "isPrivacySettingBannerSet":true,
      "isContactBannerSet":true,
      "lastDateUpdatedForPreferences":"PREFERENCE_DATE_UPDATED",
      "lastDateUpdatedAuthorizeAccount":"AUTH_DATE_UPDATED",
      "lastDateUpdatedHealthDocuments":"DOC_DATE_UPDATED",
      "isDefaultProxyAuthorizationImage":false,
      "defaultProxyAuthorizationImageName":null,
      "tblWidgetPrivacyDirectives":[  
         {  
            "idwidgetPrivacyDirective":XXX,
            "tblPlseekerTemplate":{  
               "idseekerTemplate":XXX,
               "seekerDisplayName":"SEEKER_NAME",
               "fkIdrecordSeekerType":XXX,
               "fkIdrecordHandler":-XXX,
               "fkIdorganization":XXX,
               "fkIdseeker":XXX,
               "fkIdrecordHandlerType":XXX,
               "ordinal":XXX,
               "moreInfo":"DESCRIPTION",
               "contactEmail":"EMAIL",
               "parentDirective":"",
               "isDefault":false,
               "dateCreated":"DATE_CREATED",
               "dateUpdated":"DATE_UPDATED",
               "tblWidgetPrivacyDirectives":null
            },
            "isActive":true,
            "dateCreated":"DATE_CREATED",
            "createdBy":XXX,
            "isPrimary":false,
            "privacyDirectiveOrder":XXX,
            "tblWidgetPrivacyDirectiveType":{  
               "idTblWidgetPrivacyDirectiveType":XXX,
               "text":"DESCRIPTION"
            }
         },
		 ...
      ],
      "tblPortalAdminMappings":null,
      "tblLandingPagesRecommendedOrganizations":null,
      "tblLandingPagesDefaultPrivacyDirectives":[  
         {  
            "idLandingPagesDefaultPrivacyDirectives":XXX,
            "fkIDSeekerTemplate":XXX
         },
		 ...
      ],
      "customRequiredStatementEnabled":false
   }
}

Related Function Calls
^^^^^^^^^^^^^^^^^^^^^^

SD-XX:  PortalService.getPortalDetails()
""""""""""""""""""""""""""""""""""""""""

**Inputs**::
  
  * Integer portalId

**Outputs**::

  * TblLandingPages portalInfo

**Source files:**

  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/controller/PortalsController.java
  
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/service/PortalService.java

  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/service/impl/PortalServiceImpl.java

  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/models/TblLandingPages.java
  
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/dao/TblLandingPagesDao.java
  
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/dao/impl/TblLandingPagesDaoImpl.java
  
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/models/TblLandingPagesDefaultPrivacyDirectives.java

  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/models/TblLandingPages.java
  
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/dao/TblLandingPagesDao.java
  
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/dao/impl/TblLandingPagesDaoImpl.java
  
  
**Database tables:**
  
  * dbPPMS_D.tblLandingPages
  * dbPPMS_D.tblLandingPagesDefaultPrivacyDirectives

.. _Method SD-XX:

**Method XX:**

Save portal settings for a specific portal.

  **savePortal**

**Inputs**::	 

An example is provided below of the JSON data that is sent to the foregoing method call::

{  
   "status":"success",
   "message":"success",
   "isSuccess":true,
   "count":X,
   "data":{  
      "idlandingPage":XX,
      "landingPageName":"PORTAL_NAME",
      "landingPageUrl":"PORTAL_URL",
      "isActive":true,
      "dateCreated":"DATE_CREATED",
      "dateUpdated":"DATE_UPDATED",
      "createdBy":XX,
      "demoLandingPageUrl":"DEMO_URL",
      "nickName":"NICKNAME",
      "isProxyEnabled":true,
      "isFemaleProfileImage":false,
      "isMaleProfileImage":false,
      "customRequiredStatementDocumentName":null,
      "femaleProfileImageName":null,
      "maleProfileImageName":null,
      "canRequestHealthDoc":false,
      "allowInstitutionalPD":true,
      "canAccessPrivacyPreferences":false,
      "isPrivacySettingBannerSet":true,
      "isContactBannerSet":true,
      "lastDateUpdatedForPreferences":"PREFERENCE_DATE_UPDATED",
      "lastDateUpdatedAuthorizeAccount":"AUTH_DATE_UPDATED",
      "lastDateUpdatedHealthDocuments":"DOC_DATE_UPDATED",
      "isDefaultProxyAuthorizationImage":false,
      "defaultProxyAuthorizationImageName":null,
      "tblWidgetPrivacyDirectives":[  
         {  
            "idwidgetPrivacyDirective":XXX,
            "tblPlseekerTemplate":{  
               "idseekerTemplate":XXX,
               "seekerDisplayName":"SEEKER_NAME",
               "fkIdrecordSeekerType":XXX,
               "fkIdrecordHandler":-XXX,
               "fkIdorganization":XXX,
               "fkIdseeker":XXX,
               "fkIdrecordHandlerType":XXX,
               "ordinal":XXX,
               "moreInfo":"DESCRIPTION",
               "contactEmail":"EMAIL",
               "parentDirective":"",
               "isDefault":false,
               "dateCreated":"DATE_CREATED",
               "dateUpdated":"DATE_UPDATED",
               "tblWidgetPrivacyDirectives":null
            },
            "isActive":true,
            "dateCreated":"DATE_CREATED",
            "createdBy":XXX,
            "isPrimary":false,
            "privacyDirectiveOrder":XXX,
            "tblWidgetPrivacyDirectiveType":{  
               "idTblWidgetPrivacyDirectiveType":XXX,
               "text":"DESCRIPTION"
            }
         },
		 ...
      ],
      "tblPortalAdminMappings":null,
      "tblLandingPagesRecommendedOrganizations":null,
      "tblLandingPagesDefaultPrivacyDirectives":[  
         {  
            "idLandingPagesDefaultPrivacyDirectives":XXX,
            "fkIDSeekerTemplate":XXX
         },
		 ...
      ],
      "customRequiredStatementEnabled":false
   }
}

**Outputs**::

An example is provided below of the JSON data that is received from the foregoing method call::

{  
   "status":"success",
   "message":"success",
   "isSuccess":true,
   "count":1,
   "data":XXX
}

Related Function Calls
^^^^^^^^^^^^^^^^^^^^^^

SD-XX:  PortalService.getPortalDetails()
""""""""""""""""""""""""""""""""""""""""

**Inputs**::
  
  * Integer portalId

**Outputs**::

  * TblLandingPages portalData

SD-XX:  PortalService.getTblWidgetPrivacyDirectives()
"""""""""""""""""""""""""""""""""""""""""""""""""""""

**Inputs**::
  
  * n/a

**Outputs**::

  * Set<TblWidgetPrivacyDirective> privacyDirectives

SD-XX:  WidgetPrivacyDirectiveService.deleteWidgetPrivacyDirectives()
"""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""

**Inputs**::
  
  * Set<TblWidgetPrivacyDirective> privacyDirectives

**Outputs**::

  * n/a

SD-XX:  PortalService.getTblLandingPagesDefaultPrivacyDirectives()
""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""

**Inputs**::
  
  * n/a

**Outputs**::

  * Set<TblLandingPagesDefaultPrivacyDirectives> privacyDirectives

SD-XX:  PortalService.deletePortalDefaultPD()
"""""""""""""""""""""""""""""""""""""""""""""

**Inputs**::
  
  * Set<TblLandingPagesDefaultPrivacyDirectives> privacyDirectives

**Outputs**::

  * n/a

SD-XX:  TblLandingPages.getTblWidgetPrivacyDirectives()
"""""""""""""""""""""""""""""""""""""""""""""""""""""""

**Inputs**::
  
  * n/a

**Outputs**::

  * Set<TblWidgetPrivacyDirective> privacyDirectives

SD-XX:  WidgetPrivacyDirectiveService.saveWidgetPrivacyDirectives()
"""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""

**Inputs**::
  
  * Set<TblWidgetPrivacyDirective> privacyDirectives

**Outputs**::

  * n/a

**Source files:**

  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/controller/PortalsController.java

  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/service/PortalService.java

  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/service/impl/PortalServiceImpl.java

  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/models/TblLandingPages.java
  
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/dao/TblLandingPagesDao.java
  
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/dao/impl/TblLandingPagesDaoImpl.java

  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/service/WidgetPrivacyDirectiveService.java

  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/service/impl/WidgetPrivacyDirectiveServiceImpl.java

  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/models/TblWidgetPrivacyDirective.java

  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/dao/TblWidgetPrivacyDirectiveDao.java

  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/dao/impl/TblWidgetPrivacyDirectiveDaoImpl.java

  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/models/TblLandingPagesDefaultPrivacyDirectives.java
  
**Database tables:**
  
  * dbPPMS_D.tblLandingPages
  * dbPPMS_D.tblLandingPagesDefaultPrivacyDirectives
  * dbPPMS_D.tblWidgetPrivacyDirective
  
Recommended Organizations
^^^^^^^^^^^^^^^^^^^^^^^^^


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

