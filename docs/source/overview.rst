.. _Overview:

Overview
========

This document provides a guide to the subsystems that collectively make up the Platform for Engaging Everyone Responsibly (PEER), including resources that are provided through API calls with Private Access Connect and PrivacyLayer, both of which are independent services licensed by Genetic Alliance from Private Access.

It is **recommended** that this documentation be read in the order listed.  Accordingly, if you elect to skip around within the documentation, be aware that it is written assuming that the preceding sections have been read so you may find it necessary to backtrack to fill in missing concepts and terminology.

The documentation is divided into the data workflow, and the three major functional aspects of the PEER system that influence or generate the content of the data workfow, as well as the subsystems and individual components addressed in each of the three user groups:

* :ref:`Data`

* :ref:`Participants`

* :ref:`Administrators`

* :ref:`Researchers`

.. _Data:

Data perspective
~~~~~~~~~~~~~~~~

As its name indicates, PEER is designed to engage any participant to share his or her information as broadly or as narrowly as he or she wishes, and to change the degree to which his or her information may be discovered and used as circumstances change.  This is achieved through three distinct "actor" workflows, each of whose interactions with the system are summarized in the sections that follow.  

The Data perspective focuses on the system from the point of view of the data it contains, including how these data elements are initially introduced into the system and their respective flow, transformation and/or retention through the point that such data is consumed (i.e., used by the system, viewed, exported or discarded) according to explicit rules.



.. _Participants:

Participant perspective
~~~~~~~~~~~~~~~~~~~~~~~

.. image:: https://s3.amazonaws.com/peer-downloads/images/TechDocs/Participant+Overview.png
    :alt: Participant Overview Illustration
| 
The Participant perspective focuses on the experience of the system from the point of view of end-users who supply information on behalf of themselves or persons for whom they are authorized to act.  This section of PEER is comprised of 9 major subsystem workflows, illustrated above and summarized in the following sections. 

======================
Participant engagement 
======================

PEER is designed to support sponsors in employing various forms of engagement, including both passive (such as badges placed on an affiliate's website) and active means (such as targeted emails).  Unless a participant indicates a wish to be excluded from the use of cookies, PEER administrators who are authorized by the participant can use one of several major tracking systems to assess the effectiveness of their outreach activities.

==================
Sign-up or sign-in 
==================

All first-time visitors to PEER are required to sign-up and returning users sign-in using their assigned credentials.  This is accomplished using Private Access Connect, an OpenID Connect service (which uses OAuth 2.0 standards), to provide single sign-on (SSO) capabilities.   For more information, refer to the :ref:`Sign-up or sign-in` description.

================================
Authorization and proxy settings 
================================

PEER enables properly authorized individuals or groups to act on behalf of others.  Depending on the types of authority that are entailed and the relationship between the parties, such authorization may take place through legal attestation, express authorization, and/or creation of a legal power of attorney.  For more information, refer to the :ref:`Authorization` description.

================
Privacy settings 
================

One of the unique aspects of PEER is that participants are able to deignate who can access their information and for what purposes, which leverages the PrivacyLayer system licensed from Private Access.  Generally speaking, these settings "Allow", "Deny" or defer making a decision to either allow or deny access ("Ask Me") to certain information.  Default privacy settings may be created by the PEER sponsor according to their institutional privacy policy, which the individual participant may accept or revise as part of registration or at a later time.  

====================
Dashboard activities 
====================

Once a participant has successfully registered (as a new user) and/or signed in (as a returning user) to a PEER portal, he or she is taken to the main dashboard.  This page serves as the home screen for the participant or any other individuals for whom the participant is authorized to act, and from which all of the features of the system are readily accessible.

==============
Taking surveys 
==============

PEER provides an extensive facility for conducting longitudinal surveys of participants.  

===============================
Enrolling in studies (eConsent) 
===============================

To the extent that participants wish to take part in research studies and clinical trials that require informed consent, PEER [plans to provide] a facility for enabling the review and consideration of IRB-approved consent documents to which participants can electronically evidence their consent.  

===============
Other utilities 
===============

In addition to the foregoing services, PEER provides a number of utilities such as for checking audit reports, and picking up and responding to notificaitons and requests for access to materials falling under the "Ask Me" category.

================
Participant data 
================

In addition to sharing access to their information with third-parties, participants are able to use PEER to retrieve, edit and/or supplement information aoubt heseves..  


.. _Administrators:

Administrator perspective
~~~~~~~~~~~~~~~~~~~~~~~~~

.. image:: https://s3.amazonaws.com/peer-downloads/images/TechDocs/Administrator+Overview.png
    :width: 89%
    :alt: Administrator Overview Illustration
| 
Organizations (or individuals) who operate a PEER registry are called "PEER sponsors". If a PEER sponsor is part of a hosted network (such as Genetic Alliance's SaaS-based PEER service), they may be required to comply with that authority's requirements as a condition to receiving rights to use the network, various trademarks and/or copyrighted materials in conjunction with their use of PEER.  

Once approved, PEER sponsors administer the display and operation of one or more portals from an administrative user account. The Administrator perspective focuses on the experience of the system from the point of view of these sponsors who design and configure the operation and content of their registry to achieve their goals and to serve their members and the individuals they invite to register as end-users.  The Administrator perspective of PEER is comprised of 8 major subsystem workflows, illustrated above and summarized in the following sections. 

==================
Sign-up or sign-in 
==================

All first-time visitors to PEER are required to sign-up, and returning users sign-in using their assigned credentials.  As in the case of individual participants, this is accomplished using Private Access Connect, an OpenID Connect service (which uses OAuth 2.0 standards), to provide single sign-on (SSO) capabilities, and in the case of PEER administrators to establish the individual's role-based access privileges.

========================
Sponsor account creation 
========================

PEER enables properly authorized administrators to create and provision one or more subordinate administrative user accounts, who in each case may be designated a subset of the assignor's rights and authorities.

========================
Create or update portals
========================

PEER enables properly authorized administrators to create and configure the appearance and location of one or more PEER portals, as well as to designate various aspects of each portal's operation.

================
Default settings 
================

As part of provisioning a new PEER portal (or modifyng an existing portal), the administrator must establish various default settings regarding participant's user experience and how the portal will function.  Some of these settings are required, some are pre-set but may be revised, and others are optional.  

==============
Curate surveys 
==============

PEER includes 16 survey question types that may be used by properly authorized administrators to generate one or more static or longitudinal survey instruments.  Genetic Alliance has developed an extensive library common data instruments (CDIs) that contain two or more Common Data Elements (CDEs) assocated with a topic.  Sponsors may create a survey comprised of multiple CDIs or create their own questions from scratch or as a modification of a previous question.  Questions and answers may be tagged regarding the topics coverd so that differently-phrased questions about the same topic will nevertheless be grouped together for analysis purposes. 

================================
Messages, outreach and follow-up 
================================

Properly authorized administrators may use various facilities to develop the content and designate the timing and distribution of communications for use in participant-related engagement and event-triggered follow-up.  These communications may be delivered to participant's email address or displayed on the dashboard the next time the participant returns to this screen.

===================
Accessing your data 
===================

Properly authorized researchers may discover, view, edit, and/or export participant data to the extent they have been granted rights by the individual partipants' settings or the sponsor's settings as applicable. 

====================
Statistics dashboard 
====================

In addition to information accessible to adminstrative personnel through a thrid-party application such as Google Analytics or Mix Panel, PEER provides a dashboard that permits properly authorized administrative personnel to drill down into participants' aggregated results, to the level of detail that he or she is entitled.  


.. _Researchers:

Researcher perspective
~~~~~~~~~~~~~~~~~~~~~~

.. image:: https://s3.amazonaws.com/peer-downloads/images/TechDocs/Researcher+Overview.png
    :width: 67%
    :alt: Researcher Overview Illustration
| 
Individuals who seek to access data submitted by PEER participants are called "researchers". Once authenticated, researchers may discover and access information to which they are authorized.   

Once approved, researchers find and access information from a researcher account. The Researcher perspective focuses on the experience of the system from the point of view of these researchers who query and acquire information to the extent they are authorized by the data sharing settings established by or on behalf of participants.  The Reseracher perspective of PEER is comprised of 6 major subsystem workflows, illustrated above and summarized in the following sections.

==================
Sign-up or sign-in 
==================

All first-time visitors to PEER are required to sign-up, and returning users sign-in using their assigned credentials.  As in the case of individual participants and administrative personnel, this is accomplished using Private Access Connect, an OpenID Connect service (which uses OAuth 2.0 standards), to provide single sign-on (SSO) capabilities, and in the case of researchers to establish the individual's data access privileges.

======================
Data access privileges 
======================

PEER enables properly authorized researchers to discover, analyze, view, and export data based on the privacy settings in effect at the time such access is proposed, which may be exclusively for that individual or to a group of multiple resarchers in which the individual researcher is a member or affiliated at the time of the proposed access.

==================
Searching for data
==================

PEER enables properly authorized administrators to search for data based on various criteria, including key word, concept, survey instrument, sponsoring organization and time frame.

=================================
Data analysis, viewing and export 
=================================

In each case, the researcher's rights to analyze, view and/or export the results of such searches of PEER data are limited to only the data to which the researcher is entitled access based on the then current instantiation of each participants' data sharing settings.  

===================================
Contacting prospective participants
===================================

To the extent expressly permitted, PEER provides researcehers with contact information and express authority to contact individuals or a designated person acting on their behalf.  In addition, to the extent that data access is restricted by an "Ask me" setting, the researcher may invoke an automated process by PrivacyLayer to try and secure such additional access rights from the individual or an authorized party acting on thier behalf (and who will be permitted to decide whether or not to allow, deny or continue to pend such access researcher request).  

========================
Getting informed consent  
========================

Where the researcher proposes to use such information for a specific research purpose under a protocol that requires informed consent, PEER [plans to provide] an additional facility for uploading the IRB-approved consent documents to which participants can electronically evidence their consent if they wish to take part in such research study or clinical trial.


