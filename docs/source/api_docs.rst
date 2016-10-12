.. _API documentation:

=================
API documentation 
=================

This section describes the processes, functions, methods and source files used in those portions of the workflow diagrams that rely upon APIs in performing the defined flow.  

.. Attention::  As stated herein, all source file references are based on the current Subversion source code repository, but will be moved updated as these source files are moved into the GitHub repository as part of the OSS migration initiative. 
 

.. _Landing page

Landing page
************

This page loads when the client first visits the URL of a specific portal.

URL: https://widget2.peerplatform.org/portal/[PORTAL_ID]
API: /portal/getportalinfo

Source Files: 

OpenID/trunk/private-access-server/private-access-webportal/src/main/java/com/privateaccess/webportal/controller/LoginController.java

The API call to getportalinfo retrieves portal information configured for the specified portal set in the session attribute called “portalInfo”.  This session attribute is taken from the URL which contains the Portal ID.  An example portal URL is as follows:

https://widget2-beta.peerplatform.org/portal/474

The API will return an APIResponse object (See OpenID/trunk/private-access-server/private-access-webportal/src/main/java/com/privateaccess/webportal/model/APIResponse.java) the data field of which contains the portal information in a PortalInfo object (See OpenID/trunk/private-access-server/private-access-webportal/src/main/java/com/privateaccess/webportal/model /PortalInfo.java)  for the specified Portal ID.

Here is an example response::

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


.. _Sign-up and sign-in APIs

Sign-up and sign-in functions
*****************************

Enter challenge question
------------------------

GET:: **API:/question**

**Reference**

See :ref:`PA 04 API` (Existing User Verification: Step 090)

**Use:**

This API is invoked when the user clicks on the sign-in button after entering responses to the challenge questions.  The API call passes the user’s answers back to the PA Connect server, which responds with an instruction to the application client to display an error message (092) or the screen (091) for submittal of the user’s password.

**Source files::**

 OpenID/trunk/private-access-server/ private-access-openid-server/src/main/java/com/privateaccess/openid/connect/controller/LoginController.java

 OpenID/trunk/private-access-server/private-access-openid-server/src/main/java/com/privateaccess/openid/connect/model/UserLoginChallenge.java


**Databases:**  dbPPMS_D, dbPPMS_D_Demo

**Tables:** user_login_challenge

