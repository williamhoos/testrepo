.. _API documentation:

=================
API documentation 
=================

This section describes the processes, functions, methods and source files used in those portions of the workflow diagrams that rely upon APIs in performing the defined flow.  

.. Attention::  As stated herein, all source file references are based on the current Subversion source code repository, but will need to be updated as the source files are moved into the GitHub repository as part of the OSS migration initiative. 
 

.. _Landing page:

Landing page
************

.. _PEER-01 API:

Get portal info (PEER-01)
-------------------------

**API:/portal/getportalinfo**

**Purpose or Use:**

 This API call retrieves portal information that is used by PEER to configure the content and theme of the desired portal based on the URL that is entered in the client's browser.  The information that is returned by the API call is for the portal that is set in the session attribute called **portalInfo**, which corresponds to the portal ID in the URL (*i.e.*, the number 474 in the example shown below):

     https://widget2-beta.peerplatform.org/portal/474

**Source files:**
  
 OpenID/trunk/private-access-server/private-access-webportal/src/main/java/com/privateaccess/webportal/controller/LoginController.java

**Data accessed from:** 

    * database.table (to come) 
    * database.table (to come)

**Response objects:**:

 The API will return the following APIResponse object:: 

 OpenID/trunk/private-access-server/private-access-webportal/src/main/java/com/privateaccess/webportal/model/APIResponse.java

 In turn, the data field within the APIResponse object contains the portal information for the specified Portal ID in a PortalInfo object, as follows::

 OpenID/trunk/private-access-server/private-access-webportal/src/main/java/com/privateaccess/webportal/model/PortalInfo.java

**Example result**::

  {  
    "status":"success",
    "message":"success",
    "isSuccess":true,
    "data":{  
       "widgetId":"474",
       "prepend":"",
       "referralCode":"",
       "trialButtonId":"106",
       "authorizeUrl":"openid_connect_login?portalWidgetId=474",
       "registrationSuccessUrl":null,
       "isDemo":false,
       "isFresh":null,
       "psid":null
    }
  }

.. Attention:: Display of the landing page is part of the participant engagement workflow.  The information described above needs to be supplmented with the database.table information that is called by this API.


.. _PEER-02 API:

Message content (PEER-02)
-------------------------

**API:/portal/view/template/message**

**Purpose or Use:**

  This API retrieves the various messaging content templates for the specified portal. The messages, which are used in modal pop-up windows, include the following templates:
  
    * successMessage
    * errorMessage
    * removeAccountMessage
    * deleteConfirmModal
    * deleteAccountModal
    * deleteAccountDependencyMsgModal
    * addOrganizationConfirmModal
    * enableMixpanelConfirmModal
    * clearSessionConfirmModal
    * updateAuthorizationModal
    * changePreferenceModal
    * timeoutModal
    * healthErrorModal

**Source files:**
  
 OpenID/trunk/private-access-server/private-access-webportal/src/main/java/com/privateaccess/webportal/controller/RedirectController.java
  
 OpenID/trunk/private-access-server/private-access-webportal/src/main/webapp/WEB-INF/views/template/message.jsp

**Data accessed from:** 

    * database.table (to come)
    * database.table (to come)

.. Attention:: Clarify where/how these messages are used by PEER, and where the data in them originates and how it can be changed/updated. 


.. _PEER-03 API:

Research opportunity (PEER-03)
------------------------------

**API:/portal/view/template/researchOpportunityDetailsModal**

**Purpose or Use:**

  This API call retrieves the various messaging content templates for the specified portal used in modal popup windows related to a research opportunity. These messages are used as part of the dynamic consent flow.  The message templates include the following templates:
  
    * ResearchOpportunityDetailsModal
    * ResearchOpportunityRequiredModal

**Source files:**
  
 OpenID/trunk/private-access-server/private-access-webportal/src/main/java/com/privateaccess/webportal/controller/RedirectController.java
  
 OpenID/trunk/private-access-server/private-access-webportal/src/main/webapp/WEB-INF/views/template/researchOpportunityDetailsModal.jsp

**Data accessed from:** 

    * database.table (to come)
    * database.table (to come)

.. Attention:: Clarify where/how these messages are used by PEER, and where the data in them originates and how it can be changed/updated. 


.. _PEER-04 API:

Signed-up template (PEER-04)
----------------------------

**API:/portal/view/template/signedup**

**Purpose or Use:**

  This API call retrieves the main content of the landing page for the specified portal.  This content includes the headline text, logo, start now button, and associated supplemental buttons, each as it is configured by the Administrative user specifically for the portal.  See also, :ref:`Administrator perspective`.

**Source files:**
  
 OpenID/trunk/private-access-server/private-access-webportal/src/main/java/com/privateaccess/webportal/controller/RedirectController.java
  
 OpenID/trunk/private-access-server/private-access-webportal/src/main/webapp/WEB-INF/views/template/signedup.jsp

**Data accessed from:** 

    * database.table (to come)
    * database.table (to come)

.. Attention:: Clarify where/how these messages are used by PEER, and where the data in them originates and how it can be changed/updated. 


.. _PEER-05 API:

Landing page features (PEER-05)
-------------------------------

**API:/services/feature/landingPageFeatures/{PORTAL_ID}**

**Purpose or Use:**

  This API call retrieves the content of the "features" to be displayed in the carosuel on the specified portal's landing page.

**Source files:**
  
 OpenID/trunk/private-access-server/private-access-openid-server/src/main/java/com/privateaccess/peer/controller/FeatureController.java

**Data accessed from:** 

    * dbPPMS_D.tblWidgetInfo
    * dbPPMS_D_Demo.tblWidgetInfo 

**Example result**::

  {  
    "status":"success",
    "message":"success",
    "isSuccess":true,
    "data":[  
       {  
          "videoURL":"//www.youtube.com/embed/n6p-v0Ih-fw",
          "isVideoIncluded":true,
          "imageURL":"1426091758348_howitworks_feature_image2.jpg",
          "isGuide":false,
          "name":"How it works video!",
          "id":"F_1",
          "isImageIncluded":true
       }
     ]
  }

.. Attention:: Clarify where/how these messages are used by PEER, and where the data in them originates and how it can be changed/updated. If the data is truly from dbPPMS, then we need to look at this as part of bifurcating the PEER and PA services.


.. _PEER-06 API:

Landing page features (PEER-06)
-------------------------------

**API:/services/widgetinfo/{PORTAL_ID}**

**Purpose or Use:**

  This API call retrieves the theme and content information for the specified portal.

**Source files:**
  
 OpenID/trunk/private-access-server/private-access-openid-server/src/main/java/com/privateaccess/peer/controller/WidgetInfoController.java
 
 OpenID/trunk/private-access-server/private-access-openid-server/src/main/java/com/privateaccess/peer/models/ TblWidgetInfo.java

**Data accessed from:** 

    * dbPPMS_D.tblWidgetInfo
    * dbPPMS_D_Demo.tblWidgetInfo 

**Example result**::

  {  
    "status":"success",
    "message":"success",
    "isSuccess":true,
    "data":{  
        "idtheme":474,
        "stretchToBrowser":true,
        "border":1,
        "shadow":0,
	"cornerRadius":15,
        "theme1color":"FF4DE1",
        "theme2color":"137DBA",
        "theme3color":"FFA229",
        "linkColor":"35FF1F",
        "linkRollover":"FF5719",
        "linkClicked":"C24213",
        "background":"FFFFFF",
        "borderColor":"C9C9C9",
        "buttonColor":"FFCB1F",
        "buttonGradient":"FF722B",
        "fontColor":"FFFFFF",
        "guide1":"2",
        "guide2":"23",
        "guide3":"24",
        "fkFeaturedContentType":1,
        "featuredContentValue":"host_SharonTerry.png",
        "isLogoIncluded":false,
        "preHeadLine":"YOUR HEADER HERE",
        "postHeadLine":"can help!",
        "preHeadLineColor":"000000",
        "postHeadLineColor":"000000",
        "headLineLogo":"tf_logo.png",
        "introText":"<b>SHARE</b>... Answer as many questions as you would like, and control how and with whom that information is shared. <b>CONNECT</b>... Find out how you compare to others, and let support and helpful resources come to you. <b>DISCOVER</b>... If you wish, let researchers access your information to help spark innovation for all.",
        "stepsMessage":"It's Easy as 1, 2, 3",
        "step1func":"Register",
        "step1copy":"<p class=\"title1\" >Register</p><font size=\"2\"><p class=\"title2\"> (or sign in) </p></font>",
        "step2func":"TakeExampleSurvey",
        "step2copy":"<p class=\"title1\" >Enter Health Information</p><font size=\"2\"><p class=\"title2\">Click to sample some questions</p></font>",
        "step3func":"TakeExampleSurvey",
        "step3copy":"Let Researchers Find YOU!",
        "step4func":"none",
        "step4copy":"None",
        "footerTitle":"Respecting Your Wishes is Our Priority", 
        "footerContent":"We protect your privacy according to your preferences. To do this, we use technology from our partner Private Access. Then you can share your health information with whomever you choose, on your own terms.",
        "askQuestion":"",
        "signingInTags":"",
        "sigedInTags":"",
        "dateCreated":1422572938000,
        "dateUpdated":1422572938000,
        "fkIdlandingpage":205,
        "isPreview":false,
        "isConditionQuestion":true,
        "isTagsQuestion":true,
        "hostList":"F_1",
        "txtbtnStartNow":"Start Now!",
        "btnFunc1":"ContinueSurvey",
        "txtSignedInText1":"Continue Health Survey",
        "btnFunc2":"AddParticipant",
        "txtSignedInText2":"Add Family Member",
        "btnFunc3":"none",
        "txtSignedInText3":"None",
        "btnFunc4":"none",
        "txtSignedInText4":"None",
        "livingTags":null,
        "deceasedTags":null,
        "prenatalFetusTags":null,
        "prenatalDeceasedFetusTags":null,
        "spinnerColor":"FF0000",
        "mixPanelCode":"2db24x1e8115e6ed2adf323b4e7ez22e",
        "medicalHistory":false,
        "familyHistory":false,
        "labResults":false,
        "molecularProfiling":false,
        "medicalRecords":false,
        "isBRCAReport":false,
        "treatments":false,
        "txtMedicalHistory":null,
        "txtFamilyHistory":null,
        "txtLabResults":null,
        "txtMolecularProfiling":null,
        "txtMedicalRecords":null,
        "txtTreatments":null,
        "googleAnalyticCode":"UA-123456789-6",
        "isHealineTextIncluded":true,
        "isAddStartNowLink":true,
        "isDemo":false,
        "useJTIPS":true,
        "useLandingPage":true,
        "medicalHistoryName":"Medical History",
        "familyHistoryName":"Family History",
        "labResultsName":"Medical History",
        "molecularProfilingName":"Molecular Profiling",
        "treatmentsName":"General Health",
        "medicalRecordsName":"Medical Records",
        "uploadBRCAReportName":"Upload BRCA Report",
        "livePortalId":474,
        "demoPortalId":475
    }
 }
 
.. Attention:: Clarify where/how these messages are used by PEER, and where the data in them originates and how it can be changed/updated. If the data is truly from dbPPMS, then we need to look at this as part of bifurcating the PEER and PA services.  Also, we should clarify in the *Purpose or Use* discussion how this API differs from :ref:`PEER-04` 


.. _Sign-up and sign-in APIs:

Sign-up and sign-in functions
*****************************

.. _PA-01 API:

Set portal information (PA-01)
------------------------------

**API:/portal/setportalinfo**

**References**

    * Invoked at step 001 of :ref:`Register or login` (for new users)
    * Invoked at step 001 of :ref:`Login selection` (for returning users)

**Purpose or Use:**

 This API call sets the portal information into a session object for use by the PA Connect service during registration of a new user or sign in of an existing user.  The API informs PA Connect the portal that the Account Holder has logged into, which enables the service to know where to return the user after they have been successfully authenticated.
    
**Source files:**

 OpenID/trunk/private-access-server/private-access-webportal/src/main/java/com/privateaccess/peer/controller/LoginController.java

**Example of JSON input**::

 {  
    "widgetId":"474",
    "prepend":"",
    "referralCode":"",
    "trialButtonId":106,
    "authorizeUrl":"openid_connect_login?portalWidgetId=474",
    "isDemo":false,
    "registrationSuccessUrl":null
 }


.. _PA-02 API:

Get portal name (PA-02)
-----------------------

**API: /services/widgetinfo/getPortalName/[PORTAL_ID]**

**Reference**
    
    * Invoked at step 001 of :ref:`Register or login` (for new users)
    * Invoked at step 001 of :ref:`Login selection` (for returning users)
    
**Purpose or Use:**

 This API call retrieves the name of the portal for which the participant will be signing in or registering.  This enables the name of that registry to be displayed on the login screen generated by the PA Connect service. 

**Source files:**

 OpenID/trunk/private-access-server/private-access-webportal/src/main/java/com/privateaccess/peer/controller/WidgetInfoController.java 
 
 OpenID/trunk/private-access-server/private-access-openid-server/src/main/java/com/privateaccess/peer/models/ TblWidgetInfo.java

**Data accessed from:** 

    * dbPPMS_D.tblWidgetInfo 
    * dbPPMS_D_Demo.tblWidgetInfo

**Example of JSON response**::

 {  
    "status":"success",
    "message":"success",
    "isSuccess":true,
    "data":{  
       "portalFullName":"Portal Full Name",
       "portalNickName":"Portal Nickname"
    }
 }


.. _PA-03 API:

Login (PA-03)
-------------

**API:/login**

**References**

    * Invoked at step 007 of :ref:`Login selection`
    * Invoked at step 074 of :ref:`Activate account`

**Purpose or Use:**

 This API is invoked when a user enters their username or an email address into the returning user field and clicks on the “Sign in” button during the login process or clicks on the link in the verification email that is sent to a new user (and that when clicked signals the application to skip the sign-in and challenge questions screens, and proceed directly to the password entry screen).  The API passes to the PA Connect service the name or email address that was entered by the user (or conveyed by employing the single-use token in the verification email), along with any parameters (such as whether the Remember Me option was toggled on or off by the user before he or she clicked on the "Sign in" button).

**Source files:**
  
 OpenID/trunk/private-access-server/ private-access-openid-server/src/main/java/com/privateaccess/openid/connect/controller  /LoginController.java
 
 OpenID/trunk/private-access-server/private-access-openid-server/src/main/java/com/privateaccess/openid/connect/model/UserAccount.java

**Data accessed from:** 

    * dbPPMS_D.user_account 
    * dbPPMS_D_Demo.user_account

**Request Headers:**

 Authorization – oAuth token

**Query parameters:**

 None

**Form parameters:**

    * **user** – string (required) - user name or email address for the user wishing to login
    * **rememberMe** – string (optional) - indicates whether the user has invoked (or disabled) the Remember Me option in connection with this login (and that will in turn affect his or her future login experience)
    * **authorizedURL** – string (optional) - indicates whether to bypass the enter username screen because the user came from a new account verification email link
    * **model** - ModelMap (required) - Spring framework that is used by the application to model data objects
    * **request** - HttpServletRequest (required) - the object passed to the processLogin method, including any query parameters
    * **response** - HttpServletResponse (required) - the object returned to the client browser
    * **session** - HttpSession (required) - stores the session information (username, user id) for later screens/methods to utilize

.. Note:: We should elaborate on the use of the Spring Framework ModelMap class.

**Status codes:** n/a

**Method:** processLogin

*Input parameters*

    * **rememberMe** – string (required) - permits a value of Yes or Null 
    * **model** - ModelMap (required) 
    * **request** - HttpServletRequest (required)
    * **response** - HttpServletResponse (required)
    * **session** - HttpSession (required)
    * **userAccount** - UserAccount (required)
    * **userSiteKey** - UserSiteKey (required)
    * **list** - List<UserLoginChallenge> (required)

*Valid Responses*

    * **URL** (string) - If the userAccount object has not been verified, this response redirects the browser to the "complete verification" instruction screen that informs the user to verify his or her registration by clicking on the link in the system-generated email message, and enables them to send a new message if the earlier one was lost or not received
    * **Login error** (string) - If the account has not set challenge questions 
    * **Login error** (string) - If the user name or account does not exist
    * **Null** (string) - Calls the next API call (API:/question)
    
.. Hint:: We may wish to create two or more specific error messages that will inform the user of the reasons for the error rather than a generic error message that covers multiple issues.

**Example call**::

 Example request here

**Example result**::

 Example response here


.. _PA-04 API:

Enter challenge question (PA-04)
--------------------------------

**API:/question**

**Reference**

    * Invoked at step 090 of :ref:`Existing user verification`

**Purpose or Use:**

 This API is invoked when the user clicks on the sign-in button after entering responses to the challenge questions that are generated by the application to deter phishing-type attacks.  The API call passes the user’s answers back to the PA Connect server, which responds with an instruction to the application client to either display an appropriate error message (092) or to display the screen (091) for submittal of the user’s password.

**Source files::**

 OpenID/trunk/private-access-server/ private-access-openid-server/src/main/java/com/privateaccess/openid/connect/controller/LoginController.java

 OpenID/trunk/private-access-server/private-access-openid-server/src/main/java/com/privateaccess/openid/connect/model/UserLoginChallenge.java
 
**Data accessed from:**  

    * dbPPMS_D.user_login_challenge
    * dbPPMS_D_Demo.user_login_challenge


.. _PA-05 API:

Enter Password (PA-05)
----------------------

**API:/password**

**References**

    * Invoked at step 076 of :ref:`Activate account` (first time user)
    * Invoked at step 104 of :ref:`Password entry` (returning user)

**Purpose or Use:**

 After the user enters his or her password and clicks on the “Sign in” button, this API call is made by PEER to pass the user’s password entry to the PA Connect server, which responds with an instruction to the client to either display the appropriate error message or to open the welcome screen (080) if this is the first time the user has visited the registry, or takes them to the main user dashboard (085) and the profile the user was last using in the case of a returning user.

**Source files::**

 OpenID/trunk/private-access-server/ private-access-openid-server/src/main/java/com/privateaccess/openid/connect/controller/LoginController.java
 
 OpenID/trunk/private-access-server/private-access-openid-server/src/main/java/com/privateaccess/openid/connect/model/UserAccount.java

**Data accessed from:**  

    * dbPPMS_D.user_account
    * dbPPMS_D_Demo.user_account






.. BELOW IS AN API TEMPLATE FOR FUTURE USE - COPY / DO NOT REMOVE**
.. _API template:

Additional API documentation
****************************

.. _TBD API:

API function (TBD)
------------------

**API:/**

**References**

    * Invoked at step XXX of :ref:``
    * Invoked at step XXX of :ref:``

**Purpose or Use:**

  This API is invoked when / called by....

**Source files:**
  
  Enter all that are applicable

**Data accessed from:** 

    * database.table 
    * database.table

**Request Headers:**

  Authorization – oAuth token

**Query parameters:**

  If applicable

**Form parameters:**

    * ** ** – string (required) - description of purpose
    * ** ** – string (optional) - other comments 

**Status codes:** n/a

**Method:** nameHere

*Input parameters*

    * ** ** – string (required) - description of purpose
    * ** ** – string (optional) - other comments 

*Valid responses*

    * ** ** – response - description of use
    * ** ** – response - other comments 

**Example call**::

 Example request here

**Example result**::

 Example response here

.. Attention:: Necessary before OSS begins.

.. Note:: Nice to have before OSS community joins.

.. Hint:: Future suggestions, if any.
