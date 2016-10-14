
.. _Architecture top:

Architecture
============

This section of the document focuses on the overall architecture of PEER, beginning from a macro level and extending to the depth of the methods, source files and database tables that are presently being used by the application.  

.. Note:: In the interest of time, the present documentation generally does not include function calls, nor list the inputs and outputs based on each method.  Future enhancement to the technical documentation should consider adding this inventory.  

This descriptions that follow generally focus on the PEER open source code, and include (for reference purposes) information respecting the resources that are provided through API calls to PA Connect and PrivacyLayer, both of which are independent services licensed by Genetic Alliance from Private Access for inclusion in Genetic Alliance's SaaS-based version of PEER.

The overall architecture of the PEER application is portayed in the following diagram:

.. _PEER Architecture:

.. image:: https://s3.amazonaws.com/peer-downloads/images/TechDocs/PEER+High-Level+Architecture.png
     :alt: High-Level PEER Architecture Illustration  

The authentication, single sign-on and privacy directives set by individual participants respecting who can access their information and for what purposes is acquired through API calls to access the services portrayed in the following diagram:

.. _PA Architecture:

.. image:: https://s3.amazonaws.com/peer-downloads/images/TechDocs/Private+Access+High-Level+Architecture.png
     :alt: High-Level Private Access Services Illustration  


.. _Top level:

Top Level
~~~~~~~~~

As shown above, at the highest level, PEER is comprised of two components, one that comprises all of its administrative functions, and a second for participant portals. These components are complemented by the services that PEER acquires via API from PA Connect and PrivacyLayer.  At the present time, PEER functions from four databases:

 * **peer_surveys** - contains all administrative functions for surveys
 * **peer_surveys_published** - contains all PST Surveys information
 * **dbPPMS** - contains (for the production environment) the administrative components of PEER, as well as Private Access' Privacy Preferences Management System (or PPMS)
 * **dbPPMS_D** - contains (for the beta environment) the administrative components of PEER, as well as the PPMS

.. Important:: As part of migrating the PEER source code to open source, the PEER and Private Access components will be divided into separate databases so that the data tables that are exclusively used by PEER will be physically separated from the data tables that are used by the Private Access service.  At the conclusion of this work, we anticipate that the dbPPMS and dbPPMS_D will be used exclusively by Private Access (and not included in the OSS), and a third database for the administrative components of PEER and the API calls to Private Access will be reflected in the top level architecture.

Admin Component
~~~~~~~~~~~~~~~

The administration component of PEER is used by properly authorized and authenticated users to create and manage participant portals, as well as (to the extent permitted by each participant's privacy preference setttings) to discover, view and export data that is acquired through the operation of such portals.  

As illustrated above, the administration component of PEER is comprised of the five functional areas shown in green in the above illustration. These include:

  * Portal administration
  * Account management
  * PA / PEER administration
  * Search service
  * Data export service

These areas are in turn broken into a number of fourth tier functions illustrated in yellow above, and in some cases even further articulated into fifth (teal colored boxes), sixth (grey shaded boxes) and more granular functions.  In order to assist future developers wishing to extend and/or modify PEER's features, the documentation below defines the methods, source files and database tables that are employed in providing these functions. 

.. Important:: The sections below define these elements for several of the more involved portions of the architecture.  These are provided for discussion purposes to ascertain whether this is too detailed, insufficiently detailed or just right.

Portal administration
---------------------

As shown, the portal administration section of the PEER admin contains twelve components that collectively enable the creation, modification and management of the appearance, content, and functional behaviour of the portals under a PEER sponsor's control, as well as the rights of staff members and researchers to access the administrative features and data acquired through the portal (obviously subject to the individuals' privacy and data sharing directives).


.. _users and Permissions:

Users and permissions
---------------------

PEER's administrative features can be provisioned into the following four levels:

  * **PEER Super Admin** - this user(s) has access to all of the administrative functions for any portal in the PEER system, and the ability to designated the "Portal Super Admin" user(s).  Notwithstanding his or her having administrative access to any PEER portal, in order to protect the integrity of such portal's data, the PEER Super Admin rights do *not* permit the individual to access end user data without either (a) being explictly designated by the end user as a recipient of their information, or (b) being separately granted administrative, staff or researcher level access by the Portal Super Admin.
  
  * **Portal Super Admin** - this user(s) has the same administrative rights for their portal(s) as the PEER Super Admin - except that the Portal Super Admin's rights are limited to just their organization's portals. Super Admins can grant rights to other super admin users, staff members and recommended researchers.  This includes the following General Permissions authority:
  
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
      
  * **Staff member** - this user(s) is designated rights for any portals that the Administrator has the authority to manage, and may be granted any of the foregoing rights except for the right to delegate rights to other users and the right to edit individual user data
  
  * **Recommended researcher** - this user(s) is designated data access rights for any portals that the Administrator has the authority to manage, but the Super Admin is not able to provision researchers with *any* of the General Permissions, or the right to edit individual data, or designate them as a proxy agent.
 
In all of the foregoing cases, the [PEER or Portal] Super Administrator can either assign portal administration rights to an existing account or approve requests to set up a new account.  Each such account is assigned one or more Portal Administrators who received from individuals requesting to be a Portal Administrator.  


**Example result**::Method: 
  getPortalUsers
  
Source Files:
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/controller/PrivateAccessController.java
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/service/impl/PortalAdminMappingServiceImpl.java
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/models/TblPortalAdminMapping.java
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/dao/impl/TblPortalAdminMappingDaoImpl.java
  
Databse Tables: 
  dbPPMS_D.tblPortalAdminMapping

Settings
~~~~~~~~

1 Method: 
  getAllSeekerTemplates
  tblPlseekerTemplateService.getAllSeekerTemplate
  
Source Files:
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/controller/SeekerTemplateController.java
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/service/impl/TblPlseekerTemplateServiceImpl.java
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/dao/impl/TblPlseekerTemplateDaoImpl.java

Database Tables: 
  dbPPMS_D.tblPLSeekerTemplate
  
2 Method: 
  savePortal
  
Source Files:
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/controller/PortalsController.java
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/service/impl/PortalServiceImpl.java
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/dao/impl/TblLandingPagesDaoImpl.java

Database Tables:  
  dbPPMS_D.tblLandingPages
  dbPPMS_D.tblWidgetPrivacyDirectives  

4 Method:
	updateDateForPortalparameters
	
Source Files:  
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/controller/PortalsController.java
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/service/impl/PortalServiceImpl.java
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/models/TblLandingPages.java
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/dao/TblLandingPagesDao.java
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/dao/impl/TblLandingPagesDaoImpl.java
  
Database Tables:  
  dbPPMS_D.tblLandingPages

3 Method:
	getAllPortals
	
Source Files:  
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/controller/PortalsController.java
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/models/TblPeerAccount.java
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/dao/impl/TblPeerAccountDaoImpl.java
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/service/PortalService.java
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/service/impl/PortalServiceImpl.java
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/dao/impl/TblPortalAdminMappingDaoImpl.java

Database Tables:  
  dbPPMS_D.tblPeerAccount
  dbPPMS_D.tblPortalAdminMapping

5 Method:
	getPortalAssociateOrganizations

Source Files:    
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/controller/PortalsController.java
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/models/TblPeerAccount.java
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/dao/impl/TblPeerAccountDaoImpl.java
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/service/impl/PortalServiceImpl.java
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/models/ViewPortalDetails.java
  
Database Tables:  
  dbPPMS_D.tblPeerAccount
  dbPPMS_D.tblPortalAdminMapping

6 Method:
  getPendingOrganizationMemberByOrganizationIds
  
Source Files:    
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/controller/OrganizationMemberController.java
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/service/OrganizationMemberService.java
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/service/impl/OrganizationMemberServiceImpl.java
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/dao/impl/TblShaOrganizationMemberDaoImpl.java
  
Database Tables: 
  dbPPMS_D.tblShaOrganizationMember
  dbPPMS_D.tblShaOrganization

7 Method:
  getWidgetInfoByPortalId
  
Source Files: 
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/controller/WidgetInfoController.java
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/service/impl/WidgetInfoServiceImpl.java
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/models/TblWidgetInfo.java
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/dao/TblWidgetInfoDao.java
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/dao/impl/TblWidgetInfoDaoImpl.java

Database Tables: 
  dbPPMS_D.tblWidgetInfo
  dbPPMS_D.tblPeerAccount
  dbPPMS_D.tblWidgetTheme
  dbPPMS_D.tblWidgetDemo

8 Method:
  getAllOrganizationName

Source Files:   
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/controller/ShaOrganizationController.java
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/models/TblShaOrganization.java
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/dao/TblShaOrganizationDao.java
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/dao/impl/TblShaOrganizationDaoImpl.java
  
Database Tables: 
  dbPPMS_D.tblShaOrganization
  dbPPMS_D.tblShaOrganizationPrivacyDirective
  dbPPMS_D.tblShaOrganizationPreference
  dbPPMS_D.tblShaOrganizationType

9 Method:
  getOrganizationsByLandinPageId

Source Files:       
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/service/LandingPagesRecommendedOrganizationsService.java
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/models/TblLandingPagesRecommendedOrganizations.java

Database Tables: 
  dbPPMS_D.tblLandingPagesRecommendedOrganizations

10 Method:
  getPortalPrivacyDirectives

Source Files:       
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/controller/PortalsController.java
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/service/impl/PortalServiceImpl.java
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/models/TblLandingPages.java
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/dao/impl/TblLandingPagesDaoImpl.java
  
Database Tables: 
  dbPPMS_D.tblWidgetPrivacyDirective
  dbPPMS_D.tblLandingPages

11 Method:
  updateLastModifiedPortal

Source Files:       
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/controller/PortalsController.java
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/service/impl/PortalServiceImpl.java
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/models/TblLandingPages.java
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/dao/impl/TblLandingPagesDaoImpl.java
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/models/TblWidgetPrivacyDirective.java
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/dao/TblWidgetPrivacyDirectiveDao.java
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/dao/impl/TblWidgetPrivacyDirectiveDaoImpl.java

Database Tables: 
  dbPPMS_D.tblWidgetPrivacyDirective
  dbPPMS_D.tblLandingPages

12 Method:
  getAllSeekerTemplates

Source Files:       
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/controller/SeekerTemplateController.java
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/service/TblPlseekerTemplateService.java
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/service/impl/TblPlseekerTemplateServiceImpl.java
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/dao/TblPlseekerTemplateDao.java
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/dao/impl/TblPlseekerTemplateDaoImpl.java

Database Tables: 
  dbPPMS_D.tblPLSeekerTemplate

13 Method:
  getSeekerGroupNames  
  
Source Files:
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/controller/SeekerGroupController.java
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/service/SeekerGroupService.java
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/service/impl/SeekerGroupServiceImpl.java
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/dao/TblSeekerGroupDao.java
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/dao/impl/TblSeekerGroupDaoImpl.java

Database Tables: 
  dbPPMS_D.tblSeekerGroup

14 Method:
  getAllOrganizationName

Source Files:   
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/controller/ShaOrganizationController.java
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/models/TblShaOrganization.java
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/dao/TblShaOrganizationDao.java
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/dao/impl/TblShaOrganizationDaoImpl.java
  
15 Method:
  getPortalPrivacyDirectives  
  
Source Files:
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/controller/PortalsController.java
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/service/impl/PortalServiceImpl.java
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/models/TblWidgetPrivacyDirectiveType.java
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/dao/TblWidgetPrivacyDirectiveDao.java
  
Database Tables: 
  dbPPMS_D.tblWidgetPrivacyDirective  
  dbPPMS_D.tblWidgetPrivacyDirectiveType
  
16 Method:
  getOrganization
  
Source Files:
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/models/TblShaOrganization.java
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/dao/TblShaOrganizationDao.java
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/dao/impl/TblShaOrganizationDaoImpl.java
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/models/TblShaOrganizationType.java
  
Database Tables:
  dbPPMS_D.tblShaOrganization
  dbPPMS_D.tblShaOrganizationType

View Portal
===========
Method:
  
1 Method: (Get Code for Website)
  getWidgetInfoByPortalId    
  
Source Files:
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

Database Tables: 
  dbPPMS_D.tblWidgetInfo
  dbPPMS_D.tblWidgetDemo


Participant portal
==================





.. attention: Remove PA Administrative access as a superior level to the PEER Administrator

Calls the Edit Guide API for the selected guide
