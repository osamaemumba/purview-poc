# Azure Purview QuickStart

<p align="center">
  <img src="./images/PurviewQuickStartFlowDiagram1.jpeg" width="446" height="400">
</p>

Azure Purview accounts are created using ARM templates and are configured by using the Purview REST API's.

## Table of Contents

* [Prerequisites](#prerequisites)
* [Deploy and Configure Purview Account](#deploy-and-configure-purview-account)
* [Removing the QuickStart resources from the Purview Account](#removing-the-quickstart-resources-from-the-purview-account)
* [Troubleshooting](#troubleshooting)

## Prerequisites

* Application registration needs to be created to enable authentication against the Azure Active Directory.

* Create an Application Registration in Azure Active Directory, which helps in establishing a trust relationship between application and the Microsoft identity platform.

* In Azure Homepage, search for `Active Directory` and select `Azure Active Directory` from the drop-down list.

<p align="center">
  <img src="./images/purview-01-01.png">
</p>

* In Azure Active Directory page, select `App registrations` from the left pane.

<p align="center">
  <img src="./images/purview-01-02.png">
</p>

* Click `New registration` to create a new Application Registration.

<p align="center">
  <img src="./images/purview-01-03.png">
</p>

* Enter the following information:-
    * `Name`: Name for the new Application Registration.
    * `Supported account types`: Defines who can use this application
    * `Redirect URI`: Authentication response is sent to this URI 

    **Supported account type** and **Redirect URI** can be left as default. Click on `Register` at the bottom when done.

* After a new registration is created, copy the client id as shown below:-

<p align="center">
  <img src="./images/purview-01-04.png">
</p>

* Click on `Certificates & secrets` on the left pane:-

<p align="center">
  <img src="./images/purview-01-05.png">
</p>

* Click on `New client secret` to create a new secret id for application registration:-

<p align="center">
  <img src="./images/purview-01-06.png">
</p>

* Enter a description, select the expiration of secret id and click `Add` at the bottom:-

<p align="center">
  <img src="./images/purview-01-07-updated.png">
</p>

* Copy the secret id under `Value` column as shown below:-

<p align="center">
  <img src="./images/purview-01-08.png">
</p>

* The complete procedure of creating the Application Registration and copying Client id and secret is shown below:-

<p align="center">
  <img src="./images/purview-01.gif">
</p>

* To give application access to the subscription, in Azure Subscription console, add a role assignment of role `Purview Data Curator` and `Purview Data Source Administrator` to the Service Principal App registration created earlier as shown below:-

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

* With Application registration created, search for `Subscription` and select `Subscriptions` from the drop-down list:-

<p align="center">
  <img src="./images/purview-02-01.png">
</p>

* Select the Azure Subscription and click `Access control (IAM)`:-

<p align="center">
  <img src="./images/purview-02-02.png">
</p>

* To attach the required roles to Application registration, choose `Add` and select `Add role assignment` from the drop down list:-

<p align="center">
  <img src="./images/purview-02-03.png">
</p>

* To add role assignment, search for `Purview Data Curator` role and under `Select` search for the service principal created earlier as shown below. Click on the application registration to select it:-

<p align="center">
  <img src="./images/purview-02-04.png">
</p>

* Make sure the application registration shows under `Selected members`. Click `Save` to add role:-

<p align="center">
  <img src="./images/purview-02-05.png">
</p>

* Repeat the same procedure to add `Purview Data Source Administrator` role to the Application registration as well.

* The complete procedure of adding the required roles to the Application Registration is shown below:-

<p align="center">
  <img src="./images/purview-02.gif">
</p>

## Deploy and Configure Purview Account

* The following values are needed when using the OneClick Deployment ARM template:-
    * **Resource Group:** Name of the resource group. All the resources created by the template will be in this resource group.
    * **Region:** Azure region in which to create the resource group.
    * **Data Lake Account Name:** Name of the ADSL2 storage account. This storage account will be created and registered as a data source with Purview account. The name you provide will be appended with a unique sting to make it globally available. The field can contain only lowercase letters and numbers. Name must be between 1 and 11 characters.
    * **Storage Account Name:** Name of the Blob storage account. This storage account will be created and registered as a data source with Purview account. The name you provide will be appended with a unique sting to make it globally available. The field can contain only lowercase letters and numbers. Name must be between 1 and 11 characters.
    * **New or existing Purview Account:** Whether to create a new Purview account or use an existing one.
    * **Purview Account Name:** Name of the Azure Purview Account. In case of new Purview account, give a new name. To use an existing one, enter the name of existing purview account. For the new Purview account, the name you provide will be appended with a unique sting to make it globally available. The Purview account name can contain only letters, numbers and hyphens. The first and last characters must be a letter or number. The hyphen(-) character must be immediately preceded and followed by a letter or number. Spaces are not allowed.
    * **Purview Resource Group:** Povide the name of the resource group for the existing Purview account. Leave as it is if you are creating a new Purview account or if the existing Purview account is present in the same resource group where all other resources are going to be deployed. This resource group is required to get the Purview account path to assign required RBAC role(s) to the new or existing Purview account.
    * **Key Vault Name:** Name of the Azure Key vault. This is used to store Client Secret needed to perform API calls and also information on resources such as data sources and glossary terms, so they can be purged afterwards when delete function is triggered. The name you provide will be appended with a unique sting to make it globally available. A vault's name must be between 1-11 alphanumeric characters. The name must begin with a letter, end with a letter or digit, and not contain consecutive hyphens.
    * **Factory Name:** Name for the Azure Data Factory. This Data Factory will be created and load the data into the storage accounts with sample NYC Taxi data. The name you provide will be appended with a unique sting to make it globally available. The name can contain only letters, numbers and hyphens. The first and last characters must be a letter or number. Spaces are not allowed.
    * **Aad App Client Id:** Client ID of the Application Registration created in prerequisite.
    * **Aad App Client Secret:** Client Secret of the Application Registration created in prerequisite.
    * **Location:** Location where the resources will be deployed. It is, by default, set to the region of the resource group. You can upadate it to any other region. Note that all the resources will be deployed to the same location/region, that is why while chhosing the loaction, make sure all the resources/services are available in that location/region.

* Click the following button to deploy the Purview QuickStart:-

     [![Deploy To Azure](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/1-CONTRIBUTION-GUIDE/images/deploytoazure.svg?sanitize=true)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fosamaemumba%2Fpurview-poc%2Fmain%2Fazuredeploy.json)

* Currently it takes ~25-30 minutes for one complete deployment. 

## Removing the QuickStart resources from the Purview Account
* After the deployment is complete, the resources can be deleted by running the `deletePurviewFunctionTriggerUrl` URL from the deployment outputs as shown below:-

<p align="center">
  <img src="./images/purview-03-update.gif">
</p>


## Troubleshooting

This portion lists solutions to problems one might encounter with Purview OneClick Deployment.

### Common Problems

Here is a list of common problems one might encounter while deploying the template:-

#### 1. Purview Account created but `no` QuickStart resources were found:

In case the deployment is successful and there aren't any resources created. It can be verified by going to the Purview Account Portal. That is caused by the time Purview Account needs to set up properly. Resources can be provisioned using the `configurePurviewFunctionTriggerUrl` url from the deployment outputs. The same url needs to be used to trigger the creation function after some time, in case it does not work in first attempt. It should output `AzurePurview Creation function triggered successfully`. The following gif illustrates the creation of resources:-

<p align="center">
  <img src="./images/purview-04.gif">
</p>

#### 2. Failed Deployment

* Deployment failed on `runDataFactory` step.
* Deployment failed on `triggerConfigurePurviewFunction` step.

#### 3. Successful Deployment

* Deployment is successful but resources are not created.

For the failed deployments, simply deploying the ARM template again solves the problem.

### Solution

#### 2. Failed Deployment

In case the deployment failed, this can be caused by the Powershell modules used in the deployment script.
Make sure that:-

* The App Registration client ID and client secret are correct.
* The application service principal has access to the `Purview Data Curator` and `Purview Data Source Administrator` roles at subscription level.

If the above requirements are satisfied, rerunning the deployment should resolve this issue.

#### 3. Successful Deployment without resources
In case the deployment was successful, but no resources were provisioend. This can be caused by the time Azure Purview Accounts need to set up and to have APIs respond properly.
The following explains the working of deployment script in this case:-

  * Unresponsive APIs return a status code of 500.
  * The deployment script waits for 1 minute and retries to get a status code of 200.
  * In case of success status code return, the script creates the resources.
  * In case of status code 500 return, the script does 15 retries, with 1 minute wait each time.
  * If APIs are still unresponsive, the deployment script completes the deployment without provisioning resources.
