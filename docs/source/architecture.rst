
.. _Architecture top:

Architecture
============

This section of the document focuses on the overall architecture of PEER, beginning from a macro level and extending to the depth of the methods, source files and database tables that are presently being used by the application.  

.. Note:: In the interest of time, the present documentation generally does not include function calls, nor list the inputs and outputs based on each method.  Future enhancement to the technical documentation should consider adding this inventory.  

This descriptions that follow generally focus on the PEER open source code, and include (for reference purposes) information respecting the resources that are provided through API calls to PA Connect and PrivacyLayer, both of which are independent services licensed by Genetic Alliance from Private Access for inclusion in Genetic Alliance's SaaS-based version of PEER.

.. _General orientation:

General orientation
~~~~~~~~~~~~~~~~~~~

The overall architecture of the PEER application is portayed in the following diagram:

.. _PEER Architecture:

.. image:: https://s3.amazonaws.com/peer-downloads/images/TechDocs/PEER+High-Level+Architecture.png
     :alt: High-Level PEER Architecture Illustration  

The authentication, single sign-on and privacy directives set by individual participants respecting who can access their information and for what purposes is acquired through API calls to access the services portrayed in the following diagram:

.. _PA Architecture:

.. image:: https://s3.amazonaws.com/peer-downloads/images/TechDocs/Private+Access+High-Level+Architecture.png
     :alt: High-Level Private Access Services Illustration  


As shown above, at the highest level, PEER is comprised of two components, one that comprises all of its administrative functions, and a second for participant portals. These components are complemented by the services that PEER acquires via API from PA Connect and PrivacyLayer.  At the present time, PEER functions from four databases:

 * **peer_surveys** - contains all administrative functions for surveys
 * **peer_surveys_published** - contains all PST Surveys information
 * **dbPPMS** - contains (for the production environment) the administrative components of PEER, as well as Private Access' Privacy Preferences Management System (or PPMS)
 * **dbPPMS_D** - contains (for the beta environment) the administrative components of PEER, as well as the PPMS

.. Attention:: As part of migrating the PEER source code to open source, the PEER and Private Access components will be divided into separate databases so that the data tables that are exclusively used by PEER will be physically separated from the data tables that are used by the Private Access service.  At the conclusion of this work, we anticipate that the dbPPMS and dbPPMS_D will be used exclusively by Private Access (and not included in the OSS), and a third database for the administrative components of PEER and the API calls to Private Access will be reflected in the top level architecture.

Administrative component
~~~~~~~~~~~~~~~~~~~~~~~~

The administration component of PEER is used by properly authorized and authenticated users to create and manage participant portals, as well as (to the extent permitted by each participant's privacy preference setttings) to discover, view and export data that is acquired through the operation of such portals.  

As illustrated above, the administrative component of PEER is comprised of the five functional areas shown in green in the above illustration. These include:

  * Portal administration
  * Account management
  * PA / PEER administration
  * Search service
  * Data export service

These areas are in turn broken into a number of fourth tier functions illustrated in yellow above, and in some cases even further articulated into fifth (teal colored boxes), sixth (grey shaded boxes) and more granular functions.  In order to assist future developers wishing to extend and/or modify PEER's features, the documentation below defines the methods, source files and database tables that are employed in providing these functions. 

.. Attention:: The sections below define these elements for several of the more involved portions of the architecture.  These are provided for discussion purposes to ascertain whether this is too detailed, insufficiently detailed or just right.

Portal administration
---------------------

As shown, the portal administration section of the PEER admin contains twelve components that collectively enable the creation, modification and management of the appearance, content, and functional behaviour of the portals under a PEER sponsor's control, as well as the rights of staff members and researchers to access the administrative features and data acquired through the portal (obviously subject to the individuals' privacy and data sharing directives).


.. _Users and Permissions:

Users and permissions
---------------------

PEER's administrative features can be provisioned into the following four levels:

  * **PEER Super Admin** - this user(s) has access to all of the administrative functions for any portal in the PEER system, and the ability to designate "Portal Super Admin" user(s).  Currently, all PEER Super Admin users are employees of Genetic Alliance. Notwithstanding such users' ability to access any PEER portal's administrative and support features as part of Genetic Alliance's role as a steward from PEER, to protect the integrity of the user data contributed through these portals, the PEER Super Admin rights *do *not permit access* to any end user data unless the PEER Super Admin user is (a) explictly authorized by the end user to access their information, or (b) separately granted administrative, staff or researcher level access by the Portal Super Admin.

  * **Portal Super Admin** - this user(s) has the same administrative rights as the PEER Super Admin - '_except that_' the Portal Super Admin's rights are limited to just their organization's portals. Super Admins can grant rights to other super admin users, staff members and recommended researchers.  This includes the following General Permissions authority:
  
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


.. _Settings :

Settings
~~~~~~~~

The following 14 method calls are made by PEER in connection with the variety of functions and administrative options that are managed from the Settings screen in the PEER Admin, shown here: 

.. image:: https://s3.amazonaws.com/peer-downloads/images/TechDocs/Methods+SG-01+-+16.png

.. _General settings:

General settings
----------------

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
-----------------------

The next five methods are invoked when an administrative user clicks on the Save button at the bottom of the General Settings window in PEER:

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
  

View portal
~~~~~~~~~~~

The following methods are used to get the block of HTML code that, when posted as instructed, will display the fully configured PEER portal on the sponsor's website.  

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

Themes
~~~~~~

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


Feature
~~~~~~~

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
  

Participant portal
~~~~~~~~~~~~~~~~~~


Calls the Edit Guide API for the selected guide
