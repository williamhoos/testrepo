.. _Data perspective:

================
Data perspective 
================

As indicated in the :ref:`Overview` discussion respecting :ref:`Data`, this section describes how PEER presently handles data from the point that it is initially entered into the system by a user, proxy or administrator, to the point that that data is either viewable onscreen or appears in an exported file or document, or is taken into account in any calculation of reported results. 

Where not self-descriptive, this section includes an explanation of each function and/or subroutine that’s involved in this process, including its range of input and output values, any associated program variables and constants, and an explanation for how these are used by the application in handling or manipulating data; and 


.. _Data summary:

Summary
*******

PEER's data layer utilizes the MySQL Server, Version 5.5.37 RDBMS to store data.

Data Model
----------

The data layer associate with the JAVA components of the application adopt a tradition online transaction processing model. To the extent possible, all data models are normalized minimally to the third normal form.

Table Design
------------

For security and efficiency purposes, all tables used within PEER have a surrogate key serving as a primary key, and that is used only for internal system purposes.  
 
Data abstraction layers 
-----------------------

PEER data contained in dbPPMS is presented with multiple levels of abstraction in the data access layer and the data tier, with each layer serving a different purpose.

Views
-----

The following data abstraction layers are implemented:

Views are used as a logical layer for all CRUD operations.  This helps to avoid coupling the data access layer tightly with the physical implementation of the database tables.

There are two types of views

  * **Base views** - A base view initially will be a logical mirror of the physical table.  As adapters to the physical tables, the need will arise to change base views to maintain the same logical view to the data clients in the future.  All changes must be constrained to ensure that base views maintain the integrity of data after CUD (Create Update and Delete) operations.
  
  * **Non-base views** - Will be all other views that are not a base view. *i.e.*, aggregate views, compount views, ...etc.  All these views are read only.  No CUD operation should be performed on these views.

.. Attention::  We need to confirm through the audit that the foregiong description of views is still accurate.

Data Objects
------------

Views into the dbPPMS database are mapped to data objects.  All data clients beyond the data mediator layer use Data Objects as the primary data exchange format.  This ensures that data clients get no insight to the database schema.  There are multiple benefits to this:

  * Database security in case of security breaches
  * No coupling between the middle tier and the data tier
  
Naming conventions
------------------

The following table lists the recommended naming conventions that are generally employed in the data persistence layer. The bold part of the rule is a constant.  The part in [Square Brackets] is a pick from a predefined domain of values.  The *italicized* part is user defined.

+-----------------+-------------------------------------------+--------------------------------------+----------------------------------------+
| Type            | Rule                                      | Example                              | Notes                                  |
+=================+===========================================+======================================+========================================+
| database        | **db** *Application*                      | **db** *PPMS*                        |                                        |
|                 |                                           |                                      |                                        |
|                 |                                           |                                      |                                        |
+-----------------+-------------------------------------------+--------------------------------------+----------------------------------------+
| table           | **tbl** [SubSystem] *UserDefined*         | **tbl** PL *PrivacyDirective*        | Tables follow a single naming          |
|                 | [SubSystem] can be any of the following:  |                                      | convention (*e.g.*, privacyDirective   |
|                 |                                           |                                      | for all privacy directive rows         |
|                 |   * PL - PrivacyLayer                     |                                      |                                        |
|                 |   * Sys - System tables                   |                                      |                                        |
|                 |   * Sha - Shared tables                   |                                      |                                        |
+-----------------+-------------------------------------------+--------------------------------------+----------------------------------------+
| base view       | **vew** [SubSustem] [UserDefined section  | **vew** PL *PrivacyDirective*        | vewPLPrivacyDirective is the base      |
|                 | of the table that this view represents]   |                                      | view for tblPLPrivacyDirective         |
+-----------------+-------------------------------------------+--------------------------------------+----------------------------------------+
| non-base view   | **vew** [SubSystem] *UserDefined*         | **vew** PL *PersonRecordDetail*      |                                        |
|                 |                                           |                                      |                                        |
+-----------------+-------------------------------------------+--------------------------------------+----------------------------------------+
| function        | **fnc** [SubSystem] *UserDefined*         | **fnc** Sha *TrimString*             |                                        |
|                 |                                           |                                      |                                        |
+-----------------+-------------------------------------------+--------------------------------------+----------------------------------------+
| procedure       | **prc** [SubSystem] *UserDefined*         | **fnc** Sha *ComputeAlertDelay*      |                                        |
|                 |                                           |                                      |                                        |
+-----------------+-------------------------------------------+--------------------------------------+----------------------------------------+
| primary key:    | **ID** [UserDefined section of the table] | **ID** *User*                        |                                        |
| standard tables |                                           |                                      |                                        |
|                 |                                           |                                      |                                        |
+-----------------+-------------------------------------------+--------------------------------------+----------------------------------------+
| primary key:    | **ID**                                    | **ID**                               | No need to be specific here since this |
| pivot tables    |                                           |                                      | ID will usually not be referenced      |
| (many-to-many)  |                                           |                                      | anywhere other than joins.             |
+-----------------+-------------------------------------------+--------------------------------------+----------------------------------------+
| foreign key     | **FK** *UserDefined* _[PrimaryKey]        | **FK** *SessionUser* _IDUser         |                                        |
|                 |                                           |                                      |                                        |
+-----------------+-------------------------------------------+--------------------------------------+----------------------------------------+
| index           | Designated data is indexed by Elastic     |                                      |                                        |
|                 | Search                                    |                                      |                                        |
+-----------------+-------------------------------------------+--------------------------------------+----------------------------------------+
| date columns    | **Date** *UserDefined*                    | **Date** *Created*                   |                                        |
|                 |                                           | **Date** *Modified*                  |                                        |
+-----------------+-------------------------------------------+--------------------------------------+----------------------------------------+

.. _survey database:

PEER survey database
********************

Each of the PEER staging, beta and production environments contains a database entitled "**peer_surveys_published**".  This database is generally the same in all three environments, although the table structures and fields contained inside certain tables may be slightly different depending on development work that is taking place at the time, and the progress of migrating that work from staging into the beta, and production environments.  

.. _Data table:

Data table
==========

Survey data that is collected through PEER is stored in a table entitled "**Data**" that is generated by the following SQL instruction::

 CREATE TABLE `data` (
 `id` BIGINT(20) UNSIGNED NOT NULL AUTO_INCREMENT,
 `survey_id` BIGINT(20) UNSIGNED NOT NULL DEFAULT '0',
 `session_id` BIGINT(32) UNSIGNED NOT NULL DEFAULT '0',
 `date_added` DATETIME NULL DEFAULT NULL,
 `widget_id` VARCHAR(50) NULL DEFAULT NULL,
 `user_id` BIGINT(20) NULL DEFAULT NULL,
 `variable_date` DATE NULL DEFAULT NULL,
 `question_id` BIGINT(20) UNSIGNED NULL DEFAULT NULL,
 `variable_value` VARCHAR(2000) NULL DEFAULT NULL,
 `response_value` VARCHAR(2000) NULL DEFAULT NULL,
 `unit_of_measure` BIGINT(20) UNSIGNED NULL DEFAULT NULL,
 `group_id` BIGINT(20) UNSIGNED NULL DEFAULT NULL,
 `segment_id` INT(11) NOT NULL,
 `skipped` TINYINT(2) UNSIGNED NULL DEFAULT '0',
 `is_dynamic` TINYINT(3) UNSIGNED NULL DEFAULT '0',
 `instance_id` INT(11) NOT NULL,
 `dynamic_question_id` INT(11) NOT NULL,
 `proxy_user_id` INT(11) NOT NULL,
 PRIMARY KEY (`id`),
 INDEX `Index 2` (`survey_id`, `session_id`, `group_id`, `question_id`, `widget_id`, `user_id`, `date_added`, `variable_date`)
 )
 COLLATE='utf8_general_ci'
 ENGINE=InnoDB
 AUTO_INCREMENT=2042996

As noted, the Data table employs the **InnoDB engine**, which provides the standard ACID-compliant transaction features, along with foreign key support and is included as standard in most binaries distributed by MySQL AB. The InnoDB software is dual licensed, and is distributed under the GNU General Public License, and can be licensed to parties wishing to combine InnoDB in proprietary software.  The Data table uses **utf8_general_ci** as its default character set for storing data.  

An image of the Data table, which as of October 2016 comprises approximately 106.5 MB in beta, and ___ MB in production, is shown below:

.. image:: https://s3.amazonaws.com/peer-downloads/images/TechDocs/PEER+survey+data+table.png
     :alt: PEER survey data table    
     
Properties
==========

As indicated, the table contains a total of 18 columns, each of which has a number of properties including (as indicated by the key shown to the left of the numbered columns) that these values are indexed and the results cached to make the data more efficient to query.  The gold key designates the Primary Key for the index, and each of the green keys indicate other properties that are included in the data index.  

Other properties include the "**Unsigned**" column, which when checked indicates that the data field can only contain positive numerical values, and when unchecked indicates that it can contain negative numerical values. The "**Allow Null**" column, which when checked indicates that the data field is permitted to contain a null value, and when unchecked requires that some value be stored in the field.   

The **Default**" column indicates the value the SQL server will use when data is entered into the table that does not designate another value for that field.

Contents
========

The primary data contained in the table is described below:  

#.  The **id** is auto-incremented, and contains an integer of up to 20 characters.  In operation, the mySQL server assigns a unique ID value automatically whenever a new record is inserted into the database.  As of October 2016, there are over 2 million records that have been entered into the beta database, and ____ million records in the production database. 

#.  The **survey_id** designates the survey to which the answer pertains. Within PEER, surveys are comprised of one or more topics, which are referred to in the Data table as a *"segment"*. In turn, topics are comprised of one or more instruments, which are referred to in the Data table as a *"group"*.  And in turn, instruments are comprised of one or more questions, which are referred to in the Data table as a *"question"*.

#.  The **session_id** indicates the session the user was in when the answer to a question was provided by the participant.

#.  The **date_added** indicates the date and time when the question was answered.

#.  The **widget_id** indicates the portal the user was employing when the answer was entered.

#.  The **user_id** indicates the participant profile that provided the answer.

#.  The **variable_date** indicates the date (but not the time) when a question was answered.  This data is a subset of the *date_added* field, and was used in an earlier release of PEER, but is not presentely being used.

#.  The **question_id** indicates the question that was being answered.  Each question is assigned a unique ID number, and is recorded as a zero (0) for an introduction or conclusion, in which cases no question is posed despite the survey presenting information.  Groups of PEER questions form *"instruments"*, which are referred to in the Data table as *"groups"*

#.  The **variable_value** indicates 

#.  The **response_value** indicates

#.  The **unit_of_measure** indicates

#.  The **group_id** indicates the instrument within which a group of one or more survey questions is presented, and to which such response (*i.e.*, reflected by the *variable_value*) pertains.  

#.  The **segment_id** indicates the topic within which a group of one or more instruments (each referenced by a unique *group_id*) is presented

#.  The **skipped** column records when the user clicked on the "skip" button rather than respond to a question

#.  The **is_dynamic** flag is 

#.  The **instance_id**

#.  The **dynamic_question_id** is used to 

#.  The **proxy_user_id** indicates 



.. Attention:: Slider question anomaly... reported in Pivotal Tracker as *https://www.pivotaltracker.com/story/show/131929961*
