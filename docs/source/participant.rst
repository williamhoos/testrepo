Participant Workflows
=====================

This section of the document focuses on the subsystems that are directed to end-users who supply information on behalf of themselves or persons for whom they are authorized to act (referred to as "participants").  This description includes the PEER open source code, as well as specialized resources that are provided through API calls with Private Access Connect and PrivacyLayer, both of which are independent services licensed by Genetic Alliance from Private Access.

As described in the :ref:`Overview`, the participant portion of PEER is comprised of 9 major subsystems, and in turn the individual components encompassed within each. These subsystems address:

.. _Engagement:

Participant engagement
~~~~~~~~~~~~~~~~~~~~~~

[ This section coming ]


.. _Sign-up or sign-in:

Sign-up or sign-in
~~~~~~~~~~~~~~~~~~

The sign-up (or sign-in) portion of PEER is provided through leveraging Private Access Connect to provide one-to-many and single sign-on (SSO) functionality that is participant centered in its orientation and integrated with each participant's authorizations, proxies, data sharing and privacy preferences, notifications and accounting of disclosures (audit) reports.  

As shown in the illustration below, the sign-up workflow (for first-time users) consists of 8 steps, and the sign-in workflow (for returning users) consists of just 3, each leveraging 4 shared utility resources:  

.. _Sign-up or sign-in drawing

.. image:: https://s3.amazonaws.com/peer-downloads/images/TechDocs/Sign-up+or+sign-in.png 
    :alt: Sign-up or Sign-in Workflow Illustration

The sign-up or sign-in workflow begins at the conclusion of the participant enagement processes, with selection of the appropriate sequence for the particular user. The sign-up sequence ends when a newly-enrolled participant activates his or her account, which occurs immediately before beginning the authorization and proxy settings workflow.  The sign-in process ends when a returning user enters his or her password from a site-key protected page (or this step is automated based on the user's previous election), at which point the user is taken to their main Dashboard page.

The foregoing subsystems are described in detail in the :ref:`Initial Sign-up` or :ref:`Existing Sign-in` workflow section. 

.. Attention:: Verify proper placement of all MixPanel and Google Analytics cookies

.. Note:: Try to adjust page for mobile-responsive design

.. Hint:: Consider substituting server-side tracking for the existing (cookie-based) user tracking tools 

.. Hint:: Consider making the system more accommodating of screen readers (ie, for visually-impaired users) 

.. Hint:: Consider provding for foreign language translations of all screens


.. _Authorization:

Authorization & proxy settings
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

For many first-time PEER users, the authorizations and proxy settings process begins immediately after the new user activates his or her PEER account.  If the portal sponsor is willing to serve as a proxy *and* has created defualt authorization settings, then PEER will give the user (or their legally authorized representative) an opportunity to review and accept or amend these settings before proceeding. Alternatively, participants can always add to or modify the list of parties whom they authorize to act on their behalf.

.. image::  https://s3.amazonaws.com/peer-downloads/images/TechDocs/Authorization+and+proxy+workflow.png
    :alt: Authorization & proxy settings Workflow Illustration


.. _Privacy:

Privacy settings
~~~~~~~~~~~~~~~~

[ This section coming ]


.. _Dashboard

Dashboard activities
~~~~~~~~~~~~~~~~~~~~

[ This section coming ]


.. _Surveys:

Taking surveys
~~~~~~~~~~~~~~

The surveys portion of PEER uses a survey creation and management system called "PEER Survey Tools" or PST for short.  As shown in the illustration below, PST includes a library of existing questions, tools to customize these questions or create new ones, as well as controls for how these questions are presented to participants. 

.. _taking surveys drawing

.. image::  
    :alt: PEER Survey Tools (PST) Workflow Illustration
| 

.. _eConsent:

Enrolling in studies (eConsent)
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

[ Future feature in planning ]


.. _Utilities

Other utilities
~~~~~~~~~~~~~~~

[ This section coming ]

.. _Participant data

Participant data
~~~~~~~~~~~~~~~~

[ This section coming ]

