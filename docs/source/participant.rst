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

.. image:: https://s3.amazonaws.com/peer-downloads/images/TechDocs/Sign-up+or+sign-in.png
    :alt: Sign-up or Sign-in Workflow Illustration

The sign-up (or sign-in) portion of PEER is provided through leveraging Private Access Connect to provide one-to-many functionality that is participant centered in orientation and to take into account each participant's data sharing and privacy preferences.  As shown in the above illustration the sign-up workflow (for first-time users) consists of 8 steps, and the sign-in workflow (for returning users) consists of just 3.  

These processes begin at the conclusion of the participant enagement workflow, and end with the beginning of the Autorization and proxy settings workflows in the case of new users, and with returning the user to their Dashboard activities page in the case of returning users.  As shown above, these processes leverage 3 shared resources, which along with the foregoing 11 subsystems are described in the following sections: 

==========================
Sign-up (First-time users) 
==========================

The process irst-time users 
