.. _Searching 

Searching for data in PEER
**************************

As described in the high-level overview of PEER's :ref:`Architecture`, any authorized PEER user (*i.e.*, any authenticated persons with the proper administrative and/or researcher privileges, and PEER participants respecting his or her own data) is able to initiate search queries respecting PEER data.  

Whereas individual participants are supported in searching their responses from the My data section of the PEER portal; all administrative and researcher search queries are initiated from the Search Data menu item located on the PEER for Research administrative dashboard. As illustrated below, clicking on Search Registry Data button on the main menu opens the Search Data page.

when an authorized user clicks on the View Results button on the Search Data page illustrated below.

.. _Search data illustration:

.. image:: https://s3.amazonaws.com/peer-downloads/images/TechDocs/Methods+SD-01+-+05.png
     :alt: Administrative Screen for Searching PEER Data 

.. _Search registry data:

Search Registry Data
--------------------

Clicking on the Search Registry Data menu item shown above invokes three API calls and corresponding method calls, SD-01 to SD-03, as follows: 

.. _Method SD-01:

SD-01:  getrsrequestdetail
^^^^^^^^^^^^^^^^^^^^^^^^^^

**Source files:**
  
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/controller/ProfileInfoController.java
  
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/service/ContactService.java
  
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/service/impl/ContactServiceImpl.java
  
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/service/impl/RsProfileInfo.java
  

.. _Method SD-02:

SD-02:  getAllPortals
^^^^^^^^^^^^^^^^^^^^^

**Source files:**
  
  TBD
  
**Database tables:**

  TBD

.. Attention:: Clicking on the Search Registry Data menu presently calls the getAllPortals method, but from a different API (/private-access-adminportal/services/profile/getStudyTrialList/8293) than the getAllPortals method for the administrative home page (see above). This should be disambiguated during code cleanup.

.. Note::  The page title for "Search Participant Data" results (after clicking on View Participants button) improperly displays the title "Search Survey Data". The grey on grey font color and background color used in the pull-down menu for the selection of type of search is difficult to read, and should also be modified during the clean up project. 


.. _Method SD-03:

SD-03:  dashBoardAPI.php
^^^^^^^^^^^^^^^^^^^^^^^^

Once a PEER portal is selected by use of the pulldown menu in the Search Survey Data screen Calls this to get a list of surveys for the selected portal
	
**Source files:**
  
  cron/dashBoardAPI.php
  includes/functions.php
  admin/classes/PAPermissions.php
  
**Database tables:**
  
  * peer_surveys_published.questions  
  * peer_surveys_published.users
  * peer_surveys_published.data
  * peer_surveys_published.surveys
  * peer_surveys_published.assign_survey
  * peer_surveys_published.survey_instance_data
  

.. _Method SD-04:

SD-04:  getProfileDetails
^^^^^^^^^^^^^^^^^^^^^^^^^

**Inputs**::	 
	
   {  
    **"foreignkeys"**:[  
    ],
    **"accessToken"**:null,
    **"userAccountId"**:"(Integer)"
   }

**Outputs:**
  
An example is provided below of one (of n) data elements that is returned from the foregoing method call::
  
   {  
    "status":"success",
    "message":"success",
    "isSuccess":true,
    "count":###
    "data":[  
       {  
          "foreignKey":"########",
          "participantFirstname":"FIRST_NAME",
          "participantLastname":"LAST_NAME",
          "access":"allow",
          "city":"CITY",
          "state":"STATE",
          "country":"COUNTRY_CODE",
          "surveyStatus":null,
          "response":null,
          "subjectId":########,
          "address1":"ADDRESS1",
          "address2":"ADDRESS2",
          "address3":"ADDRESS3",
          "cellPhone":"",
          "homePhone":"HOME_PHONE",
          "email":"EMAIL",
          "idRequest":null,
          "age":"AGE",
          "dateCreated":"TIMESTAMP",
          "profileType":"Child (Living)",
          "exportAccess":null,
          "dob":"DOB",
          "profileZipCode":"PROFILE_ZIPCODE",
          "accountZipCode":"ACCOUNT_ZIPCODE",
          "contactFirstName":"CONTACT_FIRST_NAME",
          "contactLastName":"CONTACT_LAST_NAME",
          "contactCity":"CONTACT_CITY",
          "contactState":"CONTACT_STATE",
          "contactCountry":"CONTACT_COUNTRY_CODE"
       },
	  ...
     ]
   }

Related Function Calls
^^^^^^^^^^^^^^^^^^^^^^

SD-05:  ProfileDetailsRequest.getForeignkeys()
""""""""""""""""""""""""""""""""""""""""""""""

This function extracts any foreign keys that the administrator provides as part of his or her query. This list will constrain the results to focus only on these individual participants
    
**Inputs**

  n/a 
 
**Outputs**

  List <String> foreignkeys
	

SD-06: ProfileDetailsRequest.getAccessToken() 
"""""""""""""""""""""""""""""""""""""""""""""    

This function retrieves the access token for the individual making the inquiry from their profile details.  The token is unique to the individual *and* unique for each session of the individual's login.

**Inputs**

  n/a
 
**Outputs**

  String token
	
SD-07: OIDCAuthenticationToken.getAccessTokenValue()
""""""""""""""""""""""""""""""""""""""""""""""""""""    

This function also retrieves an access token for the data seeker from the OpenID authentication.  

**Inputs**

  n/a
 
**Outputs**

  String token

.. Attention:: It appears from the JAVA code that the OpenID token issued in SD-07 over-writes the token that is received in SD-06.  As part of the code clean-up, we should verify this is done for a meaningful purpose, and not as an accident or a redundant step in the process.
	
SD-08:  ProfileDetailsRequest.getUserAccountId()
""""""""""""""""""""""""""""""""""""""""""""""""  

This function pulls the (internal) user account IDs from all of the user profiles for the portal to which the selected survey pertains, and passes this or these values as an input to other functions, including SD-09.

**Inputs**

  n/a
 
**Outputs**

  Integer userAccountId
	

SD-09:  UserAccountService.findUserAccountById()
"""""""""""""""""""""""""""""""""""""""""""""""" 

This function retrieves all of the user account data from the database for each userAccountId that was returned by SD-08.

**Inputs**

 * TblUserAccount useraccount
 * Integer userAccountId
	  
**Outputs**

 TblUserAccount useraccount
  

SD-10:  TblUserAccountDao.findById()
""""""""""""""""""""""""""""""""""""  

.. _User account object model:

.. image:: https://s3.amazonaws.com/peer-downloads/images/TechDocs/Object+Model+(user_account).png
     :width: 250px
     :align: right
     :alt: userAccount object model 

The function in SD-10 also appears to retrieve all of the user account data from the database, but by employing a different service.  A copy of the data values that are returned in the data model used by JAVA appears in the image at right.  As noted below, each of the values shown in the data model map to the database columns in the userAccount database, and are stored by PEER in an encrypted form.

**Inputs**

 * TblUserAccount useraccount
 * Integer userAccountId
		
**Outputs**

  TblUserAccount  


SD-11: TblUserAccount.getIsActive()
"""""""""""""""""""""""""""""""""""  

This function checks to see whether the account is active or not.  PEER does not presently contain a user function to "inactivate a user account" and so we hypothesie that new accounts are treated as "inactive" until the user has fully completed the confirmation step by returning the token contained in the confirmation email message that is sent to him or her immediately after accepting the EULA.  

.. Attention:: As part of the data integrity work, we need to verify that the foregoing interpretation is correct with respect to inactive accounts, and/or correct this desciption accordingly.

.. Attention:: It appears from the JAVA code that the function call in SD-10 is requesting the same data as the function call in SD-08 and SD-09.  As part of the code clean-up, we should verify this is done for a meaningful purpose, and not as an accident or a redundant step in the process.  

**Inputs**

  n/a
 
**Outputs**

  Boolean


SD-12: TblUserAccount.getLoginName()
""""""""""""""""""""""""""""""""""""

This function retrieves the user name of the accounts returned by SD-10.

**Inputs** 

  n/a
 
**Outputs**

  String loginname


SD-13:  AESCryptoManager.decrypt()
""""""""""""""""""""""""""""""""""

This function decypts the encrypted data returned in the foreging fuctions.

**Inputs**

  String encrypted
	  
**Outputs**

  String decrypted

.. Hint::  At the present time, it appears that all of the PEER account data is encrypted and decrypted using the same encryption algorithm and key.  In the past, however, for security purposes each participant account employed its own unique encryption key.  We may want to relook at the logic behind the two approaches and implement the preferable one.


SD-14:  TblShaSubjetService.getForeignKeIds()
"""""""""""""""""""""""""""""""""""""""""""""   

This function retrieves all of the Foreign Keys for the subjects returned by SD-10.  As noted by reference to the foregoing :ref:`User account object model` illustration, the account data does *not* include the Foreign Key for the user to whom such data pertains.  This approach was taken in order to provide a measure of security in addition to encrypting all of the information. 

**Inputs**

  Integer widgetId
	  
**Outputs**

  LIst<String> foreignkeys
	
SD-15:  ProfileFilterService.getDiscoverableFKids()
"""""""""""""""""""""""""""""""""""""""""""""""""""

This function begins to filter the foregoing results by limiting the full list of foreign keys returned in SD-14 to just those foreign keys for which this data seeker has access rights.

**Inputs**

 * String token
 * List<String> fullforeignkeys
	  
**Outputs**

  List<String>  filteredforeignkeys

	
SD-16: TblShaSubjetService.createProfileInfoRequest()
""""""""""""""""""""""""""""""""""""""""""""""""""""" 

This function creates a new profile request object (*i.e.*, a container) for any profiles that the present data seeker is entitled to discover, and populates the object with just the foreign key for each such profile.

**Inputs**

  List<String> foreignkeys
	  
**Outputs**

  List<ProfileInfoRequest> request

.. Attention:: It appears that the function call in SD-16 is creating a new profile request object (*i.e.*, a container) for any profiles that the data seeker is entitled to discover by making another database call for the same data as the function call in SD-15 and SD-09.  As part of the code clean-up, we should verify this is done for a meaningful purpose, and not as an accident or a redundant step in the process that could be eliminated or done more efficiently from data that has already been retrieved.


SD-17:  ProfileFilterService.getProfileContactDetails()
"""""""""""""""""""""""""""""""""""""""""""""""""""""""  

This function retrieves a list of discoverable profiles with contact details by passing the profile request object created by function SD-16 as a parameter in this contact details call for use in requesting the contact details.

**Inputs**

 * String token
 * String username
 * List<ProfileInfoRequest> request
 * TimeZone timezone
	  
**Outputs**

  List<SubjectDetail> contacts
	 
.. Attention:: Verify that the approach being employed of passing the profile request object created in SD-16 into the profile contact details call in SD-17 is indeed the correct direction for passing the object, and that the function is not inadvertently reversed (*i.e.*, that it shouldn't be passed in the other direction) 

	  
SD-18:  ProfileFilterService.getProfileExportDetails()
""""""""""""""""""""""""""""""""""""""""""""""""""""""

This function call does essentially the same thing as SD-17, but in this case for export details.  SD-18 retrieves a list of discoverable profiles with export rights by passing the profile request object created by function SD-16 as a parameter in this export details call for use in requesting the export of participant data.

**Inputs**

 * String token
 * String username
 * List<ProfileInfoRequest> request
 * TimeZone timezone
	 
**Outputs**

  List<SubjectDetails> subjects


SD-19: getProfileDetails()
""""""""""""""""""""""""""   

This function

**Inputs**

 * String token
 * String username
 * List<ProfileInfoRequest> request
 * const EXPORT
 * Timezone timezone

**Outputs**
	    
  List<SubjectDetails> subjects


SD-20: TblShaSubjetService.getSubjectDetails()
""""""""""""""""""""""""""""""""""""""""""""""   

This function takes the paramaters created for contact information and export in the SD-17 and SD-18, respectively, and sets the export properties with respect to the contact details so that it can render the export file with the appropriate setting in each cell of the export table.  Application of SD-20 enables the values for age, type of profile, and contact details to appear in the export or will fill those cells in the export table with one or two asterisks (*) when those values cannot be exported to the data seeeker.

**Inputs**

 * List<SubjectDetails> contactDetails
 * List<SubjectDetails> exportDetails
	
**Outputs**
	
  List<SubjectDetails> subjects
	  

SD-21: SubjectDetails.getAccess()
"""""""""""""""""""""""""""""""""    

This function

**Inputs**

  n/a
 
**Outputs**

  String access
		
		
SD-22:  SubjectDetails.setExportAccess()
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^    

Where the right to export data is not allowed, this function sets the value for any prohibited data field that cannot be exporteed to "Not allowed".   

**Inputs**

  String exportsetting
		
**Outputs**



SD-23:  SubjectDetails.setAge()
"""""""""""""""""""""""""""""""    

This function

**Inputs**

  String age
		
**Outputs**
	
   	

SD-24:  SubjectDetails.setProfileType()
"""""""""""""""""""""""""""""""""""""""    

This function sets the property for the account type to the reported property type (*e.g.*, Myself, Child, Parent, etc)

**Inputs**

  String type
		
**Outputs:**  
 
  	
SD-25: SubjectDetails.getProfileType()
""""""""""""""""""""""""""""""""""""""    

This function

**Inputs**

  n/a
 
**Outputs**

  String profileType [*e.g.*, Myself, Child, Parent, etc)

NOTE:  Currently at around 32:33 into the 10-17 video recording.


**Source files:**
  
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/controller/SubjectController.java
	
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/models/TblUserAccount.java

  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/dao/TblUserAccountDao.java

  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/dao/impl/TblUserAccountDaoImpl.java

  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/util/AESCryptoManager.java
  
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/service/ProfileFilterService.java
  
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/service/impl/ProfileFilterServiceImpl.java
  
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/api/request/ProfileDetailsRequest.java
  
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/api/request/FilterRequest.java
  
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/service/UserAccountService.java
  
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/models/TblPeerAccount.java
  
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/dao/TblPeerAccountDao.java
  
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/dao/impl/TblPeerAccountDaoImpl.java

  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/service/impl/UserAccountServiceImpl.java

  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/service/TblShaSubjetService.java
  
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/service/impl/TblShaSubjetServiceImpl.java

  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/dao/TblShaSubjectDao.java
  
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/dao/impl/TblShaSubjectDaoImpl.java

  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/dao/PortalDetailsDao.java
  
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/dao/impl/PortalDetailsDaoImpl.java

  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/dao/PLPDSubjectDao.java
  
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/dao/impl/PLPDSubjectDaoImpl.java

  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/dao/TblAccountSubjectDao.java
  
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/dao/impl/TblAccountSubjectDaoImpl.java
  
  OpenID/trunk/private-access-server/private-access-adminportal/src/main/java/com/privateaccess/adminportal/api/response/SubjectDetails.java

**Database tables:**

  * dbPPMS_D.user_account
  * dbPPMS_D.tblShaSubject
  * dbPPMS_D.viewPortalDetails
  * dbPPMS_D.PLPDSubject
  * dbPPMS_D.tblShaAccountSubject


.. _Method XX:

Search Particpant Data
~~~~~~~~~~~~~~~~~~~~~~

This API saves the values from the search form so that it can be executed again by the cron script when it is time to save the results of this search query for export purposes.  

Here is an example JSON object that is stored in the database table as a result of the following method XX call:

{  
   "accountId":"14",
   "contact":"true",
   "ids":[  
      "44"
   ],
   "widgetId":"PEER-692",
   "cmd":"all",
   "fromDate":"Dec 10,2015",
   "env":"live",
   "toDate":"Oct 17,2016",
   "is_dash":"true",
   "portal_id":"692",
   "isExportAll":"true",
   "token":"eyJhbGciOiJSUzI1NiJ9.eyJhdWQiOlsiNGMxY2UwZGUtZWM2Ni00Nzg4LWFlZTQtYjhmYzQ0YTRmY2NlIl0sImlzcyI6Imh0dHBzOlwvXC9jb25uZWN0LnByaXZhdGVhY2Nlc3MuY29tXC8iLCJleHAiOjE0NzY3NDkxNTAsImlhdCI6MTQ3NjcyMDM1MCwianRpIjoiMzMxNzYwY2EtNDI5Yy00NGQzLWI3ZDgtN2JiYzVjNDlkNTljIn0.WxeQ3jU_eqO412J_IF_mL_6UBZm0gpuVIpnfqeNekpjDAIhLroCbxpbQcUHwhEJeU1UpdonMVAuQjUcWms1Nq5SZoR_owUZeu2yBEEwQtd5R0nCOGOnrkeUEd3nCymK8lfa2HqWvKrktcLJmcs0h_u5NcsXrFO76LefEfhpz8X0",
   "contactExport":"true",
   "surveyExport":"true",
   "strTimeZone":"America\/Los_Angeles",
   "mode":"prod",
   "selectedPortal":"PEER-692"
}

**Method XX:**

     **saveExportSurveyData.php**

**Inputs:**	 
  accountId
  contact
  ids[]
  widgetId
  cmd
  fromDate
  env
  toDate
  is_dash
  portal_id
  isExportAll
  token
  contactExport
  surveyExport
  strTimeZone
  
.. Note:: Need to modify API to accept JSON structure as input instead of formdata
  
**Outputs:**	 
  Example of element returned from method call
  
  {  
   "message":"Your requested file is in queue. Please check status in My Exports tab.",
   "isSuccess":true,
   "insert_id":1076
  }
  
**Source files:**
  
  admin/exportcontacts/saveExportSurveyData.php
  includes/functions.php
  admin/includes/admin_functions.php
  
**Database tables:**

  * peer_surveys_published.my_export
