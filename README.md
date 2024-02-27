# PowerAutomate for MinistryPlatform

This project contains the MinistryPlatform Custom Connector that can be imported into PowerAutomate to enable simplified interactions between MinistryPlatform and PowerAutomate. Please note that all REST API endpoints are NOT available in this custom connector.

## Setup MinistryPlatform for PowerAutomate

## Installation

## Update Custom Connector

You can update a MP Custom Connector with an updated definition by downloading the latest definition and then using that definition to do a quick update.

### Steps

- Download the Defintion from Github [Download Connector](Connector/MinistryPlatform.swagger.json)
- Search and Replace the Hostname (mpi.ministryplatform.com) with your hostname ![alt text](Assets/UpdateConnectorStep2.png)
- Copy the updated JSON Definition
- Paste this definition into the Custom Connector Swagger Editor ![Search and Replace](Assets/UpdateConnectorStep1.png) ![alt text](Assets/UpdateConnectorStep3.png)
- Click the OK Button to convert JSON to YAML ![alt text](Assets/UpdateConnectorStep3a.png)
- Make sure you click the Update Connector Button ensure the changes are saved ![alt text](Assets/UpdateConnectorStep4.png)
- All Finished...Your Connector should be up to date

## Release History

- 2024.2.26
  - Added Text Support to Communications Endpoint
  - Added GitHub Repository and Custom Connector
