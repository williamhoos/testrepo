.. _Authorization & Proxy:

==============================
Authorization & Proxy Settings 
==============================

As indicated in the summary of the :ref:`Authorization` workflow, depending on whether the organization through whom a participant registered is willing to serve as a proxy, an individual user has several ways to designate the person(s) and/or group(s) to whom he or she authorizes to act on their behalf.  

As indicated, the PEER sponsor's General Settings (summarized in the shaded box) include a willingness to serve as a proxy *and* the sponsor's defualt authorization settings.  These selections are reflected in the "We're here for you" screen that is displayed to the user; and to the extent the portal sponsor has indicated a willingness or desire to serve as a proxy for participants, the account holder is provided an opportunity to accept these default setttings, or to customize them before getting started:

Accept default settings
***********************

.. image::  https://s3.amazonaws.com/peer-downloads/images/TechDocs/Accept+default+settings.png
    :alt: Accept default settings Illustration
|
Accepting the default selections confirms these proxy settings and skips directly to the :ref:`Contact Info` screen.  Alternatively, where an account holder wishes to modify or decline to accept the organization's default settings, then by clicking on the Customize button the Account Holder can customize the extent of his or her delegation of authority to the portal sponsor.

.. Note::  As indicated by Item "KE" in the the indicated configuration provided by each sponsor organization, it would be desireable to enable each portal sponsor, should they wish to do so, to provide its own welcome message that would appear in lieu of the "We're here for you" headline and message content.

.. Note::  While it is possible to decline the proxy offer, currently this is achieved by "customizing" the default settings to effectively render such designation as being irrelevant.  In the future, we may want to consider adding a more direct "decline proxy" selection.

Such customization is illustrated by the following workflow drawing:

.. _Customize settings:

Customize these settings
************************

As shown, where a default preference is provided, the account holder is provided an opportunity to modify any or all of the sponsor's default settings, and thereby to designate more or less broad authority to act on the participant's behalf, and a message to verify that such changes were intentional:

.. image::  https://s3.amazonaws.com/peer-downloads/images/TechDocs/Customize+settings.png
    :alt: Customize settings Illustration
|
.. Note:: It could help to reduce the number of steps involved in registering as a new user if the organization's default privacy preference settings could also be approved or customized as part of hte foregoing (*i.e.*, rather than waiting for a future step).

.. _Contact info:

Enter Contact Information
*************************

Whether a sponsor has provided participants with an opportunity to elect the sponsor as a proxy or not, the next step involves submitting contact information for the account holder.  This is illustrated in the following worflow:

.. image::  https://s3.amazonaws.com/peer-downloads/images/TechDocs/Enter+contact+information.png
    :alt: Enter contact information Illustration
|
While a number of other optional data fields are provided, as shown above, only the First Name, Last Name and Postal Code are treated as required fields.  Along with the individual's email address that was validated as part of the Sign-up flow, these 4 items of Personally Identifying Information (or PII) and any other optional PII elements are maintained by Private Access and not accessible to the organizational sponsor or any other party unless expressly permitted by the individual's privacy and data sharing preferences. 

.. Attention:: The Data Table needs to be bifurcated between the data elements that are approrpriately held by Private Access (such as the foregoing PII) and the data that that should be maintained by PEER. After being bifurcated, the Data Table on the PEER side should expressly designate the PII and non-PII data elements, and optionally should only receive the non-PII data.  For example, whereas the full zip code is treated as PII, the first three digits of the zip code may be included as non-PII.  

Add New Profile (1 of 3)
************************

Below is the first of three diagrams illustrating the process of creating a new profile within PEER.  Such new profiles may be created for the account holder himself or herself (*i.e.*, a "myself account"), or on behalf of a child, spouse, parent, or "someone else".  Such designations are provided for several reasons.  These reasons include the ability for each question about the profile to be stated in the appropriate person (*i.e.*, "I am..." or "Are you...?" for the myself account).  In addition, the relationship directly influences the right of an individual to create an account for another person, and the process for either self-attesting to having the appropriate authority or sending a link to the affected person to confirm their authorization for the account holder to act on their behalf.

.. image::  https://s3.amazonaws.com/peer-downloads/images/TechDocs/Add+new+profile+1.png
    :alt: Add new profile 1 Illustration
|
In the event of a myself account, the system pre-populates the profile's first and last name, and postal code from the contact information entries.  The portion of this diagram that is shaded in grey indicates a portion of the overall workflow that is the same for each type of profile, although these fields start out as blank except in the case of the "myself account profile".  In each case, another element of PII - namely the individual's date of birth - is requested as a required entry.

.. Attention:: The date of birth is another element that must be addressed in bifurcating the Data Table between the elements that are approrpriately held by Private Access and the data that that should be maintained by PEER.  The birth date may require additional processing along the lines of the foregoing discussion regarding postal code, wherein Private Access may retain full PII for the zip code whereas PEER may hold non-PII employing an abridged postal code based on HIPAA de-identification regulations.  In the case of birth date, Private Access may hold the actual date, which will be treated as PII, whereas PEER will hold the age in years or state "Over 92" for anyone over that age, which would not be treated as PII.  In this case, however, the API would be needed to address auto-calculations based on changes requiring the actual date of birth to calculate age changes.

.. Note:: Currently, there is no error checking to validate that the account holder is at least 18 years old, and/or to provide that minor children set up their own accounts upon reaching the age of majority.  Both of these protections would be desireable to add in the future.

Add New Profile (2 of 3)
************************

The second of the three diagrams addressing the process of creating a new profile within PEER indicates the additional data that is required for creating a child, spouse or parent profile.  As shown, in each of these cases, the account holder is prompted to indicate wheteher the profile holder is commonly referred to as "he" or "she", and whether the individual is for a person who is living, deceased or not yet born (*i.e.*, pre-natal).  For profiles on behalf of deceased persons, the system also requests a date of death. These selections enable the system to employ the correct personal pronoun and tense for each inquiry respecting the individual (*i.e.*, "He is..." or "Before passing away, was she...?"). 

.. image::  https://s3.amazonaws.com/peer-downloads/images/TechDocs/Add+new+profile+2.png
    :alt: Add new profile 2 Illustration
|
.. Note:: Currently, the system is employing the gender at birth question response as a "global variable".  This logic should be revised to infer such response from the profile setting unless over-ridden by the response to the gender at birth question reveals an edge case, and that logic should be taken into account in curating surveys.

Once all of the required data fields have been completed by the user, the system opens the main dashboard page.  

Add New Profile (3 of 3)
************************

The third diagram illustrates the additional steps that are required to create an "another person" profile when such other person is **not** a direct blood relative (*i.e.*, a child or parent), or the account holder's spouse.  In all other cases, counsel has opined that self-attestation is insufficient - despite the express threat that civil penalties and/or criminal prosecution my apply in the event of mis-representation.    

.. image::  https://s3.amazonaws.com/peer-downloads/images/TechDocs/Add+new+profile+3.png
    :alt: Add new profile 3 Illustration
|
.. Note:: After the system informs the account holder that his or her request to create a new account for this person has been sent, it would be preferable to return the account holder to the Kendo table for creating another user.  

Confirm Someone Else Account
****************************

.. image::  https://s3.amazonaws.com/peer-downloads/images/TechDocs/Confirmation+email+for+someone+else.png
    :alt: Confirmation email for someone else account Illustration
|
.. Hint:: Ideally, logic and appropriate message content would be added to enable the system to follow-up automatically in the event the other person does not timely respond to the initial invitiation.    

Select Date Function
********************

The following date selection utility function is provided to enable the account holder to select a date of birth, the date of death, the expected date of delivery in the profile creation workflow.  This function is also used in selecting a date in response to survey questions that require entry of a date when something took place (*e.g.*, the date of diagnosis, surgery, commencing treatment, etc.)

.. image::  https://s3.amazonaws.com/peer-downloads/images/TechDocs/Select+date+function.png
    :alt: Select date Illustration 
|

New Profile Menu
****************

The following workflow refers to the "hamburger menu" (*i.e.*, the menu that appears as three horizontal lines) in the upper right hand corner of the PEER iFrame.  Once an initial profile has been created, the menu will integrate the First Name of that profile and the optional picture.  However, until the first profile has been created, the system will display the phrase "New profile" in the name field in conjunction with a generic silhouette image.  The following block flow diagram describes the functions avalaible from this rolling over, and/or clicking on the hamburger menu icon:

.. image::  https://s3.amazonaws.com/peer-downloads/images/TechDocs/New+profile+menu.png
    :alt: New profile menu Illustration 
|
As shown, from the menu, the user is provided the option to select any of five utilities, including to (1) review and/or edit Account details; (2) review and revise the challenge question/answer pairs or site key and phrase seleced initially; (3) review and/or edit additional settings affecting the account; (4) displaying the audit log; (5) create a new profile, and (6) logout of the system. 

Update Password Function
************************

The following diagram illustrates the workflow for creating or unpdating a password.  

.. image::  https://s3.amazonaws.com/peer-downloads/images/TechDocs/Update+password.png
    :alt: Update password Illustration 
|
