This is a temporary file for sections that are being moved from one place to another...
#######################################################################################



 
Search Survey Data
~~~~~~~~~~~~~~~~~~

Checks what role the data seeker has...

.. _Method SD-XX:

**Method SD-XX:**

     **getAccountAuthorities**

**Inputs**::	 
	
**Outputs**::

An example is provided below of a data element that is returned from the foregoing method call::

  {  
   "status":"success",
   "message":"success",
   "isSuccess":true,
   "count":1,
   "data":{  
      "permissions":{  
         "EXPORT_SURVEY_RESPONSES":true,
         "PORTAL_CODE":true,
         "THEME_AND_LANDING_PAGE":true,
         "VIEW_CONTACT_INFORMATION":true,
         "PROXY_AGENT":true,
         "SURVEYS":true,
         "DOWNLOAD_CONTACT_INFORMATION":true,
         "NOTHING_ALLOWED":false,
         "EDIT_ORGANIZATION_INFORMATION":true,
         "VIEW_AGGREGATE_DATA":true,
         "GUIDES":true,
         "REFERRAL_CODES":true,
         "VIEW_INDIVIDUAL_DATA":true,
         "EDIT_INDIVIDUAL_DATA":true,
         "GENERAL_SETTINGS":true,
         "DELETE_PORTAL":true
      },
      "role":X,
      "portalRole":Y
   }
  }

Related Function Calls
^^^^^^^^^^^^^^^^^^^^^^

SD-XX:  AuthorityManager.getAccountPermissionMap()
""""""""""""""""""""""""""""""""""""""""""""""""""

**Inputs**

  * Integer myPermissions
  * Array<Authorities> authorities
  
**Outputs**

  * Map<Authorities, boolean> map
  
SD-XX:  AuthorityManager.getAllPermissions()
""""""""""""""""""""""""""""""""""""""""""""

**Inputs**

  * n/a
  
**Outputs**

  * Integer permission

SD-XX:  PeerAccount.getFkIDUserRole()
"""""""""""""""""""""""""""""""""""""

**Inputs**

  * n/a
  
**Outputs**

  * Integer role
  
SD-XX:  PortalService.getAllPortals()
"""""""""""""""""""""""""""""""""""""

**Inputs**

  * n/a

**Outputs**

  * TblLandingPages portal

SD-XX:  PortalService.listAccountPortals()
""""""""""""""""""""""""""""""""""""""""""

**Inputs**

  * Integer peerAccountId

**Outputs**

  * TblLandingPages portal

SD-XX:  PeerAccount.getIdpeerAccount()
""""""""""""""""""""""""""""""""""""""

**Inputs**

  * n/a

**Outputs**

  * Integer peerAccountId
  
**Source files:**
  
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/controller/PortalsController.java

  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/service/PortalService.java
  
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/service/impl/PortalServiceImpl.java

  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/permission/AuthorityManager.java
  
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/access/AuthorityManager.java
  
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/permission/AccountAuthorities.java
  
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/permission/Authorities.java
  
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/access/Authorities.java
  
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/models/TblPortalAdminMapping.java
  
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/dao/TblPortalAdminMappingDao.java

  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/dao/impl/TblPortalAdminMappingDaoImpl.java

  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/models/TblPeerAccount.java
  
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/dao/TblPeerAccountDao.java
  
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/dao/impl/TblPeerAccountDaoImpl.java

**Database tables:**
  
  * dbPPMS_D.tblPeerAccount  
  * dbPPMS_D.tblPortalAdminMapping

.. Attention:: Ascertain why this is only called when a user wishes to search survey data (but not when the user wishes to search participant data).  Verify whether this is correct, omitted in the prior function, or unnecessary in this one.

.. _Method SD-XX:

**Method SD-XX:**

     **dashBoardApi.php**

**Inputs**::	 

An example is provided below of the JSON data that is sent to the foregoing method call::

  {  
   "instrument_chart":true,
   "survey_id":[  
      44
   ],
   "from_date":"2015-12-10",
   "to_date":"2016-10-18",
   "portal_id":"692",
   "token":"feyJhbGciOiJSUzI1NiJ9.eyJhdWQiOlsiNGMxY2UwZGUtZWM2Ni00Nzg4LWFlZTQtYjhmYzQ0YTRmY2NlIl0sImlzcyI6Imh0dHBzOlwvXC9jb25uZWN0LnByaXZhdGVhY2Nlc3MuY29tXC8iLCJleHAiOjE0NzY4MzUzMTcsImlhdCI6MTQ3NjgwNjUxNywianRpIjoiYWNiMzEyOGEtZGY3MC00YmFiLWFlMTUtN2JkNzhjZTUwODdiIn0.omPLZTxs4r8TOPvpZVpRfjfxKDB8ONqISWl4AP2AwsV2OV57TMbYlcc89P2QdnfTLaG7FyymnCrUWiiBEXlc8LSBnIgfTerf6xkYp0nRiR40S6-2IHk9AJ70lQ2sEaCXUkoYF5pKlONNNg6oLUET8mfmR4rp0B_Rhj-4YbjTuR",
   "filter_fks":[  

   ]
  }

**Outputs**::

An example is provided below of the JSON data that is received from the foregoing method call::

	{  
	   "message":"success",
	   "isSuccess":true,
	   "data":{  
		  "pie":{  
			 "isData":false
		  },
		  "instrument_count":4,
		  "question_count":85,
		  "chart_height":260,
		  "users_instrument_info":[  
			 {  
				"all_users":0,
				"completed_users":0,
				"percent_users":0
			 },
			 {  
				"all_users":0,
				"completed_users":0,
				"percent_users":0
			 },
			 {  
				"all_users":0,
				"completed_users":0,
				"percent_users":0
			 },
			 {  
				"all_users":0,
				"completed_users":0,
				"percent_users":0
			 }
		  ],
		  "asked_qcounts":[  
			 0,
			 0,
			 0,
			 0
		  ],
		  "actual_qcounts":[  
			 55,
			 14,
			 5,
			 11
		  ],
		  "groups":[  
			 "\\\"Test\\\" Questions##2265####0",
			 "Instrument rich text 14 pt##1884####0",
			 "Job Status##670####0",
			 "Insurance##723####0"
		  ],
		  "groupId":[  
			 2265,
			 1884,
			 670,
			 723
		  ]
	   }
	}

Related Function Calls
^^^^^^^^^^^^^^^^^^^^^^

SD-XX:  parentDashboard()
"""""""""""""""""""""""""

**Inputs**

  * Array<Object> request
  * String mode
  * String oauthServerURL
  
**Outputs**

  * Object returnData

SD-XX-01:  getPortalSurveys()
"""""""""""""""""""""""""""""

**Inputs**

  * Integer portalId
  
**Outputs**

  * Object surveyData

SD-XX-02:  getFilterForeignKeys()
"""""""""""""""""""""""""""""""""

**Inputs**

  * Array foreignKeys
  * String token
  * String oauthServerUrl
  
**Outputs**

  * Array filteredForeignKeys

SD-XX-02-01:  PAPermissions.filterForeignKeys()
"""""""""""""""""""""""""""""""""""""""""""""""

**Inputs**

  * Array foreignKeys
  * String permission [discover, contact, export]
  
**Outputs**

  * Array filteredForeignKeys

**Source files:**

  cron/dashBoardAPI.php
  includes/functions.php
  admin/classes/PAPermissions.php
  
**Database tables:**

  * peer_surveys_published.users
  * peer_surveys_published.survey_sections
  * peer_surveys_published.segment
  * peer_surveys_published.group
  * peer_surveys_published.question_choices
  * peer_surveys_published.questions
  * peer_surveys_published.data
  * peer_surveys_published.completed_instruments
  * peer_surveys_published.assign_survey
  
**Method SD-XX:**

     **dashBoardApi.php**

**Inputs**::	 

**Outputs**::	 

Related Function Calls
^^^^^^^^^^^^^^^^^^^^^^

SD-XX:  getrsrequestdetail()
""""""""""""""""""""""""""""

**Inputs**

  * Integer accountId
  * Authentication auth
  
**Outputs**

  * APIResponse response

SD-XX-01:  ContactService.getStudyTrialList()
"""""""""""""""""""""""""""""""""""""""""""""

**Inputs**

  * Integer accountId
  * Authentication auth
  
**Outputs**

  * List<TblStudyTrial> trials

SD-XX-01-01:  RsProfileInfo.getStudyTrialList()
"""""""""""""""""""""""""""""""""""""""""""""

**Inputs**

  * Integer accountId
  * Authentication auth
  
**Outputs**

  * List<TblStudyTrial> trials

**Source files:**
  
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/controller/ProfileInfoController.java
  
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/service/ContactService.java
  
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/service/impl/ContactServiceImpl.java

  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/service/impl/RsProfileInfo.java
  
**Database tables:**

.. _Export data:

Export data
===========




.. _PA-PEER admin:

PA/PEER administration
======================

Add organization
----------------

PA admins
---------

PA content
----------

PEER admins
-----------

Update organization
-------------------




.. _Account management:

Account management
==================

.. _Portal administration:

Portal administration
=====================

As shown, the portal administration section of the PEER admin contains twelve components that collectively enable the creation, modification and management of the appearance, content, and functional behaviour of the portals under a PEER sponsor's control, as well as the rights of staff members and researchers to access the administrative features and data acquired through the portal (obviously subject to the individuals' privacy and data sharing directives).



.. _Dashboard Messages:

Dashboard Messages
------------------

.. _Method SD-XX:

**Method XX:**

  **getAccountAuthorities**

.. Note:: This method call is the same as the method call described in the previous section for Search Survey Data.  Please refer to that method call for further details.

**Method SD-XX:**

     **dashBoardApi.php**

**Inputs**::	 

An example is provided below of the JSON data that is sent to the foregoing method call::

{  
   "get_participant_messages":true,
   "portal_id":"692",
   "message_id":[  
      41,
      42,
      43,
      128,
      129,
      133,
      155,
      213
   ],
   "filter_fks":[  

   ]
}

**Outputs**::

An example is provided below of the JSON data that is received from the foregoing method call::


{  
   "message":"success",
   "isSuccess":true,
   "data":[  

   ]
}

Related Function Calls
^^^^^^^^^^^^^^^^^^^^^^

SD-XX:  apiPortalMessages()
"""""""""""""""""""""""""""

**Inputs**::
  
  * Object request

**Outputs**::

  * Array messages

**Source files:**
  
  cron/dashBoardAPI.php
  includes/functions.php
  
**Database tables:**
  
  * peer_surveys_published.participant_messages
  * peer_surveys_published.assign_messages
  * peer_surveys_published.admins
  
.. _Method SD-XX:

**Method XX:**

  **getProfileDetails**

.. Note:: This method call is the same as the method call described in the previous section for Search Registry Data in method SD-04.  Please refer to that method call for further details.


.. _Export Data:

Export Data
-----------





.. _Features :

Features
--------

The following methods (FE-01 and FE-02) are invoked when an authorized user selects the Features menu item on the administrative menu.

.. _Method FE-01:

**Method FE-01:**

     **getFeatureDetails**
	
**Source files:**
  
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/controller/FeatureController.java
  
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/service/FeatureService.java
  
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/service/impl/FeatureServiceImpl.java
  
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/models/TblFeaturedContentType.java
  
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/models/TblFeature.java
  
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/dao/TblFeatureDao.java
  
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/dao/impl/TblFeatureDaoImpl.java

**Database tables:**
  
  * dbPPMS_D.tblFeature
  * dbPPMS_D.tblFeaturedContentType
  
.. _Method FE-02:

**Method FE-02:**

     **updateFeatureDetails**
	
**Source files:**
  
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/controller/FeatureController.java

  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/service/FeatureService.java
  
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/service/impl/FeatureServiceImpl.java

  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/models/TblFeature.java
  
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/dao/TblFeatureDao.java
  
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/dao/impl/TblFeatureDaoImpl.java

**Database tables:**
  
  * dbPPMS_D.tblFeature



.. _Guides :

Guides
------


Calls the Edit Guide API for the selected guide



.. _Referral Codes:

Referral Codes
--------------




.. _Remove Portal:

Remove Portal
-------------






.. _Settings :

Settings
--------

The following 16 method calls (SG-01 to SG-16) are made by PEER in connection with the variety of functions and administrative options that are managed from the Settings screen in the PEER Admin, shown here: 

.. image:: https://s3.amazonaws.com/peer-downloads/images/TechDocs/Methods+SG-01+-+16.png

.. _General settings:

General settings
^^^^^^^^^^^^^^^^

.. _Method SG-01:

The first of these methods (SG-01) is invoked upon clicking on the General Settings menu item.  

**Method SG-01:**

  **getAllSeekerTemplates**
| **tblPlseekerTemplateService.getAllSeekerTemplate**
  
**Source files:**

  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/controller/SeekerTemplateController.java
  
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/service/impl/TblPlseekerTemplateServiceImpl.java
  
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/dao/impl/TblPlseekerTemplateDaoImpl.java

**Database table:**

  dbPPMS_D.tblPLSeekerTemplate

.. _Save general settings:

Saving general settings
^^^^^^^^^^^^^^^^^^^^^^^

The next five methods (SG-02 to SG-06) are invoked when an administrative user clicks on the Save button at the bottom of the General Settings window in PEER:

.. _Method SG-02:

**Method SG-02:**

  **savePortal**

**Source files:**

  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/controller/PortalsController.java
  
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/service/impl/PortalServiceImpl.java
  
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/dao/impl/TblLandingPagesDaoImpl.java
  
**Database tables:**
  
  * dbPPMS_D.tblLandingPages
  * dbPPMS_D.tblWidgetPrivacyDirectives  


.. _Method SG-03:

**Method SG-03:**

   **getAllPortals**
	
**Source files:**

  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/controller/PortalsController.java
  
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/models/TblPeerAccount.java
  
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/dao/impl/TblPeerAccountDaoImpl.java
  
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/service/PortalService.java
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/service/impl/PortalServiceImpl.java
  
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/dao/impl/TblPortalAdminMappingDaoImpl.java

**Database tables:**
  
  * dbPPMS_D.tblPeerAccount
  * dbPPMS_D.tblPortalAdminMapping


.. _Method SG-04:

**Method SG-04:**

     **updateDateForPortalparameters**
     
**Source files:**

  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/controller/PortalsController.java
  
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/service/impl/PortalServiceImpl.java
  
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/models/TblLandingPages.java
  
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/dao/TblLandingPagesDao.java
  
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/dao/impl/TblLandingPagesDaoImpl.java
  
**Database tables:**
  
  * dbPPMS_D.tblLandingPages


.. _Method SG-05:

**Method SG-05:**

     **getPortalAssociateOrganizations**
	
**Source files:**

  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/controller/PortalsController.java
  
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/models/TblPeerAccount.java
  
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/dao/impl/TblPeerAccountDaoImpl.java
  
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/service/impl/PortalServiceImpl.java
  
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/models/ViewPortalDetails.java

**Database tables:**
  
  * dbPPMS_D.tblPeerAccount
  * dbPPMS_D.tblPortalAdminMapping


.. _Method SG-06:

**Method SG-06:**

     **getPendingOrganizationMemberByOrganizationIds**
	
**Source files:**

  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/controller/OrganizationMemberController.java
  
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/service/OrganizationMemberService.java
  
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/service/impl/OrganizationMemberServiceImpl.java
  
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/dao/impl/TblShaOrganizationMemberDaoImpl.java

**Database tables:**
  
  * dbPPMS_D.tblShaOrganizationMember
  * dbPPMS_D.tblShaOrganization

.. _Recommended organizations:

Recommended organizations
^^^^^^^^^^^^^^^^^^^^^^^^^

The following three methods (SG-07 to SG-09) are invoked when an administrative user clicks on the Recommended Organizations menu item.

.. _Method SG-07:

**Method SG-07:**

     **getWidgetInfoByPortalId**
	
**Source files:**

  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/controller/WidgetInfoController.java
  
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/service/impl/WidgetInfoServiceImpl.java
  
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/models/TblWidgetInfo.java
  
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/dao/TblWidgetInfoDao.java
  
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/dao/impl/TblWidgetInfoDaoImpl.java

**Database tables:**
  
  * dbPPMS_D.tblWidgetInfo
  * dbPPMS_D.tblPeerAccount
  * dbPPMS_D.tblWidgetTheme
  * dbPPMS_D.tblWidgetDemo


.. _Method SG-08:

**Method SG-08:**

     **getAllOrganizationName**
	
**Source files:**

  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/controller/ShaOrganizationController.java
  
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/models/TblShaOrganization.java
  
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/dao/TblShaOrganizationDao.java
  
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/dao/impl/TblShaOrganizationDaoImpl.java
  
**Database tables:**
  
  * dbPPMS_D.tblShaOrganization
  * dbPPMS_D.tblShaOrganizationPrivacyDirective
  * dbPPMS_D.tblShaOrganizationPreference
  * dbPPMS_D.tblShaOrganizationType


.. _Method SG-09:

**Method SG-09:**

     **getOrganizationsByLandinPageId**
	
**Source files:**

  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/service/LandingPagesRecommendedOrganizationsService.java
  
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/models/TblLandingPagesRecommendedOrganizations.java

**Database tables:**
  
  * dbPPMS_D.tblLandingPagesRecommendedOrganizations


.. _Update Organ Info:

Update Organizational Info
^^^^^^^^^^^^^^^^^^^^^^^^^^

And the final six methods (SG-10 to SG-16) are invoked upon an administrative user clicking on the Update Organization Information item on the Settings sub-menu.

.. _Method SG-10:

**Method SG-10:**

     **getPortalPrivacyDirectives**
	
**Source files:**

  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/controller/PortalsController.java
  
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/service/impl/PortalServiceImpl.java
  
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/models/TblLandingPages.java
  
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/dao/impl/TblLandingPagesDaoImpl.java
  
**Database tables:**
  
  * dbPPMS_D.tblWidgetPrivacyDirective
  * dbPPMS_D.tblLandingPages


.. _Method SG-11:

**Method SG-11:**

     **updateLastModifiedPortal**
	
**Source files:**

  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/controller/PortalsController.java
  
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/service/impl/PortalServiceImpl.java
  
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/models/TblLandingPages.java
  
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/dao/impl/TblLandingPagesDaoImpl.java
  
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/models/TblWidgetPrivacyDirective.java
  
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/dao/TblWidgetPrivacyDirectiveDao.java
  
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/dao/impl/TblWidgetPrivacyDirectiveDaoImpl.java

**Database tables:**
  
  * dbPPMS_D.tblWidgetPrivacyDirective
  * dbPPMS_D.tblLandingPages


.. _Method SG-12:

**Method SG-12:**

     **getAllSeekerTemplates**
	
**Source files:**

  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/controller/SeekerTemplateController.java
  
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/service/TblPlseekerTemplateService.java
  
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/service/impl/TblPlseekerTemplateServiceImpl.java
  
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/dao/TblPlseekerTemplateDao.java
  
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/dao/impl/TblPlseekerTemplateDaoImpl.java

**Database tables:**
  
  * dbPPMS_D.tblPLSeekerTemplate


.. _Method SG-13:

**Method SG-13:**

     **getSeekerGroupNames  **
	
**Source files:**

  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/controller/SeekerGroupController.java
  
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/service/SeekerGroupService.java
  
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/service/impl/SeekerGroupServiceImpl.java
  
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/dao/TblSeekerGroupDao.java
  
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/dao/impl/TblSeekerGroupDaoImpl.java

**Database tables:**
  
  * dbPPMS_D.tblSeekerGroup


.. _Method SG-14:

**Method SG-14:**

     **getAllOrganizationName**
	
**Source files:**

  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/controller/ShaOrganizationController.java
  
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/models/TblShaOrganization.java
  
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/dao/TblShaOrganizationDao.java
  
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/dao/impl/TblShaOrganizationDaoImpl.java


.. _Method SG-15:

**Method SG-15:**

     **getPortalPrivacyDirectives **
	
**Source files:**

  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/controller/PortalsController.java
  
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/service/impl/PortalServiceImpl.java
  
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/models/TblWidgetPrivacyDirectiveType.java
  
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/dao/TblWidgetPrivacyDirectiveDao.java

**Database tables:**
  
  * dbPPMS_D.tblWidgetPrivacyDirective  
  * dbPPMS_D.tblWidgetPrivacyDirectiveType


.. _Method SG-16:

**Method SG-16:**

     **getOrganization**
	
**Source files:**

  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/models/TblShaOrganization.java
  
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/dao/TblShaOrganizationDao.java
  
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/dao/impl/TblShaOrganizationDaoImpl.java
  
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/models/TblShaOrganizationType.java
  
**Database tables:**
  
  * dbPPMS_D.tblShaOrganization
  * dbPPMS_D.tblShaOrganizationType



.. _Statistics :

Statistics
----------



.. _Surveys :

Surveys
-------




.. _Theme :

Theme
-----

The following five methods (TH-01 to TH-05) are invoked when an authorized user selects the Theme menu item from the administrative menu.

.. _Method TH-01:

**Method TH-01:**

     **getWidgetInfoByPortalId**
	
**Source files:**

  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/controller/WidgetInfoController.java
  
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/service/impl/WidgetInfoServiceImpl.java
  
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/models/TblWidgetInfo.java
  
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/dao/TblWidgetInfoDao.java
  
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/dao/impl/TblWidgetInfoDaoImpl.java
  
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/webapp/WEB-INF/resources/json/defaultTheme.json
  
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/controller/WidgetThemeController.java

**Database tables:**
  
  * dbPPMS_D.tblWidgetInfo
  * dbPPMS_D.tblPeerAccount
  * dbPPMS_D.tblWidgetTheme
  * dbPPMS_D.tblWidgetDemo
  
  
.. _Method TH-02:

**Method TH-02:**

     **getWidgetInfoDetails**
	
**Source files:**
  
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/controller/WidgetThemeController.java
  
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/service/WidgetThemeService.java
  
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/service/impl/WidgetThemeServiceImpl.java

**Database tables:**
  
  * dbPPMS_D.tblWidgetTheme

  
.. _Method TH-03:
  
**Method TH-03:**

     **getWidgetInfoDetails**
	
**Source files:**
  
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/controller/WidgetInfoController.java
  
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/service/impl/WidgetInfoServiceImpl.java
  
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/dao/TblWidgetInfoDao.java
  
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/dao/impl/TblWidgetInfoDaoImpl.java

**Database tables:**
  
  * dbPPMS_D.tblWidgetInfo
  * dbPPMS_D.tblPeerAccount
  * dbPPMS_D.tblWidgetTheme
  * dbPPMS_D.tblWidgetDemo

  
.. _Method TH-04:

**Method TH-04:**

     **cssUpload**
	
**Source files:**
  
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/controller/WidgetInfoController.java
  
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/service/impl/WidgetInfoServiceImpl.java
  
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/dao/TblWidgetInfoDao.java
  
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/dao/impl/TblWidgetInfoDaoImpl.java
  
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/service/WidgetDemoService.java
  
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/service/impl/WidgetDemoServiceImpl.java
  
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/dao/TblWidgetDemoDao.java
  
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/dao/impl/TblWidgetDemoDaoImpl.java

  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/models/TblLandingPages.java
  
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/dao/TblLandingPagesDao.java
  
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/dao/impl/TblLandingPagesDaoImpl.java
  
**Database tables:**
  
  * dbPPMS_D.tblWidgetInfo
  * dbPPMS_D.tblWidgetDemo
  * dbPPMS_D.tblLandingPages

  
.. _Method TH-05:

**Method TH-05:**

     **updateWidgetInfoDetails**
	
**Source files:**
  
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/controller/WidgetInfoController.java
  
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/service/impl/WidgetInfoServiceImpl.java
  
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/dao/TblWidgetInfoDao.java
  
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/dao/impl/TblWidgetInfoDaoImpl.java
  
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/service/impl/WidgetDemoServiceImpl.java
  
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/dao/TblWidgetDemoDao.java
  
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/dao/impl/TblWidgetDemoDaoImpl.java

**Database tables:**
  
  * dbPPMS_D.tblWidgetInfo
  * dbPPMS_D.tblWidgetDemo


.. _Users and Permissions:

Users and permissions
---------------------

PEER's administrative features can be provisioned into the following four levels:

PEER Super Admin
^^^^^^^^^^^^^^^^

This user(s) has access to all of the administrative functions for any portal in the PEER system, and the ability to designate "Portal Super Admin" user(s).  Currently, all PEER Super Admin users are employees of Genetic Alliance. Notwithstanding such users' ability to access any PEER portal's administrative and support features as part of Genetic Alliance's role as a steward from PEER, to protect the integrity of the user data contributed through these portals, the PEER Super Admin rights *do *not permit access* to any end user data unless the PEER Super Admin user is (a) explictly authorized by the end user to access their information, or (b) separately granted administrative, staff or researcher level access by the Portal Super Admin.

Portal Super Admin
^^^^^^^^^^^^^^^^^^

This user(s) has the same administrative rights as the PEER Super Admin - *except that* the Portal Super Admin's rights are limited to just their organization's portals. Super Admins can grant rights to other super admin users, staff members and recommended researchers.  This includes the following General Permissions authority:
  
       * General settings
       * Portal code
       * Theme and landing page
       * Surveys
       * Guides
       * Referral codes
       * Update organization information
       * Delete protals

   It also includes the following data access permissions:

         * View aggregate data
         * View individal data
         * Edit individals data
         * Export survey resppnses
         * View/download contact information
         * Proxy agent
      
Staff member
^^^^^^^^^^^^

This user(s) is designated rights for any portals that the Administrator has the authority to manage, and may be granted any of the foregoing rights except for the right to delegate rights to other users and the right to edit individual user data

Recommended researcher
^^^^^^^^^^^^^^^^^^^^^^

This user(s) is designated data access rights for any portals that the Administrator has the authority to manage, but the Super Admin is not able to provision researchers with *any* of the General Permissions, or the right to edit individual data, or designate them as a proxy agent.
 
In all of the foregoing cases, the [PEER or Portal] Super Administrator can either assign portal administration rights to an existing account or approve requests to set up a new account.  

.. Note:: At the conclusion of the migration, we will need to confirm with Genetic Alliance whether they wish for us to remove Private Acccess from having Super Administrative rights over the PEER Admin, and if so the relationship they prefer to replace this as a safety net for Genetic Alliance's personnel.

.. _Method UP-01:

**Method UP-01:**

  **getPortalUsers**

**Purpose or Use:**
  
.. Attention:: Need to add a description for the foregoing method.
  
**Source files:**

  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/controller/PrivateAccessController.java
  
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/service/impl/PortalAdminMappingServiceImpl.java
  
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/models/TblPortalAdminMapping.java
  
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/dao/impl/TblPortalAdminMappingDaoImpl.java
  
**Database table** 

  * dbPPMS_D.tblPortalAdminMapping


.. _View portal:

View portal
-----------

The following method (VW-01) is used to create the block of HTML code that, when posted as instructed on a website page, will display the fully configured PEER portal on the sponsor's website.

.. _Method VW-01:

**Method VW-01:**

     **getWidgetInfoByPortalId    **
	
**Source files:**

  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/controller/WidgetInfoController.java  
  
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/models/TblWidgetInfo.java
  
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/dao/TblWidgetInfoDao.java
  
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/dao/impl/TblWidgetInfoDaoImpl.java
  
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/dao/TblWidgetDemoDao.java
  
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/dao/impl/TblWidgetDemoDaoImpl.java
  
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/service/WidgetInfoService.java
  
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/service/impl/WidgetInfoServiceImpl.java
  
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/service/WidgetDemoService.java
  
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/service/impl/WidgetDemoServiceImpl.java

**Database tables:**
  
  * dbPPMS_D.tblWidgetInfo
  * dbPPMS_D.tblWidgetDemo

Search service
==============
