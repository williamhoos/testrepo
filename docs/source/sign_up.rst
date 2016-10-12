.. _Initial Sign-up:

==========================
Sign-up (First-time users) 
==========================

The sign-up workflow (for first-time users) is comprised of steps 01 - 08, and where needed, the four shared utility resources:  

.. _Sign-up drawing:

.. image:: https://s3.amazonaws.com/peer-downloads/images/TechDocs/New+Sign-up+or+Sign-in+Workflow.png
    :alt: Sign-up Workflow Illustration
    
Each of these steps or utility functions is described below:

.. _Register or login:

01: Register or login selection
*******************************

At the conclusion of the initial engagement flow, first-time users arrive at a page from which they can either sign-up (if a new user) or sign-in (if a returning user).  Here, new users begin the registration process by clicking into the "First Time User" entry field, and following the workflow sequence shown below: 

.. image:: https://s3.amazonaws.com/peer-downloads/images/TechDocs/Sign-up.png
      :alt: Register Workflow

As indicated by the two-headed arrows in the above illustration, the workflow includes several APIs to third-party services.  

The content of the sign-up screen (001) employs two API calls that are made by PEER to PA Connect, an OpenID Connect service that is used for single-sign on and identity provisioning in conjunction with Private Access' PrivacyLayer service.  PA Connect employs the OAuth 2.0 open standard. For more information about OpenID Connect, see `connect2id explained <http://connect2id.com/learn/openid-connect>`_.  

These APIs (and others that are employed by PEER) are described in the API documentation section, which includes a reference to the identifier shown in the corresponding illustration or text description of the system.

.. Attention:: Adjust the UI so that the two entry boxes are equal in height

.. Note:: Try to ensure that analytics take into account all engagement means that have brought prospects to this page

.. Hint:: Consider creating a simplified UI in which only one box appears on the page (for sign-up) and let’s someone who is a returning user click o a sign-in link (unless the system recognizes their hardware, in which case the page would display only a sign-in field with a link for sign-up so that it remains an equally simple screen view for all users)  


.. _Enter new email:

02: Enter new user email
************************

Each first-time user must enter a valid email address and affirm that they meet the minimum age requirements in order to proceed with registration process. 

.. image:: https://s3.amazonaws.com/peer-downloads/images/TechDocs/Enter+New+User+Email.png
    :alt: Enter New User Email Workflow
| 

.. Attention:: Verify that the flow works for all Proxy and Pre-Registered user cases

.. Note:: Consider adding an error-checking algorithm to verify the minimum age affirmation when the user creates a profile

.. Hint:: Consider incorporating the option for using Text Messaging in lieu of email

.. Hint:: Consider replacing the current email-based notifications system with System Tray Notifier-based notifications

.. Hint:: Consider providing a cleaner UI for error messages (see Pluralsight website as an example) 


.. _Create Username:

03: Create username and password
********************************

After clicking on the "Sign Up" button, the system opens the "Create Username and Password" screen, which prompts the user to enter a username (which may be his or her email address, or a different name); and to enter and confirm a password meeting the system's minimm criteria.

.. image:: https://s3.amazonaws.com/peer-downloads/images/TechDocs/Create+Username.png
    :alt: Create Username Workflow
|

.. Attention:: Verify in testing proper display and content of error messages for password entry, and articulate any APIs that are used to avoid duplicate registrations.

.. Note:: Time permitting before releasing as OSS, consider adding an auto-populate function to pre-populate the Username field with the user's email address entry (ie, as a default username selection)

.. Hint:: Consider replacing the current email-based notifications system with System Tray Notifier-based notifications

.. _Set Security questions:

04: Set security questions
**************************

Once the new user has selected a Username and Password, the *Create Security Questions* screen opens, and the user is prompted to select and provide answers to three challenge questions.

.. image:: https://s3.amazonaws.com/peer-downloads/images/TechDocs/Set+Security+Questions.png
    :alt: Set Security Questions Workflow
|

.. Hint:: Consider revising the Challenge Questions feature to display *only* the remaining available items (ie, by removing from the pull-down list any questions that are already being used)

.. Hint:: Consider allowing the user to enter his or her own (free-text) questions (ie, in addition to the pre-generated questions)

.. Hint:: Consider replacing (or supplementing) the use of Challenge Questions with multi-factor authentication process using a text message sent to the users mobile phone, Google Authenticator or other similar service

.. _Create site key:

05: Create site key
*******************

Upon completing the three challenge answers, the system opens the "*Create Site Key*" screen, which assists the user to select a site key and enter a phrase or caption.  Site keys help protect against phishing attacks, by presenting the credentials to the user before requesting entry of his or her password.

.. image:: https://s3.amazonaws.com/peer-downloads/images/TechDocs/Create+Site+Key.png
    :alt: Create Site Key Workflow
|

.. Hint:: Consider discontinuing the use of site keys

.. _EULA approval:

06: EULA approval
*****************

.. image:: https://s3.amazonaws.com/peer-downloads/images/TechDocs/EULA+Approval.png
    :alt: EULA approval Workflow
|

.. Attention:: Add the "Remote Entry Supplement" language to the Privacy Policy and/or Terms of Service disclosures

.. Attention:: Update all of the user agreements to refelct open source software

.. Note:: Try to simplify / shorten the language in all of the user agreements

.. Hint:: Consider replacing the "No Pending Request" notice with an opportunity to revise/correct the email address to be used for registration

.. Hint:: Consider developing an automated follow-up process that is triggered when a prospective new user does not timely confirm acceptance of the EULA

.. _Send confirmation email:

07: Send confirmation email
***************************

.. image:: https://s3.amazonaws.com/peer-downloads/images/TechDocs/Send+Confirmation+Email.png
    :alt: Send Confirmation Email Workflow
|

.. Note:: Verify how well the system is able to accommodate a new registrant having initially entered an incorrect email address, and then correcting it when resending the message (for example, verify that this use case is correctly treated in the participant PII and notification databases?) 

.. Note:: Try to assess ways to reduce the number of non-received / un-responded confirmation emails through refactoring the foregoing process (such as through use of a text message) or adding logic for sending automated reminder messages. 

.. Hint:: Consider refactoring the Confirmation Email process to postpone it until after some activity has taken place by User

.. Hint:: Consider replacing (or supplementing) the "No Pending Request" notice with a revise/corrected email flow to commence a new registration process based on entry of a different email address than initially submitted


.. _Activate account:

08: Activate account
********************

.. image:: https://s3.amazonaws.com/peer-downloads/images/TechDocs/Activate+Account.png
    :alt: Account Activate Workflow
|

.. Note:: Try to add a Remmber Me Toggle link to this page to provide new users with the option to skip the Site Key and Password entry requirement when the hardware is recognized 

.. Hint:: Consider adding an automated process to send appropriated follow-up reminder messages when an excessive amount of elapsed time has transpired without the user activating his or her account


Related functions
*****************

Returning users employ an abridged process, which can be simplified even further by permitting the system to remember their username and their hardware.  This flow is described in the :ref:`sign-in` description; and the utility functions that are leveraged by both sign-up and sign-in are described in the :ref:`shared utilities` section that follows.


