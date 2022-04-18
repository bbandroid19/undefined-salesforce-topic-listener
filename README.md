# accelerator-salesforce-topic-listener

This project listens to case data triggered from Salesforce and post the data into Anypoint MQ.This listener listens to the push topic configured in salesforce on case object.The following fields are part of the push topic query Status, Type, Priority, Id, Subject.These fields are received as JSON Object from Salesforce.The received json payload is published to Anypoint MQ.In case of any failure during the flow, the incoming orginal payload is published to dead letter queue by the exception handler.

## Prerequisite Setup

 - Create a push topic on case object with below information in Salesforce.Create Custom Fields   Sync_With_SNOW__c and Sync_With_JIRA__c with checkbox as field type. 
 
        API Version 46.0 (Replace with current API version is applicable)
        Name All_Cases (May not contain spaces)
        NotifyForOperationCreate true
        NotifyForOperationDelete false
        NotifyForOperationUndelete false
        NotifyForOperationUpdate false
        Query SELECT Id  FROM Case where Sync_With_SNOW__c=true or Sync_With_JIRA__c=true
        
 - Use the same Salesforce Org credentials in YAML File.
 - Encrypt the salesforce password and token with mule.key and add it to {env}-secured-yaml file where env is local,dev.
 - Create client application in Anypoint MQ if not created and copy the client id and client secret.
 - Encrypt client secret with mule.key and add client id and client secret to {env}-secured.yaml where env is local,dev under respective property.
 - Create an exchange named sfa-sfdc-case-exchange in Anypoint MQ if not present.
 - Create a queue named sfa-sfdc-case-queue and bind to sfa-sfdc-case-exchange in Anypoint MQ if not present.
 - Create a queue named sfa-sfdc-case-dlq and assign as dead letter queue to sfa-sfdc-case-queue in Anypoint MQ if not present.

## Installation instructions for Studio

The following instructions assume you have already configured your Maven 
`settings.xml` file according to the Setup Guide.

- Clone the project using any Github client or Anypoint Studio Git plugin
- Import the project into your workspace
- Modify the `config-local.yaml` properties appropriately
- To run the project, right-click the project and select Run As --> Mule Application

## Deployment instructions for CloudHub

Ensure the Maven profile `CloudHub-DEV` has been properly configured in your 
`settings.xml` file. In particular, make sure the Anypoint MQ client ID and secret and Salesforce credentials 
are provided.

Update the `config-dev.yaml` properties appropriately and then use one of the following 
scripts to deploy application to Cloud Hub:
   
- packageDeploy.sh or deployOnly.sh (Mac/Linux)
- packageDeploy.cmd or deployOnly.cmd (Windows)
