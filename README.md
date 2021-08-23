# Azure Purview QuickStart

<p align="center">
  <img src="./images/PurviewQuickStartFlowDiagram1.jpeg" width="446" height="400">
</p>

Azure Purview accounts are created using ARM templates and are configured by using the Purview REST APIs.

## Table of Contents

* [Introduction](#introduction)
* [Prerequisites](#prerequisites)
* [Deploy and Configure Purview Account](#deploy-and-configure-purview-account)
* [Removing the QuickStart resources from the Purview Account](#removing-the-quickstart-resources-from-the-purview-account)
* [Troubleshooting](#troubleshooting)

## Introduction

This QuickStart is a one-click solution to setup and configure Azure Purview Accounts. It creates storage accounts, loads sample data into them, connects as data sources to the Purview Account and creates Business Glossary Term with the help of ARM templates and Purview REST APIs.

Click the following button to deploy the Purview QuickStart:-

[![Deploy To Azure](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/1-CONTRIBUTION-GUIDE/images/deploytoazure.svg?sanitize=true)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fosamaemumba%2Fpurview-poc%2Fmain%2Fazuredeploy.json)

Use the OneClick Deployment button above to start the deployment. Currently it takes ~25-30 minutes for one complete deployment.

## Prerequisites

There are two main prerequisites before deploying the Quickstart:-
* Create Application Registration
* Assignment of required roles

### 1. Create Application Registration

* Application registration needs to be created to enable authentication against the Azure Active Directory. Create an Application Registration in Azure Active Directory, which helps in establishing a trust relationship between application and the Microsoft identity platform.

* Follow the steps below to create an application registration:-

    1. In Azure Homepage, search for `Active Directory` and select `Azure Active Directory` from the drop-down list.

    <p align="center">
      <img src="./images/purview-01-01.png">
    </p>

    2. In Azure Active Directory page, select `App registrations` from the left pane.

    <p align="center">
      <img src="./images/purview-01-02.png">
    </p>

    3. Click `New registration` to create a new Application Registration.

    <p align="center">
      <img src="./images/purview-01-03.png">
    </p>

    4. Enter the following information:-
        * `Name`: Name for the new Application Registration.
        * `Supported account types`: Defines who can use this application
        * `Redirect URI`: (Optional) Authentication response is sent to this URI 

        **Supported account type** and **Redirect URI** can be left as default. Click on `Register` at the bottom when done.

    5. After a new registration is created, copy the client id as shown below:-

    <p align="center">
      <img src="./images/purview-01-04.png">
    </p>

    6. Click on `Certificates & secrets` on the left pane:-

    <p align="center">
      <img src="./images/purview-01-05.png">
    </p>

    7. Click on `New client secret` to create a new secret id for application registration:-

    <p align="center">
      <img src="./images/purview-01-06.png">
    </p>

    8. Enter a description, select the expiration of secret id and click `Add` at the bottom:-

    <p align="center">
      <img src="./images/purview-01-07-updated.png">
    </p>

    9. Copy the secret id under `Value` column as shown below:-

    <p align="center">
      <img src="./images/purview-01-08.png">
    </p>

    10. The complete procedure of creating the Application Registration and copying Client id and secret is shown below:-

    <p align="center">
      <img src="./images/purview-01.gif">
    </p>


### 2. Assignment of required roles


* To give application access to the subscription, in Azure Subscription console, add a role assignment of role `Purview Data Curator` and `Purview Data Source Administrator` to the Service Principal App registration created earlier.

  The following table illustrates the required roles and permissions:-

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

* Follow the steps below to assign required roles to the application registration:-

    1. With Application registration created, search for `Subscription` and select `Subscriptions` from the drop-down list:-

    <p align="center">
      <img src="./images/purview-02-01.png">
    </p>

    2. Select the Azure Subscription and click `Access control (IAM)`:-

    <p align="center">
      <img src="./images/purview-02-02.png">
    </p>

    3. To attach the required roles to Application registration, choose `Add` and select `Add role assignment` from the drop down list:-

    <p align="center">
      <img src="./images/purview-02-03.png">
    </p>

    4. To add role assignment, search for `Purview Data Curator` role and under `Select` search for the service principal created earlier as shown below. Click on the application registration to select it:-

    <p align="center">
      <img src="./images/purview-02-04.png">
    </p>

    5. Make sure the application registration shows under `Selected members`. Click `Save` to add role:-

    <p align="center">
      <img src="./images/purview-02-05.png">
    </p>

    6. Repeat the same procedure to add `Purview Data Source Administrator` role to the Application registration as well.

    7. The complete procedure of adding the required roles to the Application Registration is shown below:-

    <p align="center">
      <img src="./images/purview-02.gif">
    </p>

## Deploy and Configure Purview Account

* The following values are needed when using the OneClick Deployment ARM template:-
    * **Resource Group:** Name of the resource group.
    * **Region:** Azure region in which to create the resource group.
    * **Data Lake Account Name:** Name of the ADSL2 storage account. This storage account will be created and registered as a data source with Purview account.
    * **Storage Account Name:** Name of the Blob storage account. This storage account will be created and registered as a data source with Purview account.
    * **New or existing Purview Account:** Whether to create a new Purview account or use an existing one.
    * **Purview Account Name:** Name of the Azure Purview Account. In case of new Purview account, give a new name. To use an existing one, enter the name of existing purview account.
    * **Purview Resource Group:** Povide the name of the resource group for the existing Purview account. Leave as it is if you are creating a new Purview account or if the existing Purview account is present in the same resource group where all other resources are going to be deployed.
    * **Key Vault Name:** Name of the Azure Key vault. This is used to store Client Secret needed to perform API calls.
    * **Factory Name:** Name for the Azure Data Factory. This Data Factory will be created and load the data into the storage accounts.
    * **Aad App Client Id:** Client ID of the Application Registration created in prerequisite.
    * **Aad App Client Secret:** Client Secret of the Application Registration created in prerequisite.
    * **Location:** Location where the resources will be deployed. It is, by default, set to the region of the resource group.

* Click the following button to deploy the Purview QuickStart:-

     [![Deploy To Azure](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/1-CONTRIBUTION-GUIDE/images/deploytoazure.svg?sanitize=true)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fosamaemumba%2Fpurview-poc%2Fmain%2Fazuredeploy.json)

* Currently it takes ~25-30 minutes for one complete deployment. 

## Removing the QuickStart resources from the Purview Account
* After the deployment is complete, resources can be deleted by following the steps below:-

    1. On the deployment page, click on `Outputs`:-

    <p align="center">
      <img src="./images/purview-03-update-01.png">
    </p>

    2. Copy and run the `deletePurviewFunctionTriggerUrl` URL from the deployment outputs:-

    <p align="center">
      <img src="./images/purview-03-update-02.png">
    </p>

    3. The complete procedure to remove the created resources is shown below:-

    <p align="center">
      <img src="./images/purview-03-update.gif">
    </p>


## Troubleshooting

This portion lists solutions to problems one might encounter with Purview OneClick Deployment.

### Common Problems

Here is a list of common problems one might encounter while deploying the template:-

#### 1. Purview Account created but `no` QuickStart resources were found:

In case the deployment is successful and there aren't any resources created. It can be verified by going to the Purview Account Portal. That is caused by the time Purview Account needs to set up properly. Resources can be provisioned using the `configurePurviewFunctionTriggerUrl` url from the deployment outputs. The same url needs to be used to trigger the creation function after some time, in case it does not work in first attempt. It should output `AzurePurview Creation function triggered successfully`. 

The following explains the working of deployment script in this case:-

  * Unresponsive APIs return a status code of 500.
  * The deployment script waits for 1 minute and retries to get a status code of 200.
  * In case of success status code return, the script creates the resources.
  * In case of status code 500 return, the script does 15 retries, with 1 minute wait each time.
  * If APIs are still unresponsive, the deployment script completes the deployment without provisioning resources.

The following gif illustrates the procedure for creation of resources:-

<p align="center">
  <img src="./images/purview-04.gif">
</p>

#### 2. Failed Deployment

* Deployment failed on `runDataFactory` step.
* Deployment failed on `triggerConfigurePurviewFunction` step.

In case the deployment failed, this can be caused by the Powershell modules used in the deployment script.
Make sure that:-

* The App Registration client ID and client secret are correct.
* The application service principal has access to the `Purview Data Curator` and `Purview Data Source Administrator` roles at subscription level.

If the above requirements are satisfied, rerunning the deployment should resolve this issue.
