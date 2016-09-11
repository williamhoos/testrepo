.. techdocs documentation master file, created by
   sphinx-quickstart on Wed Sep  7 10:33:45 2016.
   You can adapt this file completely to your liking, but it should at least
   contain the root `toctree` directive.

PEER for Open Source Documentation
==================================

This document provides a guide to the subsystems that collectively make up the Platform for Engaging Everyone Responsibly (PEER), including resources that are provided through API calls with Private Access Connect and PrivacyLayer, both of which are independent services licensed by Genetic Alliance from Private Access.

It is **recommended** that this documentation be read in the order listed.  Accordingly, if you elect to skip around within the documentation, be aware that it is written assuming that the preceding sections have been read so you may find it necessary to backtrack to fill in missing concepts and terminology.

The documentation is divided into the three major functional aspects of the PEER system, and in turn to the subsystems and individual components addressed to the following user groups:

* :ref:`Participants`
* :ref:`Administrators`
* :ref:`Researchers`

.. _Participants:

Participants
~~~~~~~~~~~~
.. image:: https://s3.amazonaws.com/peer-downloads/images/TechDocs/Participant+Overview.png
    :alt: Participant Overview Illustration

The Participant portion of PEER is directed to end-users who supply information on behalf of themselves or persons for whom they are authorized to act.  This section of PEER is comprised of 9 major subsystem workflows, illustrated above and described in the following sections. 

======================
Participant engagement 
======================

PEER is designed to support sponsors in employing various forms of engagement, including both passive (such as badges placed on an affiliate's website) and active means (such as targeted emails).  Unless a participant indicates a wish to be excluded from the use of cookies, PEER administrators who are authorized by the participant can use one of several major tracking systems to assess the effectiveness of their outreach activities.

==================
Sign-up or sign-in 
==================

All first-time visitors to PEER are required to sign-up and returning users sign-in using their assigned credentials.  This is accomplished using Private Access Connect, an OpenID Connect service (which uses OAuth 2.0 standards), to provide single sign-on (SSO) capabilities.    

================================
Authorization and proxy settings 
================================

PEER enables properly authorized individuals or groups to act on behalf of others.  Depending on the types of authority that are entailed and the relationship between the parties, such authorization may take place through legal attestation, express authorization, and/or creation of a legal power of attorney.

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

=========================================
Enrolling in reseearch studies (eConsent) 
=========================================

To the extent that participants wish to take part in research studies and clinical trials that require Informed Consent, PEER [is developing] a facility for enabling the review and consideration of such consent documents.  

===============
Other utilities 
===============

In addition to the foregoing services, PEER provides a number of utilities such as for checking audit reports, and picking up and responding to notificaitons and requests for access to materials falling under the "Ask Me" category.

================
Participant data 
================

In addition to sharing access to their information with third-parties, participants are able to use PEER to retrieve, edit and/or supplement information aoubt heseves..  


.. _Administrator:

Administrator Workflows
~~~~~~~~~~~~~~~~~~~~~~~
.. image:: https://s3.amazonaws.com/peer-downloads/images/TechDocs/Administrator+Overview.png
    :width: 89%
    :alt: Administrator Overview Illustration
    
This is a paragraph is all about administrator.


.. _Researcher:

Researcher Workflows
~~~~~~~~~~~~~~~~~~~~
.. image:: https://s3.amazonaws.com/peer-downloads/images/TechDocs/Researcher+Overview.png
    :width: 67%
    :alt: Researcher Overview Illustration
    
This is a paragraph is all about research.


Proposing changes to PEER
~~~~~~~~~~~~~~~~~~~~~~~~~

Improving PEER's code, documentation and tests are ongoing tasks.  While these are never going to be "finished", thanks to the efforts of PEER's founders and the support of several early adopter organizations and sponsors, 4-month intensive effort was dedicated to creating a baseline representation immediately prior to PEER being migrated from closed to open source software.  This effort permitted the artifacts of approximately five years of continuous development sprints to be scrubbed, functional test suites to be developed and documentation to be created. 

As an integral part of this effort, a number of potential development activities were identified as possible areas for future enhancement, and these are identified throughout the documentation.  


Full Table of Contents
~~~~~~~~~~~~~~~~~~~~~~

Contents
^^^^^^^^

.. toctree::
   :maxdepth: 3

   quickstart
   license
   help
   test
