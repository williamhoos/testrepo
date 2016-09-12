Participant Workflows
=====================

This section of the document focuses on the subsystems that are directed to end-users who supply information on behalf of themselves or persons for whom they are authorized to act (referred to as "participants").  This description includes the PEER open source code, as well as specialized resources that are provided through API calls with Private Access Connect and PrivacyLayer, both of which are independent services licensed by Genetic Alliance from Private Access.

As described in the :ref:`Overview`, the participant portion of PEER is comprised of the following 14 major subsystems workflows, and in turn to the individual components encompassed within it. These 9 subsystems are:

* :ref:`Engagement`

* :ref:`Sign-up`

* :ref:`Authorization`

.. _Engagement:

Participant engagement
~~~~~~~~~~~~~~~~~~~~~~

[ Coming ]


.. _Sign-up:

Sign-up or sign-in
~~~~~~~~~~~~~~~~~~

The sign-up (or sign-in) portion of PEER is provided through leveraging Private Access Connect to provide one-to-many and single sign-on (SSO) functionality that is participant centered in orientation and integrated with each participant's authorizations, proxies,  data sharing and privacy preferences.  

As shown in the illustration below, the sign-up workflow (for first-time users) consists of 8 steps, and the sign-in workflow (for returning users) consists of just 3, each leveraging 3 shared resource workflows:  

.. image:: https://s3.amazonaws.com/peer-downloads/images/TechDocs/Sign-up+or+sign-in.png 
    :alt: Sign-up or Sign-in Workflow Illustration

In both cases, these processes begin at the conclusion of the participant enagement workflow, with selection of the appropriate workfow sequence.  The sign-up sequence ends when the new participant activates his or her account, immediately before beginning the authorization and proxy settings workflows.  The sign-in process ends when the returning user enters his or her password (or this step is automated based on the user's direction) and they are taken to their Dashboard activities page.

The foregoing subsystems are described in the following sections: 

==========================
Sign-up (First-time users) 
==========================

The process irst-time users 
