.. _Sign-in Utlities:

==================
Shared utililities
==================

As indicated by the :ref:`Sign-up or sign-in` workflow description, the sign-up and sign-in processes share 3 utility functions to toggle on/off a remember user function and recover lost credentials.  

Workflows for these functions are described below:

.. _Remember me toggle

Remember me toggle function
***************************

Accessible from various pages during the sign-up or sign-in workflow, users are given an option to toggle on/off a feature that will remember one or more of their authentication credentials based on recognizing their hardware.

.. image:: https://s3.amazonaws.com/peer-downloads/images/TechDocs/Remember+Me.png
    :alt: Remember Me Toggle Workflow
|
.. Attention:: Verify the "Remember Me" option works (and confirm it **only** works) when a returning user seeks access from a previously used hardware device.

.. Note:: Try to add an option for Users to select the "Remember Me" option directly from the :ref:`Activate Account` page.

.. Hint:: Consider adding active monitoring intellegence to identify suspicious user activity.


.. _Recover lost credentials

Recover lost credentials
************************

From links on various pages, users are able to request reninders of lost access credentials (username, site key challenge questions responses and password). 

.. image:: https://s3.amazonaws.com/peer-downloads/images/TechDocs/Lost+credentials+recovery.png
    :alt: Recover Lost Credentials Workflow
|
.. Attention:: Verify that the overlapping nature of these does not result in creating a security vulnerability.

.. Hint:: Consider including a more intuitive way for the user to get back to the point where he or she can employ the restored credenitials. 

Reset password function
***********************

Users are able to reset their password.

.. image:: https://s3.amazonaws.com/peer-downloads/images/TechDocs/Reset+password.png
    :alt: Reset Password Workflow
|
.. Attention:: Verify that the reset password token has a responsible expiry term.

.. Attention:: The password activity screens do not currently appear on the correct DAO website URL.

.. Hint:: Consider adding anti-fraud protections associated with this procedure that are similar to Google and Amazon.
