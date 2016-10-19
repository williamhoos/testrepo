Exporting data from PEER
************************

Export Data CRON
================

This script executes on a set schedule to export any pending requests.  Here is an example crontab entry (set to run every 5 minutes):

  */5 * * * * /usr/bin/php /path/to/PST/source/admin/exportSurveyData.php > /home/ubuntu/cron/exportSurveyData.log
  
Method EX-01
------------

  **exportSurveyData.php**

Function Calls
--------------

EX-02: getExportData()
^^^^^^^^^^^^^^^^^^^^^^

**Inputs**

  * Object exportSettings
	* String loginUrl
  
**Outputs**
 

EX-03: getSurveyDetailsFromId()
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

**Inputs**

  * int surveyId
	* Object mysqli
	 
**Outputs::

  Array<Object> surveyDetails


EX-04: getSurveyUsers()
^^^^^^^^^^^^^^^^^^^^^^^

**Inputs**

  * int surveyId
	*	String widgetId
	*	String dateRange
	  
**Outputs**

   Array<Object> users
		

EX-05: getForeignKeyFromUserIds()
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

**Inputs**

   Array userIds
	  
**Outputs**

   Array<String>
		
    
EX-06:  getExportDetailsFromApi()
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

**Inputs**

  * Object requestData
	* Array <String> foreignkeys
	* Integer portalId
	 	 
**Outputs**

  * Object exportDetails
		

EX-07:  getAllowForeignKeys()
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

**Inputs**

   Object exportDetails
	  
**Outputs**

   Object filteredExportDetails
		

EX-08:  getUserIdsFromForeignKeys()
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

**Inputs**

   Array<String> foreignKeys
	  
**Outputs**

   Array<int> userIds
		

EX-09: getPortalSurveys()
^^^^^^^^^^^^^^^^^^^^^^^^^

**Inputs**

   int portalId
	  
**Outputs**

	Array<Object> surveys
		
    
EX-10:  getSurveyDetailsFromId()
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

**Inputs**
	* Array<Object> surveys
  * Object mysqli
	  
**Outputs**

	 Array<Object> surveys
		

EX-11: getResponseMetatagsExport()
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

**Inputs**
	* String questionType
	*	Array<Object> responses
	*	Object mysqli
	*	Array<Object> choices
	*	int questionId
	  
**Outputs**

	  Object metatags
		
EX-12:  getContacts()
^^^^^^^^^^^^^^^^^^^^^

**Inputs**

   Object exportDetails
			  
**Outputs**

 	 Array<Object> contactDetails


EX-13: getSurveyInstances()
^^^^^^^^^^^^^^^^^^^^^^^^^^^

**Inputs**
	 
  * Array<String> foreignKeys  
  * String whereClause
	*	int portalId
	  
**Outputs**

   Array<Object> instanceDetails
		

EX-14:  s3ExportSave()
^^^^^^^^^^^^^^^^^^^^^^

**Inputs**
	* String filepath
	*	String filename
	*	String mode
	*	int exportId
	*	Object mysqli
	  
**Outputs:
	  
EX-15:  exportSuccessEmail()
^^^^^^^^^^^^^^^^^^^^^^^^^^^^

**Inputs**

 	 int accountId
	 String widgetId
		
**Outputs**


Source files
------------ 

  * admin/exportSurveyData.php
  * includes/functions.php
  * Classes/xlsxwriter.class.php
  * includes/dbcon.php
  
Database tables
---------------

  * peer_surveys_published.my_export
  * peer_surveys_published.surveys
  * peer_surveys_published.data
  * peer_surveys_published.users
  * peer_surveys_published.questions
  * peer_surveys_published.responses
  * peer_surveys_published.choices
  * peer_surveys_published.question_group
  * peer_surveys_published.segment_group
  * peer_surveys_published.survey_sections
  * peer_surveys_published.dynamic_questions
  * peer_surveys_published.meta_tags
  * peer_surveys_published.file_upload
