.. _Data perspective:

================
Data perspective 
================

As indicated in the :ref:`Overview` discussion respecting :ref:`Data`, this section describes how PEER presently handles data from the point that it is initially entered into the system by a user, proxy or administrator, to the point that that data is either viewable onscreen or appears in an exported file or document, or is taken into account in any calculation of reported results. 

Where not self-descriptive, this section includes an explanation of each function and/or subroutine that’s involved in this process, including its range of input and output values, any associated program variables and constants, and an explanation for how these are used by the application in handling or manipulating data; and 


.. _Architecture

Achitecture
***********

The following illustration summarizes the overall architecture of the system with respect to data in/data out, including links to which pages, views or fields of the system are used to introduce data elements and the respective flow, transformation and/or retention of such data through the point that the data is consumed (*i.e.*, used by the system, viewed, exported or discarded) according to explicit rules.

.. image:: TBD 
     :alt: Overall system architecture from the perspective of data it contains
|

.. _Existing user verification

Existing user verification
**************************

Slider question anomaly... reported in Pivotal Tracker as *https://www.pivotaltracker.com/story/show/131929961*
