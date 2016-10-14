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

Method: 
  getPortalUsers
  
Source Files:
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/controller/PrivateAccessController.java
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/service/impl/PortalAdminMappingServiceImpl.java
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/models/TblPortalAdminMapping.java
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/dao/impl/TblPortalAdminMappingDaoImpl.java
  
Databse Tables: 
  dbPPMS_D.tblPortalAdminMapping

Settings
========

Method: 
  getAllSeekerTemplates
  tblPlseekerTemplateService.getAllSeekerTemplate
  
Source Files:
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/controller/SeekerTemplateController.java
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/service/impl/TblPlseekerTemplateServiceImpl.java
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/dao/impl/TblPlseekerTemplateDaoImpl.java

Database Tables: 
  dbPPMS_D.tblPLSeekerTemplate
  
Method: 
  savePortal
  
Source Files:
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/controller/PortalsController.java
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/service/impl/PortalServiceImpl.java
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/dao/impl/TblLandingPagesDaoImpl.java

Database Tables:  
  dbPPMS_D.tblLandingPages
  dbPPMS_D.tblWidgetPrivacyDirectives  

Method:
	updateDateForPortalparameters
	
Source Files:  
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/controller/PortalsController.java
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/service/impl/PortalServiceImpl.java
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/models/TblLandingPages.java
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/dao/TblLandingPagesDao.java
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/dao/impl/TblLandingPagesDaoImpl.java
  
Database Tables:  
  dbPPMS_D.tblLandingPages

Method:
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

Method:
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

Method:
  getPortalAssociateOrganizations
  
Source Files:    
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/controller/OrganizationMemberController.java
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/service/OrganizationMemberService.java
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/service/impl/OrganizationMemberServiceImpl.java
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/dao/impl/TblShaOrganizationMemberDaoImpl.java
  
Database Tables: 
  dbPPMS_D.tblShaOrganizationMember
  dbPPMS_D.tblShaOrganization

Method:
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

Method:
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

Method:
  getOrganizationsByLandinPageId

Source Files:       
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/service/LandingPagesRecommendedOrganizationsService.java
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/models/TblLandingPagesRecommendedOrganizations.java

Database Tables: 
  dbPPMS_D.tblLandingPagesRecommendedOrganizations

Method:
  getPortalPrivacyDirectives

Source Files:       
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/controller/PortalsController.java
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/service/impl/PortalServiceImpl.java
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/models/TblLandingPages.java
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/dao/impl/TblLandingPagesDaoImpl.java
  
Database Tables: 
  dbPPMS_D.tblWidgetPrivacyDirective
  dbPPMS_D.tblLandingPages

Method:
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

Method:
  getAllSeekerTemplates

Source Files:       
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/controller/SeekerTemplateController.java
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/service/TblPlseekerTemplateService.java
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/service/impl/TblPlseekerTemplateServiceImpl.java
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/dao/TblPlseekerTemplateDao.java
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/dao/impl/TblPlseekerTemplateDaoImpl.java

Database Tables: 
  dbPPMS_D.tblPLSeekerTemplate

Method:
  getSeekerGroupNames  
  
Source Files:
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/controller/SeekerGroupController.java
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/service/SeekerGroupService.java
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/service/impl/SeekerGroupServiceImpl.java
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/dao/TblSeekerGroupDao.java
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/dao/impl/TblSeekerGroupDaoImpl.java

Database Tables: 
  dbPPMS_D.tblSeekerGroup

Method:
  getPortalPrivacyDirectives  
  
Source Files:
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/controller/PortalsController.java
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/service/impl/PortalServiceImpl.java
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/models/TblWidgetPrivacyDirectiveType.java
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/dao/TblWidgetPrivacyDirectiveDao.java
  
Database Tables: 
  dbPPMS_D.tblWidgetPrivacyDirective  
  dbPPMS_D.tblWidgetPrivacyDirectiveType
  
Method:
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
