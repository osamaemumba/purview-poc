# purview-poc

<p align="center">
  <img src="./images/PurviewQuickStartFlowDiagram.jpeg" width="446" height="550">
</p>

Azure Purview accounts are managed and configured using Azure ARM templates and Azure Functions, using Azure Purview REST APIs.

## Table of Contents

* [APIs](#apis)
* [Prerequisites](#prerequisites)
* [Roles and Access Policies](#roles-and-access-policies)
* [Setup and Configure Purview](#setup-and-configure-purview)
* [OneClick Deployment](#oneclick-deployment)

## APIs

Azure Purview account configurations are managed using its REST APIs through the use of two Azure Functions. One is used to create/configure data resources and the other to delete the provisioned resources.

The following APIs are called in the creation/configuration function:

* [Create/Update Data Source](https://docs.microsoft.com/en-us/rest/api/purview/scanningdataplane/data-sources/create-or-update)
* [Create Scan](https://docs.microsoft.com/en-us/rest/api/purview/scanningdataplane/scans/create-or-update)
* [Run Scan](https://docs.microsoft.com/en-us/rest/api/purview/scanningdataplane/scan-result/run-scan)
* [Create Glossary Term](https://atlas.apache.org/api/v2/resource_GlossaryREST.html#resource_GlossaryREST_createGlossaryTerm_POST)

The following APIs are called in the deletion function:

* [Delete Data Source](https://docs.microsoft.com/en-us/rest/api/purview/scanningdataplane/data-sources/delete)
* [Delete Glossary Term](https://atlas.apache.org/api/v2/resource_GlossaryREST.html#resource_GlossaryREST_deleteGlossaryTerm_DELETE)

## Prerequisites

* Application registration needs to be created to enable authentication against your Azure Active Directory  

  <font size="3"> To be provided</font>  
The following values are needed when using the OneClick Deployment ARM template:-
    * **Resource Group:** Name of the resource group
    * **Region:** Azure region in which to deploy resources
    * **Dls Name:** Name of the ADSL2 storage account
    * **Storage Account Name:** Name of the Blob storage account
    * **New or existing Purview Account:** Whether to create a new account or use an existing one
    * **Purview Account Name:** Name of the Azure Purview Account
    * **Purview Resource Group:** Name of the resource group for purview account (change only in case when using an existing Purview account)
    * **Key Vault Name:** Name of the Azure Key vault
    * **Aad App Client Id:** Client ID of the Application Registration created in prerequisite
    * **Aad App Client Secret:** Client Secret of the Application Registration created in prerequisite
    * **Location:** Location where the resources will be deployed 

## Roles and Access Policies

The following table illustrates the required roles and permissions needed:-

<table>
    <thead>
        <tr>
            <th>Feature/Service</th>
            <th>Role Assigned</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td rowspan=2>Application Registration</td>
            <td>Purview Data Curator</td>
        </tr>
        <tr>
            <td>Purview Data Source Administrator</td>
        </tr>
    </tbody>
</table>

## Setup and Configure Purview

The following steps are required for a successful deployment of the QuickStart.

* Create an Application Registration in Azure Active Directory. Which helps in establishing a trust relationship between application and the Microsoft identity platform. Copy the client id and client secret from App registration as shown below:-

<p align="center">
  <img src="./images/purview-01.gif">
</p>

* To give application access to the subscription, in Azure Subscription console, add a role assignment of role `Purview Data Curator` and `Purview Data Source Administrator` to the Service Principal App registration created earlier as shown below:-

<p align="center">
  <img src="./images/purview-02.gif">
</p>

* Use the link [below](#oneclick-deployment) to start the deployment of resources.

## OneClick Deployment

[![Deploy To Azure](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/1-CONTRIBUTION-GUIDE/images/deploytoazure.svg?sanitize=true)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fosamaemumba%2Fpurview-poc%2Fmain%2Fazuredeploy.json)
