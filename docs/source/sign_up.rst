.. _Initial Sign-up:

==========================
Sign-up (First-time users) 
==========================

As indicated in the left arm of the :ref:`Sign-up or sign-in workflow` description, the registration process for first-time PEER users is comprised of the following 8 steps:

.. _Register or login:

Register or login selection
***************************

At the conclusion of the initial engagement flow, first-time users arrive at sign-up or sign-in screen.  Here, new users begin the registration process by clicking into the "First Time User" entry field. 

.. image:: https://s3.amazonaws.com/peer-downloads/images/TechDocs/Register+or+Login+Selection.png
    :width: 94.9%
    :alt: Register Workflow
| 

.. Attention:: Adjust the UI so that the two entry boxes are equal in height

.. Note:: Try to ensure that analytics take into account all engagement means that have brought prospects to this page

.. Hint:: Consider creating a simplified UI in which only one box appears on the page (for sign-up) and let’s someone who is a returning user click to sign-in link (unless the system recognize their hardware, in which case the page would display only a sign-in field with a link for sign-up so that it remains an equally simple screen view)  


.. _Enter new email:

Enter new user email
********************

The first-time user must enter a valid email address and affirm that they meet the minimum age requirements in order to proceed with registration process.

.. image:: https://s3.amazonaws.com/peer-downloads/images/TechDocs/Enter+New+User+Email.png  
    :width: 93.5%
    :alt: Enter New User Email Workflow
| 

.. Attention:: Verify that the flow works for all Proxy and Pre-Registered use cases

.. Note:: Consider adding error-checking algorithm to current the minimum age affirmation

.. Hint:: Consider incorporating the option for using Text Messaging in lieu of email

.. Hint:: Consider replacing the current email-based notifications system with System Tray Notifier-based notifications

.. Hint:: Consider providing a cleaner UI for error messages (see Pluralsight website as an example) 


.. _Create Username

Create username and password
****************************

After clicking on the "Sign Up" button, the system opens the "Create Username and Password" screen, which prompts the user to enter a username (which may be his or her email address or a different name); and to enter and confirm a password meeting the system's minimm criteria.

.. image:: https://s3.amazonaws.com/peer-downloads/images/TechDocs/Create+Username.png
    :width: 94.1%
    :alt: Create Username Workflow
|

.. Attention:: Verify in testing proper display and content of error messages for password entry

.. Note:: Try to add an auto-populate function to pre-populate the Username field with the user's email address entry (ie, as a default username selection)

.. Hint:: Consider replacing the current email-based notifications system with System Tray Notifier-based notifications

.. _Set Security questions

Set security questions
**********************

Once these are accepted, the "Create Security Questions" screen opens, and the user is prompted to select and provide answers to three Challenge Questions.

.. image:: https://s3.amazonaws.com/peer-downloads/images/TechDocs/Set+Security+Questions.png
    :width: 91.2%
    :alt: Set Security Questions Workflow
|

.. Hint:: Consider revising the Challenge Questions feature to display *only* the available items (ie, by removing from the pull-down list any questions that are already being use)

.. Hint:: Consider allowing the user to enter their own (free-text) questions (ie, in addition to the pre-generated questions)

.. Hint:: Consider replacing (or supplementing) the use of Challenge Questions with multi-factor authentication process using an SMS message sent to the users mobile phone, Google Authenticator or other

.. _Create site key

Create site key
***************

Upon completing the three Challenge Answers, the system opens the "Create Site Key" screen.

.. image:: https://s3.amazonaws.com/peer-downloads/images/TechDocs/Create+Site+Key.png
    :width: 77.9%
    :alt: Create Site Key Workflow
|

.. Hint:: Consider discontinuing the use of site keys

.. _EULA approval

EULA approval
*************

.. image:: https://s3.amazonaws.com/peer-downloads/images/TechDocs/EULA+approval.png
    :width: 72.2%
    :alt: EULA approval Workflow
|

.. Attention:: Add the "Remote Entry Supplement" language to the Privacy Policy and/or Terms of Service disclosures

.. Attention:: Update all of the user agreements to refelct open source software

.. Note:: Try to simplify / shorten the language in all of the user agreements

.. Hint:: Consider replacing the "No Pending Request" notice with an opportunity to revise/correct the email address to be used for registration

.. Hint:: Consider developing an automated follow-up process that is triggered when a prospective new user does not timely confirm acceptance of the EULA

.. _Send confirmation email

Send confirmation email
***********************

.. image:: https://s3.amazonaws.com/peer-downloads/images/TechDocs/Send+confirmation+email.png
    :width: 84.3%
    :alt: Send Confirmation Email Workflow
|

.. Hint:: Consider refactoring the Confirmation Email process to postpone it until after some activity has taken place by User

.. Hint:: Consider replacing (or supplementing) the "No Pending Request" notice with a revise/corrected email flow to commence a new registration process based on entry of a different email address than initially submitted

.. Hint:: Consider 

.. _Activate account

Activate account
****************

.. image:: https://s3.amazonaws.com/peer-downloads/images/TechDocs/Activate+Account.png
    :width: 96.8%
    :alt: Account Activate Workflow
|

.. Note:: Try to add a Remmber Me Toggle link to this page to provide new users with the option to skip the Site Key and Password entry requirement when the hardware is recognized 

.. Hint:: Consider adding an automated process to send appropriated follow-up reminder messages when an excessive amount of elapsed time has transpired without the user activating his or her account
