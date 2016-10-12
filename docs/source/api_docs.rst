.. _API documentation:

=================
API documentation 
=================

This section describes the processes, functions, methods and source files used in those portions of the workflow diagrams that rely upon APIs in performing the defined flow.  

.. Attention::  As stated herein, all source file references are based on the current Subversion source code repository, but will need to be updated as the source files are moved into the GitHub repository as part of the OSS migration initiative. 
 

.. _Landing page:

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


.. _Sign-up and sign-in APIs:

Sign-up and sign-in functions
*****************************


Login
-----

**/login**

**Reference**

See PA-03 at step 007 of :ref:`Login selection`

**Purpose or Use:**

This API is invoked when the user enters their username and clicks the “Sign in” button.  The purpose of the API call is in order to pass the username that was entered by the user into the input box, along with any parameters (such as whether the Remember Me option is toggled on or off) to the PA Connect service.

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
    * **rememberMe** – string (optional) - indicates whether the user has invoked (or disabled) the option in this login
    * **authorizedURL** – string (optional) - indicates whether to bypass the enter username screen because the user came from a new account verification email link
    * **model** - ModelMap (required) - Spring framework that is used by the application to model data objects
    * **request** - HttpServletRequest (required) - the object passed to the processLogin method, including any query parameters
    * **response** - HttpServletResponse (required) - the object returned to the client browser
    * **session** - HttpSession (required) - stores the session information (username, user id) for later screens/methods to utilize

.. Note:: We should elaborate on the use of the Spring Framework ModelMap class.

.. tabularcolumns:: |l|l|l|
 user
 string
 required

user name or email address for the user wishing to login 

**Status codes:** n/a

**Method:** processLogin

*Input parameters*

+--------------+--------------------------+-----------+----------------------+
| Parameter    | Type                     | Required? | Use or Other Comment |
|              |                          |           |                      |
+==============+==========================+===========+======================+
| rememberMe   | String                   | Y         | Yes or Null          |
+--------------+--------------------------+-----------+----------------------+
| model        | ModelMap                 | Y         |                      |
+--------------+--------------------------+-----------+----------------------+
| request      | HttpServletRequest       | Y         |                      |
+--------------+--------------------------+-----------+----------------------+
| response     | HttpServletResponse      | Y         |                      |
+--------------+--------------------------+-----------+----------------------+
| session      | HttpSession              | Y         |                      |
+--------------+--------------------------+-----------+----------------------+
| userAccount  | UserAccount              | Y         |                      | 
+--------------+--------------------------+-----------+----------------------+
| userSiteKey  | UserSiteKey              | Y         |                      |
+--------------+--------------------------+-----------+----------------------+
| list         | List<UserLoginChallenge> | Y         |                      |
+--------------+--------------------------+-----------+----------------------+

*Response string*

+----------------+------------------------------------------------------------+
| Valid Response | Use or Other Comment                                       |
|                |                                                            |
+================+============================================================+
| URL            | If the userAccount object has not been verified, redirects |
|                | the browser to the Complete Verification screen            |
+----------------+------------------------------------------------------------+
| Login error    | If the account has not set challenge questions             |
+----------------+------------------------------------------------------------+
| Login error    | If the user name or account does not exist                 |
+----------------+------------------------------------------------------------+
| Null           | Calls the next API call (API:/question)                    |
+----------------+------------------------------------------------------------+

.. Hint:: We may wish to create two or more specific error messages that will inform the user of the reasons for the error rather than a generic error message that covers multiple issues.

**Example call**::

 Example request here

**Example result**::

 Example response here


Enter challenge question:  (**API:/question**)
----------------------------------------------

**Reference**

See :ref:`Existing user verification` (at 090)

**Use:**

This API is invoked when the user clicks on the sign-in button after entering responses to the challenge questions.  The API call passes the user’s answers back to the PA Connect server, which responds with an instruction to the application client to display an error message (092) or the screen (091) for submittal of the user’s password.

**Source files::**

 *OpenID/trunk/private-access-server/ private-access-openid-server/src/main/java/com/privateaccess/openid/connect/controller/LoginController.java*

 *OpenID/trunk/private-access-server/private-access-openid-server/src/main/java/com/privateaccess/openid/connect/model/UserLoginChallenge.java*
 

**Databases:**  dbPPMS_D, dbPPMS_D_Demo

**Tables:** user_login_challenge

