![Microsoft Cloud Workshops](https://github.com/Microsoft/MCW-Template-Cloud-Workshop/raw/master/Media/ms-cloud-workshop.png 'Microsoft Cloud Workshops')

<div class="MCWHeader1">
Serverless architecture
</div>

<div class="MCWHeader2">
Hands-on lab step-by-step
</div>

<div class="MCWHeader3">
January 2021
</div>

Information in this document, including URL and other Internet Web site references, is subject to change without notice. Unless otherwise noted, the example companies, organizations, products, domain names, e-mail addresses, logos, people, places, and events depicted herein are fictitious, and no association with any real company, organization, product, domain name, e-mail address, logo, person, place or event is intended or should be inferred. Complying with all applicable copyright laws is the responsibility of the user. Without limiting the rights under copyright, no part of this document may be reproduced, stored in or introduced into a retrieval system, or transmitted in any form or by any means (electronic, mechanical, photocopying, recording, or otherwise), or for any purpose, without the express written permission of Microsoft Corporation.

Microsoft may have patents, patent applications, trademarks, copyrights, or other intellectual property rights covering subject matter in this document. Except as expressly provided in any written license agreement from Microsoft, the furnishing of this document does not give you any license to these patents, trademarks, copyrights, or other intellectual property.

The names of manufacturers, products, or URLs are provided for informational purposes only and Microsoft makes no representations and warranties, either expressed, implied, or statutory, regarding these manufacturers or the use of the products with any Microsoft technologies. The inclusion of a manufacturer or product does not imply endorsement of Microsoft of the manufacturer or product. Links may be provided to third party sites. Such sites are not under the control of Microsoft and Microsoft is not responsible for the contents of any linked site or any link contained in a linked site, or any changes or updates to such sites. Microsoft is not responsible for webcasting or any other form of transmission received from any linked site. Microsoft is providing these links to you only as a convenience, and the inclusion of any link does not imply endorsement of Microsoft of the site or the products contained therein.

© 2020 Microsoft Corporation. All rights reserved.

Microsoft and the trademarks listed at <https://www.microsoft.com/legal/intellectualproperty/Trademarks/Usage/General.aspx> are trademarks of the Microsoft group of companies. All other trademarks are property of their respective owners.

**Contents**

<!-- TOC -->

- [Serverless architecture hands-on lab step-by-step](#serverless-architecture-hands-on-lab-step-by-step)
  - [Abstract and learning objectives](#abstract-and-learning-objectives)
  - [Overview](#overview)
  - [Solution architecture](#solution-architecture)
  - [Requirements](#requirements)
  - [Exercise 1: Azure data, storage, and serverless environment setup](#exercise-1-azure-data-storage-and-serverless-environment-setup)
    - [Help references](#help-references)
    - [Task 1: Provision the storage account](#task-1-provision-the-storage-account)
    - [Task 2: Provision the Function Apps](#task-2-provision-the-function-apps)
    - [Task 3: Provision the Event Grid topic](#task-3-provision-the-event-grid-topic)
    - [Task 4: Provision the Azure Cosmos DB account](#task-4-provision-the-azure-cosmos-db-account)
    - [Task 5: Provision the Computer Vision API service](#task-5-provision-the-computer-vision-api-service)
    - [Task 6: Provision Azure Key Vault](#task-6-provision-azure-key-vault)
    - [Task 7: Retrieve the URI for each secret](#task-7-retrieve-the-uri-for-each-secret)
  - [Exercise 2: Develop and publish the photo processing and data export functions](#exercise-2-develop-and-publish-the-photo-processing-and-data-export-functions)
    - [Help references](#help-references-1)
    - [Task 1: Create a system-assigned managed identity for your Function App to connect to Key Vault](#task-1-create-a-system-assigned-managed-identity-for-your-function-app-to-connect-to-key-vault)
    - [Task 2: Configure application settings](#task-2-configure-application-settings)
    - [Task 3: Add Function App to Key Vault access policy](#task-3-add-function-app-to-key-vault-access-policy)
    - [Task 4: Finish the ProcessImage function](#task-4-finish-the-processimage-function)
    - [Task 5: Deploy the Function App using GitHub Actions](#task-5-deploy-the-function-app-using-github-actions)
  - [Exercise 3: Create functions in the portal](#exercise-3-create-functions-in-the-portal)
    - [Help references](#help-references-2)
    - [Task 1: Create function to save license plate data to Azure Cosmos DB](#task-1-create-function-to-save-license-plate-data-to-azure-cosmos-db)
    - [Task 2: Add an Event Grid subscription to the SavePlateData function](#task-2-add-an-event-grid-subscription-to-the-saveplatedata-function)
    - [Task 3: Add an Azure Cosmos DB output to the SavePlateData function](#task-3-add-an-azure-cosmos-db-output-to-the-saveplatedata-function)
    - [Task 4: Create function to save manual verification info to Azure Cosmos DB](#task-4-create-function-to-save-manual-verification-info-to-azure-cosmos-db)
    - [Task 5: Add an Event Grid subscription to the QueuePlateForManualCheckup function](#task-5-add-an-event-grid-subscription-to-the-queueplateformanualcheckup-function)
    - [Task 6: Add an Azure Cosmos DB output to the QueuePlateForManualCheckup function](#task-6-add-an-azure-cosmos-db-output-to-the-queueplateformanualcheckup-function)
  - [Exercise 4: Monitor your functions with Application Insights](#exercise-4-monitor-your-functions-with-application-insights)
    - [Help references](#help-references-3)
    - [Task 1: Use the Live Metrics Stream to monitor functions in real time](#task-1-use-the-live-metrics-stream-to-monitor-functions-in-real-time)
    - [Task 2: Observe your functions dynamically scaling when resource-constrained](#task-2-observe-your-functions-dynamically-scaling-when-resource-constrained)
  - [Exercise 5: Explore your data in Azure Cosmos DB](#exercise-5-explore-your-data-in-azure-cosmos-db)
    - [Help references](#help-references-4)
    - [Task 1: Use the Azure Cosmos DB Data Explorer](#task-1-use-the-azure-cosmos-db-data-explorer)
  - [Exercise 6: Create the data export Logic App](#exercise-6-create-the-data-export-logic-app)
    - [Help references](#help-references-5)
    - [Task 1: Create the Logic App](#task-1-create-the-logic-app)
  - [Exercise 7: Complete ExportLicensePlates Function App](#exercise-7-complete-exportlicenseplates-function-app)
    - [Help references](#help-references-6)
    - [Task 1: Finish your ExportLicensePlates Function code and push changes to GitHub to trigger deployment](#task-1-finish-your-exportlicenseplates-function-code-and-push-changes-to-github-to-trigger-deployment)
  - [Exercise 8: Rerun the Logic App and verify data export](#exercise-8-rerun-the-logic-app-and-verify-data-export)
    - [Task 1: Run the Logic App](#task-1-run-the-logic-app)
    - [Task 2: View the exported CSV file](#task-2-view-the-exported-csv-file)
  - [After the hands-on lab](#after-the-hands-on-lab)
    - [Task 1: Delete the resource group in which you placed your Azure resources](#task-1-delete-the-resource-group-in-which-you-placed-your-azure-resources)
    - [Task 2: Delete the GitHub repo](#task-2-delete-the-github-repo)

<!-- /TOC -->

# Serverless architecture hands-on lab step-by-step

## Abstract and learning objectives

In this hand-on lab, you will be challenged to implement an end-to-end scenario using a supplied sample that is based on Microsoft Azure Functions, Azure Cosmos DB, Event Grid, and related services. The scenario will include implementing compute, storage, workflows, and monitoring, using various components of Microsoft Azure. The hands-on lab can be implemented on your own, but it is highly recommended to pair up with other members at the lab to model a real-world experience and to allow each member to share their expertise for the overall solution.

At the end of the hands-on-lab, you will have confidence in designing, developing, and monitoring a serverless solution that is resilient, scalable, and cost-effective.

## Overview

Contoso Ltd. is rapidly expanding their toll booth management business to operate in a much larger area. As this is not their primary business, which is online payment services, they are struggling with scaling up to meet the upcoming demand to extract license plate information from a large number of new tollbooths, using photos of vehicles uploaded to cloud storage. Currently, they have a manual process where they send batches of photos to a 3rd-party who manually transcodes the license plates to CSV files that they send back to Contoso to upload to their online processing system. They want to automate this process in a way that is cost effective and scalable. They believe serverless is the best route for them, but do not have the expertise to build the solution.

## Solution architecture

Below is a diagram of the solution architecture you will build in this lab. Please study this carefully, so you understand the whole of the solution as you are working on the various components.

![The Solution diagram is described in the text following this diagram.](../Whiteboard%20design%20session/media/preferred-solution.png 'Solution diagram')

The solution begins with vehicle photos being uploaded to an Azure Storage blobs container, as they are captured. An Event Grid subscription is created against the Blob storage create event, calling the photo processing **Azure Function** endpoint (on the side of the diagram), which in turn sends the photo to the **Cognitive Services Computer Vision API OCR** service to extract the license plate data. If processing was successful and the license plate number was returned, the function submits a new Event Grid event, along with the data, to an Event Grid topic with an event type called "savePlateData". However, if the processing was unsuccessful, the function submits an Event Grid event to the topic with an event type called "queuePlateForManualCheckup". Two separate functions are configured to trigger when new events are added to the Event Grid topic, each filtering on a specific event type, both saving the relevant data to the appropriate **Azure Cosmos DB** collection for the outcome, using the Cosmos DB output binding. A **Logic App** that runs on a 15-minute interval executes an Azure Function via its HTTP trigger, which is responsible for obtaining new license plate data from Cosmos DB and exporting it to a new CSV file saved to Blob storage. If no new license plate records are found to export, the Logic App sends an email notification to the Customer Service department via their Office 365 subscription. **Application Insights** is used to monitor all of the Azure Functions in real-time as data is being processed through the serverless architecture. This real-time monitoring allows you to observe dynamic scaling first-hand and configure alerts when certain events take place. **Azure Key Vault** is used to securely store secrets, such as connection strings and access keys. Key Vault is accessed by the Function Apps through an access policy within Key Vault, assigned to each Function App's system-assigned managed identity.

## Requirements

- Microsoft Azure subscription (non-Microsoft subscription). You can create a free account at https://azure.com/free.
- Local machine or an Azure virtual machine configured with:
  - Git (https://git-scm.com/) - accept all defaults when installing
  - Visual Studio Code (https://code.visualstudio.com/download). Install these extensions ([instructions for extensions](https://code.visualstudio.com/docs/editor/extension-gallery)):
    - Azure Functions
    - C#
  - .NET Core 3.1 (https://dotnet.microsoft.com/download/dotnet-core/3.1) SDK
  - .NET 5 (https://dotnet.microsoft.com/download/dotnet/5.0) SDK
  - Modern web browser such as Microsoft Edge (https://www.microsoft.com/edge) - make sure to set it as your default (edge://settings/defaultBrowser)
- GitHub account. You can create a free account at https://github.com/join.
    - Forked and Cloned a copy of this repository to your lab machine.

## Exercise 1: Azure data, storage, and serverless environment setup

**Duration**: 30 minutes

You must provision a few resources in Azure before you start developing the solution. Ensure all resources use the same resource group for easier cleanup.

In this exercise, you will provision an Azure Blob Storage Account using the Hot tier, and create two Containers (think of them like Folders) within to store uploaded photos and exported CSV files. You will then provision two Function Apps instances, one you will deploy via GitHub Actions, and the other you will manage using the Azure portal. Next, you will create a new Event Grid topic. After that, you will create an Azure Cosmos DB Account with two Collections. Finally, you will provision a new Cognitive Services Computer Vision API service for applying object character recognition (OCR) on the license plates.

### Help references

|                                            |                                                                                           |
| ------------------------------------------ | :---------------------------------------------------------------------------------------: |
| **Description**                            | **Links**                                                                                 |
| Creating a Storage Account (blob hot tier) | https://docs.microsoft.com/azure/storage/common/storage-create-storage-account            |
| Creating a Function app                    | https://docs.microsoft.com/azure/azure-functions/functions-create-function-app-portal     |
| Concepts in Event Grid                     | https://docs.microsoft.com/azure/event-grid/concepts                                      |
| Creating an Azure Cosmos DB account        | https://docs.microsoft.com/azure/cosmos-db/manage-account                                 |

### Task 1: Provision the storage account

1. Open the [Azure Portal Create Storage Account Blade](https://portal.azure.com/#create/Microsoft.StorageAccount).

2. On the **Create storage account** blade, specify the following configuration options:

    a. For **Resource group**, select the **Use existing** radio button, and select the **ServerlessArchitecture** resource group.

    b. **Name**: enter a unique value for the storage account such as **tollboothstorage** (must be all lower case; ensure the green check mark appears).

    c. Ensure the **Location** selected is the same region as the resource group.

    d. For performance, ensure **Standard** is selected.

    e. For account kind, select **StorageV2 (general purpose v2)**.

    f. For replication, select **Locally-redundant storage (LRS)**.

    ![Fields in the Create storage account blade are set to the previously defined values.](media/new-storage-account-form.png 'Create storage account blade')

3. Select **Review + create**, then select **Create**.

4. After the storage account has completed provisioning, open the storage account by selecting **Go to resource**.

    ![In the Azure Portal, once the storage account has completed provisioning a status message is displayed saying Your deployment is complete. Beneath the next steps section The Go to resource button is highlighted.](media/storage-go-to-resource.png "Go to resource")

5. On the **Storage account** blade, select **Access Keys**, under Settings in the menu. Then on the **Access keys** blade, select the **Click to copy** button for **key1 connection string.**

    ![In the Storage account blade, under Settings, Access keys is selected. Under Default keys, the copy button next to the key1 connection string is selected.](media/image15.png 'Storage account blade')

6. Paste the value into a text editor, such as Notepad, for later reference.

7. Select **Containers** under **Blob Service** in the left menu. Then select the **+ Container** button to add a new container. In the **Name** field, enter **images**, select **Private (no anonymous access)** for the public access level, then select **Create** to create the container.

    ![In the Storage blade, under Settings, Containers is selected. In the Containers blade, the + (add icon) Container button is selected. Below, the Name field displays images, and the Public access level is set to Private (no anonymous access).](media/storage-new-container-images.png 'Storage and Containers blade')

8. Repeat these steps to create a container named **export**.

    ![In the Storage blade, under Settings, Containers is selected. In the Containers blade, the + (add icon) Container button is selected. Below, the Name field displays export, and the Public access level is set to Private (no anonymous access).](media/new-container-export.png 'Storage and Containers blade')

9. Select **Queues** under **Queue service** in the left menu. Then select the **+ Queue** button to add a new queue. In the **Queue name** field, enter **failedexports**, then select **OK** to create.

    ![In the Storage blade, under Settings, Containers is selected. In the Containers blade, the + (add icon) Container button is selected. Below, the Name field displays export, and the Public access level is set to Private (no anonymous access).](media/setup-queue.png 'Storage and Containers blade')

### Task 2: Provision the Function Apps

1. Open the [Azure Portal Create Function App Blade](https://portal.azure.com/#create/Microsoft.FunctionApp).

2. Within the **Create Function App** *Basics* blade, specify the following configuration options:

    a. **Subscription**: Select your Azure subscription for this lab.

    b. **Resource Group**: Select **ServerlessArchitecture**.

    c. **Name**: Unique value for the App name (ensure the green check mark appears). Provide a name similar to **TollBoothFunctionApp**.

    d. **Publish**: Select **Code**.

    e. **Runtime stack**: Select **.NET**.

    f. **Version**: Select **3.1**.

    g. **Region**: Select the region you are using for this lab, or the closest available one.

    ![In the basics tab of the Create Function App blade, the form fields are set to the previously defined values.](media/new-functionapp-net-basics.png 'Function App Basics blade')

3. Select **Next: Hosting >**.

4. Within the **Hosting** blade, specify the following configuration options:

    a. **Storage account**: Leave this option as **create new**.

    b. **Operating system**: Select **Windows**.

    c. **Plan type**: Select **Consumption (Serverless)**.

    ![In the Hosting tab of the Create Function App blade, the form fields are set to the previously defined values.](media/new-functionapp-net-hosting.png "Function App Hosting blade")

5. Select **Next: Monitoring >**.

    a. **Enable Application Insights**: Select **Yes**.

    b. Leave other options 'as is'.

6. Select **Review + create**, then select **Create** to provision the new Function App.

    ![The Review + create button is highlighted.](media/new-functionapp-net-monitoring-review.png "Function App Monitoring")

7. **Repeat steps 1-3** to create a second Function App.

8. Within the **Create Function App** blade *Basics* tab, specify the following configuration options:

    a. **Subscription**: Select your Azure subscription for this lab.

    b. **Resource Group**: Select **ServerlessArchitecture**.

    c. **Name**: Unique value for the App name (ensure the green check mark appears). Provide a name similar to **TollBoothEvents**.

    d. **Publish**: Select **Code**.

    e. **Runtime stack**: Select **Node.js**.

    f. **Version**: Select **12 LTS**.

    g. **Region**: Select the region you are using for this lab, or the closest available one.

    ![Fields in the Create Function App blade Basics tab are set to the previously defined values.](media/new-functionapp-nodejs-basics.png 'Function App Basics blade')

10. Select **Next: Hosting >**.

11. Within the **Hosting** blade, specify the following configuration options:

    a. **Storage account**: Leave this option as **create new**.

    b. **Operating system**: Select **Windows**.

    c. **Plan type**: Select **Consumption**.

    ![Fields in the Create Function App blade Hosting tab are set to the previously defined values.](media/new-functionapp-net-hosting.png "Function App Hosting blade")

12. Select **Next: Monitoring >**.

    a. **Enable Application Insights**: Select **Yes**.

    b. **Application Insights**: Select the Application Insights instance you created earlier when provisioning the .NET Function App (ex. **TollboothMonitor**).

    ![Fields in the Create Function App blade Monitoring tab are set to the previously defined values.](media/new-functionapp-nodejs-monitoring.png "Function App Monitoring blade")

13. Select **Review + create**, then select **Create** to provision the new Function App.

### Task 3: Provision the Event Grid Topic

1. Open the [Azure Portal Create Event Grid Topic Blade](https://portal.azure.com/#create/Microsoft.EventGridTopic).

2. On the **Create Topic** blade, specify the following configuration options:

    a. **Name:** Unique value for the Topic name such as **TollBoothTopic** (ensure the green check mark appears).

    b. Select the Resource Group **ServerlessArchitecture**.

    c. Ensure the **Location** selected is set to the same region as your Resource Group.

    ![In the Create Topic blade, the Name field is set to TollBoothTopic, and the Resource Group selected is ServerlessArchitecture.](media/new-eventgrid-topic.png 'Create Topic blade')

3. Select **Next: Advanced >**.

4. Make sure **Event Grid Schema** is selected as the event schema.

    ![Select the event grid scema.](media/new-eventgrid-topic-advanced.png "Create Topic - Advanced")

5. Select **Review + Create**, then select **Create** in the screen that follows.

6. After the Event Grid Topic has completed provisioning, open the account by opening the **ServerlessArchitecture** resource group, and then selecting the **Event Grid** topic name.

7. Select **Overview** in the menu, and then copy the **Topic Endpoint** value.

    ![In the TollBoothTopic blade, Overview is selected, and the copy button next to the Topic Endpoint is called out.](media/image21.png 'TollBoothTopic blade')

8. Select **Access Keys** under Settings in the menu.

9. Within the **Access Keys** blade, copy the **Key 1** value.

    ![In the TollBoothTopic blade, in the left menu under Settings, Access keys is selected. In the listing of Access keys, the copy button next to the Key 1 access key is selected.](media/image22.png 'TollBoothTopic - Access keys blade')

10. Paste the values into a text editor, such as Notepad, for later reference.

### Task 4: Provision the Azure Cosmos DB account

1. Open the [Azure Portal Create Cosmos DB Blade](https://portal.azure.com/#create/Microsoft.DocumentDB).

2. On the **Create Azure Cosmos DB Account** blade, specify the following configuration options:

    a. Specify the Resource Group **ServerlessArchitecture**.

    b. For Account Name, type a unique value such as **tollboothdbABC** (ensure the green check mark appears).

    c. Select the **Core (SQL)** API.

    d. Select the same **Location** as your Resource Group if available. Otherwise, select the next closest **region**.

    e. Ensure **Notebooks** is disabled.

    e. Set **Capacity mode** to **Provisioned throughput**.

    f. Ensure **Apply Free Tier Discount** is set to **Do Not Apply**.

    g. Select **Production** for the Account Type.

    h. Ensure **Geo-Redundancy** is disabled.

    i. Ensure **Multi-region writes** is disabled.

    j. Ensure **Availability Zones** is disabled.

    ![Fields in the Azure Cosmos DB blade are set to the previously defined settings.](media/new-cosmosdb.png 'Azure Cosmos DB blade')

3. Select **Review + create**, then select **Create**.

4. After the Azure Cosmos DB account has completed provisioning, open the account by opening the **ServerlessArchitecture** resource group, and then selecting the **Azure Cosmos DB** account name.

5. Select **Data Explorer** in the left-hand menu, then select **New Container**.

    ![In the Data Explorer blade, the Data Explorer item is selected in the left menu. The New Container button is selected in the Data Explorer pane.](media/data-explorer-new-container.png 'Data Explorer blade')

6. On the **Add Container** blade, specify the following configuration options:

    a. Enter **LicensePlates** for the **Database id**.

    b. Leave **Provision database throughput** unchecked.

    c. Enter **Processed** for the **Container id**.

    d. Partition key: **/licensePlateText**

    e. Throughput: **5000**

    ![In the Add Container blade, fields are set to the previously defined values.](media/cosmosdb-add-processed-collection.png 'Add Container blade')

7. Select **OK**.

8. Select **New Container** to add another container.

9. On the **Add Container** blade, specify the following configuration options:

    a. For Database id, choose **Use existing** and select **LicensePlates**.

    b. Enter **NeedsManualReview** for the **Container id**.

    c. Partition key: **/fileName**

    d. Throughput: **5000**

    ![In the Add Container blade, fields are set to the previously defined values.](media/cosmosdb-add-manualreview-collection.png 'Add Collection blade')

10. Select **OK**.

11. Select **Firewall and virtual networks** in the left-hand menu.

12. Select **+ Add my current IP** to add your IP address to the IP list under Firewall. Next, check the box next to **Accept connections from within public Azure datacenters**. This will enable Azure services, such as your Function Apps to access your Azure Cosmos DB account.

    ![The checkbox is highlighted.](media/cosmos-db-firewall.png "Firewall and virtual networks")

13. Select **Save**.

14. Select **Keys** under Settings in the left-hand menu.

15. Underneath the **Read-write Keys** tab within the Keys blade, copy the **URI** and **Primary Key** values.

    ![In the tollbooth - Keys blade, under Settings, Keys is selected. On the Read-write Keys tab, the copy buttons for the URI and Primary Key fields are selected.](media/image28.png 'tollbooth - Keys blade')

16. Paste the values into a text editor, such as Notepad, for later reference.

### Task 5: Provision the Computer Vision API service

1. Open the [Azure Portal Create Computer Vision Blade](https://portal.azure.com/#create/Microsoft.CognitiveServicesComputerVision).

2. On the **Create Computer Vision API** blade, specify the following configuration options:

    a. **Name**: Unique value for the Computer Vision instance name such as **tollboothvisionABC** (ensure the green check mark appears).

    b. Ensure the **Location** selected is the same region as your Resource Group.

    c. For pricing tier, select **S1 (10 Calls per second)**.

    d. Specify the Resource Group **ServerlessArchitecture**.

    ![In the Create Computer Vision blade, fields are set to the previously defined values.](media/create-computer-vision.png 'Create blade')

3. Select **Review + create** then **Create**.

4. After the Computer Vision API has completed provisioning, open the service by opening the **ServerlessArchitecture** resource group, and then selecting the **Computer Vision** **API** service name.

5. Under Resource Management in the left-hand menu, select **Keys and Endpoint**.

6. Within the **Keys and Endpoint** blade, copy the **ENDPOINT** value and **KEY 1** value.

    ![In the Cognitive Services blade, under Resource Management, Keys and Endpoint is selected. The Copy button next to the Endpoint and Key 1 values are selected.](media/copy-computer-vision-key.png 'Keys and Endpoint information')

7. Paste the values into a text editor, such as Notepad, for later reference.

### Task 6: Provision Azure Key Vault

Azure Key Vault is used to securely store all secrets, such as database connection strings and keys.

1. Open the [Azure Portal Create Key Vault Blade](https://portal.azure.com/#create/Microsoft.KeyVault).

2. On the **Create key vault** blade, specify the following configuration options:

    a. **Subscription**: Select your Azure subscription used for this lab.

    b. **Resource group**: Select **ServerlessArchitecture**.

    c. **Key vault name**: Unique value for the name such as **TollBoothVaultABC** (ensure the green check mark appears).

    d. **Region**: Select the same region as your Resource Group.

    e. **Pricing tier**: Select **Standard**.

    f. **Soft delete**: Select **Enable**.

    g. **Retention period (days)**: Leave at 90.

    h. **Purge protection**: Select **Disable**.

    ![In the Create key vault blade, fields are set to the previously defined values.](media/create-key-vault.png 'Create blade')

3. Select **Review + create**, then select **Create**.

4. After the deployment completes, select **Go to resource**.

    ![When the deployment completes, a message is displayed indicating Your deployment is complete. The Go to resource button is highlighted in the next steps section.](media/key-vault-deployment-complete.png "Your deployment is complete")

5. Select **Secrets** under Settings in the left-hand menu.

6. Select **Generate/Import** to add a new key.

    ![The Secrets menu item and the Generate/Import button are highlighted.](media/generate-secret.png "Key Vault - Secrets")

7. Use the table below for the Name / Value pairs to use when creating the secrets. You only need to populate the **Name** and **Value** fields for each secret, and can leave the other fields at their default values.

    |                          |                                                                                                                                                             |
    | ------------------------ | :---------------------------------------------------------------------------------------------------------------------------------------------------------: |
    | **Name**      |                                                                          **Value**                                                                          |
    | computerVisionApiKey     |                                                                   Computer Vision API key                                                                   |
    | eventGridTopicKey        |                                                                 Event Grid Topic access key                                                                 |
    | cosmosDBAuthorizationKey |                                                                    Cosmos DB Primary Key                                                                    |
    | blobStorageConnection    |                                                               Blob storage connection string                                                                |

    When you are finished creating the secrets, your list should look similar to the following:

    ![The listing of secrets is displayed matching the previously defined values.](media/key-vault-keys.png "Key Vault Secrets")

### Task 7: Retrieve the URI for each secret

When you set the App Settings for the Function App in the next exercise, you will need to reference the URI of a secret in Key Vault, including the version number. To do this, perform the following steps for each secret and **copy the values** to Notepad or similar text application.

1. Open your Key Vault instance in the portal.

2. Select **Secrets** under Settings in the left-hand menu.

3. Select the secret whose URI value you wish to obtain.

4. Select the **Current Version** of the secret.

    ![The secret's current version is selected.](media/key-vault-secret-current-version.png "Current Version")

5. Copy the **Secret Identifier**.

    ![The Secret Identifier field is highlighted. Next to this field is a copy button.](media/key-vault-secret-identifier.png "Secret Identifier")

    When you add the Key Vault reference to this secret within a Function App's App Settings, you will use the following format: `@Microsoft.KeyVault(SecretUri={referenceString})`, where `{referenceString}` is replaced by the Secret Identifier (URI) value above. **Be sure to remove the curly braces (`{}`)**.

    For example, a complete reference would look like the following:

    `@Microsoft.KeyVault(SecretUri=https://tollboothvault.vault.azure.net/secrets/blobStorageConnection/d6ea0e39236348539dc33565e031afc3)`

When you are done creating the values, you should have a list similar to the following:

```text
@Microsoft.KeyVault(SecretUri=https://tollboothvault.vault.azure.net/secrets/blobStorageConnection/771aa40adac64af0b2aefbd741bd46ef)
@Microsoft.KeyVault(SecretUri=https://tollboothvault.vault.azure.net/secrets/computerVisionApiKey/ce228a43f40140dd8a9ffb9a25d042ee)
@Microsoft.KeyVault(SecretUri=https://tollboothvault.vault.azure.net/secrets/cosmosDBAuthorizationKey/1f9a0d16ad22409b85970b3c794a218c)
@Microsoft.KeyVault(SecretUri=https://tollboothvault.vault.azure.net/secrets/eventGridTopicKey/e310bcd71a72489f89b6112234fed815)
```

## Exercise 2: Develop and publish the photo processing and data export Functions

**Duration**: 45 minutes

Use Visual Studio Code with the Azure Functions tooling to develop and debug the Functions locally, and then publish them to Azure using GitHub Actions. 

The starter project, TollBooth, contains most of the code needed. You will add in the missing code before deploying to Azure.

### Help references

|                                       |                                                                        |
| ------------------------------------- | :--------------------------------------------------------------------: |
| **Description**                       |                               **Links**                                |
| Code and test Azure Functions locally | https://docs.microsoft.com/azure/azure-functions/functions-run-local |
| What are managed identities for Azure resources? | https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/overview |

### Task 1: Create a system-assigned managed identity for your Function App to connect to Key Vault

In order for your Function App to access Key Vault and read secrets, you must [create a system-assigned managed identity](https://docs.microsoft.com/azure/app-service/overview-managed-identity#adding-a-system-assigned-identity) for the Function App, and [create an access policy in Key Vault](https://docs.microsoft.com/azure/key-vault/key-vault-secure-your-key-vault#key-vault-access-policies) for the application identity.

1. Open the **ServerlessArchitecture** resource group, and then select the Azure Function App you created whose name ends with **FunctionApp**. This is the one you created using the **.NET** runtime stack. If you did not use this naming convention, that's fine. Just be sure to make note of the name so you can distinguish it from the Function App you will be developing using the portal later on.

    ![In the ServerlessArchitecture resource group, the TollBoothFunctionApp is selected.](media/image33.png 'ServerlessArchitecture resource group')

2. Select **Identity** in the left-hand menu. Within the **System assigned** tab, switch **Status** to **On**. Select **Save**.

    ![In the Identity blade, the System assigned tab is selected with the Status set to On and the Save button is highlighted.](media/function-app-identity.png "Identity")

### Task 2: Configure application settings

In this task, you will apply application settings using the Microsoft Azure Portal.

1. Select **Configuration** in the left-hand menu.

    ![In the TollBoothFunctionApp blade on the Overview tab, under Configured features, the Configuration item is selected.](media/image34.png 'TollBoothFunctionApp blade')

2. Scroll to the **Application settings** section. Use the **+ New application setting** link to create the following additional Key/Value pairs (the key names must exactly match those found in the table below). **Be sure to remove the curly braces (`{}`)**.

| **Application Key** | **Value** |
| ------------------------ | :----------------------------------------------------------------------------------------------------: |
| computerVisionApiUrl     | Computer Vision API endpoint you copied earlier. Append **vision/v2.0/ocr** to the end. Example: `https://<YOUR-SERVICE-NAME>.cognitiveservices.azure.com/vision/v2.0/ocr` |
| computerVisionApiKey     | Enter `@Microsoft.KeyVault(SecretUri={referenceString})`, where `{referenceString}` is the URI for the **computerVisionApiKey** Key Vault secret                                                                   |
| eventGridTopicEndpoint   | Event Grid Topic endpoint                                                                  |
| eventGridTopicKey        | Enter `@Microsoft.KeyVault(SecretUri={referenceString})`, where `{referenceString}` is the URI for the **eventGridTopicKey** Key Vault secret                                                                 |
| cosmosDBEndPointUrl      | Cosmos DB URI                                                                        |
| cosmosDBAuthorizationKey | Enter `@Microsoft.KeyVault(SecretUri={referenceString})`, where `{referenceString}` is the URI for the **cosmosDBAuthorizationKey** Key Vault secret                                                                    |
| cosmosDBDatabaseId       | Cosmos DB database id (LicensePlates)                                                            |
| cosmosDBCollectionId     | Cosmos DB processed collection id (Processed)                                                        |
| exportCsvContainerName   | Blob storage CSV export container name (export)                                                       |
| blobStorageConnection    | Enter `@Microsoft.KeyVault(SecretUri={referenceString})`, where `{referenceString}` is the URI for the **blobStorageConnection** Key Vault secret                                                                |

> **NOTE:** if you would like to quickly add these settings you can modify the [function-app-setting.json](function-app-setting.json) file and then copy/paste it using the **Advanced edit** to add the new settings. Make sure you don't overwrite the existing settings when doing this. Make sure to paste inside of the JSON array in the editor in the Azure Portal.

![In the Application Settings section, the previously defined key / value pairs are displayed.](media/application-settings.png 'Application Settings section')

3. Select **Save**.

    ![Screenshot of the Save icon.](media/image36.png 'Save icon')

### Task 3: Add Function App to Key Vault access policy

Perform these steps to create an access policy that enables the "Get" secret permission:

1. Open your Key Vault in the Azure Portal.

2. Select **Access policies**.

3. Select **+ Add Access Policy**.

    ![In the Key vault Access Policies blade, The Add Access Policy link is highlighted.](media/key-vault-add-access-policy.png "Access policies")

4. Select the **Select principal** section on the Add access policy form.

    ![In the Add access policy form, the Select principal field is highlighted.](media/key-vault-add-access-policy-select-principal.png "Add access policy")

5. In the Principal blade, search for your TollBoothFunctionApp Function App's service principal, select it, then select the **Select** button.

    ![In the Select a principal form, TollBooth is entered into the search field and the Function App's principal is selected in the search results. The Select button is also highlighted.](media/key-vault-principal.png "Principal")

6. Expand the **Secret permissions** and check **Get** under Secret Management Operations.

    ![In the Add access policy form, the Secret Permissions dropdown is expanded with the Get checkbox checked.](media/key-vault-get-secret-policy.png "Add access policy")

7. Select **Add** to add the new access policy.

8. When you are done, you should have an access policy for the Function App's managed identity. Select **Save** to finish the process.

    ![In the list of Key Vault access policies, the policy that was just created is highlighted. The Save button is selected to commit the changes.](media/key-vault-access-policies.png "Access policies")

### Task 4: Finish the ProcessImage Azure Function

There are a few components within the starter project that must be completed, marked as TODO in the code. 

The first set of TODO items we will address are in the ProcessImage function, the FindLicensePlateText class that calls the Computer Vision API, and finally the SendToEventGrid.cs class, which is responsible for sending processing results to the Event Grid topic you created earlier.

> **Note:** If using Windows ensure you cloned to a folder with a short path `C:\work\` for example. Longer paths, such as  `C:\Users\workshop\Downloads\`, mean you will encounter build issues in later steps: `The specified path, file name, or both are too long. The fully qualified file name must be less than 260 characters, and the directory name must be less than 248 characters.`

1. In Visual Studio Code select **Open Folder...** and navigate to the **TollBooth** folder (`hands-on-lab\starter\TollBooth\TollBooth\`) and then choose **Select Folder**.

2. From the **Edit** menu in Visual Studio Code select **Find in files** and then search for 'TODO'.

3. The results will show you a list of TODO tasks, where each task represents one line of code that needs to be completed.

    ![A list of TODO tasks, including their description, project, file, and line number are displayed.](media/vs-task-list.png 'TODO tasks')

4. Open `ProcessImage.cs`. Notice that the Run method is decorated with the FunctionName attribute, which sets the name of the Azure Function to "ProcessImage". This is triggered by HTTP requests sent to it from Event Grid. This request is the result of an Event subscription for the Function, which you will do in a later task, subscribing to "blob-created" events. The Function's trigger watches for new blobs being added to the images container of the storage account created in Exercise 1. The data passed to the Function from Event Grid includes the URL of the blob which is passed to the input binding of the Function to obtain the binary data of the uploaded image.

5. The following code represents the completed task in `ProcessImage.cs` - update the file as shown:

    ```csharp
    // **TODO 1: Set the licensePlateText value by awaiting a new FindLicensePlateText.GetLicensePlate method.**
    licensePlateText = await new FindLicensePlateText(log, _client).GetLicensePlate(licensePlateImage);
    ```

6. Open `FindLicensePlateText.cs`. This class is responsible for contacting the Computer Vision API to find and extract the license plate text from the photo, using Optical Character Recognition (OCR). Notice that this class also shows how you can implement a resilience pattern using [Polly](https://github.com/App-vNext/Polly), an open source .NET library that helps you handle transient errors. This is useful for ensuring that you do not overload downstream services, in this case, the Computer Vision API. This will be demonstrated later on when visualizing the Function's scalability.

7. The following code represents the completed task in `FindLicensePlateText.cs`  - update the file as shown:

    ```csharp
    // TODO 2: Populate the below two variables with the correct AppSettings properties.
    var uriBase = Environment.GetEnvironmentVariable("computerVisionApiUrl");
    var apiKey = Environment.GetEnvironmentVariable("computerVisionApiKey");
    ```

8. Open `SendToEventGrid.cs`. This class is responsible for sending an Event to the Event Grid Topic, including the event type and license plate data. Event subscribers will use the event type to filter and act on the events they need to process. Make note of the event types defined here (the first parameter passed into the Send method), as they will be used later on when creating new Functions in the second Function App you provisioned earlier.

9. The following code represents the completed tasks in `SendToEventGrid.cs`:

    ```csharp
    // TODO 3: Modify send method to include the proper eventType name value for saving plate data.
    await Send("savePlateData", "TollBooth/CustomerService", data);

    // TODO 4: Modify send method to include the proper eventType name value for queuing plate for manual review.
    await Send("queuePlateForManualCheckup", "TollBooth/CustomerService", data);
    ```

    > **Note**: TODOs 5, 6, and 7 will be completed in later steps of the guide.

### Task 5: Deploy the Function App using GitHub Actions

In this task, you will configure GitHub Actions so that committing code will push the updates automatically to your Function in Azure.

When you cloned this repository you also cloned the necessary GitHub Actions code required to deploy. You will need to update a few items and enable the Action prior to its first use.

1. Navigate to your Function App in the Azure Portal and from the top navigation select **Get publish profile**. A text file will download to your computer.

    ![On Azure Portal, with a Function App open, with Download publish profile highlighted.](media/function-app-profile.png 'Download Function App Publish Profile')

2. Open the downloaded file in Visual Studio Code, select all the text and copy it.

3. Using Microsoft Edge, open GitHub and navigate to the **Secrets** section of **Settings** for the repository you cloned and create a new Secret called **AZURE_FUNCTIONAPP_PUBLISH_PROFILE**, pasting in the entire contents of the file from step 2.

    ![On GitHub, showing how to set Repository Secrets using Settings.](media/actions-github-secret.png 'Setting a GitHub repository secret')

4. Open the `main.yml` file from the '.guthub\workflows' folder using Visual Studio Code and change the placeholder **YOURFUNCTIONAPPNAME** with the actual name of your Azure Function Apps

5. In Visual Studio Code, click on the Git Extension in the left navigation and you should see a series of Changes listed. Enter an appropriate Message and hit Ctrl+Enter to commit them (on Mac Command+Enter).

6. From the Source Control ellipses menu select **Push** to send your changes to GitHub. This may take a few moments to complete.

    ![In Visual Studio Code's Git Extension with the 'Push' option highlighted.](media/git-push.png 'Visual Studio Code Git Extension - Push')

7. In Microsoft Edge, open the **Actions** section of your repository, and if required, enable the Action. If you wish to run the Action you can select it and choose **Run workflow**.

    ![On GitHub, in Actions with the 'run worklow' option highlighted.](media/run-action-manual.png 'Run GitHub Action manually')

9. Once the Action completes, open the Azure Portal and navigate to the **ServerlessArchitecture** resource group, then select the Azure Function App to which you just deployed.

10. Select **Functions** in the left-hand menu. You should see both functions you just published from the Visual Studio solution listed.

    ![In the Function Apps blade, in the left tree-view both TollBoothFunctionApp, and Functions (Read Only) are expanded. Beneath Functions (Read Only), two functions ExportLicensePlates and ProcessImage are highlighted.](media/dotnet-functions.png 'TollBoothFunctionApp blade')

11. Now we need to add an Event Grid subscription to the ProcessImage function, so the function is triggered when new images are added to blob storage. Select the **ProcessImage** function, select **Integration** on the left-hand menu, select **Event Grid Trigger (eventGridEvent)**, then select **Create Event Grid subscription**.

    ![In the TollboothFunctionApp tree-view, the ProcessImage function is selected. In the code window pane, the Add Event Grid subscription link is highlighted.](media/processimage-add-eg-sub.png 'ProcessImage function')

12. On the **Create Event Subscription** blade, specify the following configuration options:

    a. **Name**: Unique value for the subscription name similar to **processimagesub** (ensure the green check mark appears).

    b. **Event Schema**: Select **Event Grid Schema**.

    c. For **Topic Type**, select **Storage Accounts (Blob & GPv2)**.

    d. Select your **subscription** and **ServerlessArchitecture** resource group.

    e. For resource, select your recently created storage account. Enter **processimagesubtopic** into the **System Topic Name** field.

    f. Select only the **Blob Created** from the event types dropdown list.

    g. Leave Azure Function as the Endpoint Type.

13. Leave the remaining fields at their default values and select **Create**.

    ![In the Create event subscription form, the fields are set to the previously defined values.](media/processimage-eg-sub.png)

## Exercise 3: Create functions in the portal

**Duration**: 45 minutes

Create two new Azure Functions written in Node.js, using the Azure portal. These will be triggered by Event Grid and output to Azure Cosmos DB to save the results of license plate processing done by the ProcessImage function.

### Help references

|                                                                   |                                                                                                       |
| ----------------------------------------------------------------- | :---------------------------------------------------------------------------------------------------: |
| **Description**                                                   |                                                **Links**                                              |
| Create your first function in the Azure portal                    |        https://docs.microsoft.com/azure/azure-functions/functions-create-first-azure-function         |
| Store unstructured data using Azure Functions and Azure Cosmos DB | https://docs.microsoft.com/azure/azure-functions/functions-integrate-store-unstructured-data-cosmosdb |

### Task 1: Create Function to save license plate data to Azure Cosmos DB

In this task, you will create a new Node.js function triggered by Event Grid and that outputs successfully processed license plate data to Azure Cosmos DB.

1. Navigate to the Azure portal https://portal.azure.com/.

2. Open the **ServerlessArchitecture** resource group, then select the Azure Function App you created whose name ends with **Events**. If you did not use this naming convention, make sure you select the Function App that you _did not_ deploy to in the previous exercise.

3. Select **Functions** in the left-hand menu, then select **+ Add**.

    ![In the Function Apps blade, the TollBoothEvents2 application is selected. In the Overview tab, the + New function button is selected.](media/functions-new.png 'TollBoothEvents2 blade')

4. Select **Develop in portal** and then enter **event grid** into the template filter form, then select the **Azure Event Grid trigger** template. In the **New Function** field enter `SavePlateData`, then select **Add**.

    ![In the Template search form, event grid is typed in the search field. Below, the Event Grid trigger tile is highlighted.](media/new-function-event-grid-trigger-template.png "Add function dialog")

5. Select **Code + Test**, then replace the code in the new `SavePlateData` function with the following:

    ```javascript
    module.exports = function(context, eventGridEvent) {
        context.log(typeof eventGridEvent);
        context.log(eventGridEvent);

        context.bindings.outputDocument = {
            fileName: eventGridEvent.data['fileName'],
            licensePlateText: eventGridEvent.data['licensePlateText'],
            timeStamp: eventGridEvent.data['timeStamp'],
            exported: false
        };

        context.done();
    };
    ```

    ![The function code is displayed.](media/saveplatedata-code.png "SavePlateData Code + Test")

6. Select **Save**.

7. If you see the following error about Application Insights not being configured, ignore for now. We will add Application Insights in a later exercise.

    ![Error about App Insights not being installed.](media/app-insights-error-message.png "App Insights error")

### Task 2: Add an Event Grid subscription to the SavePlateData function

In this task, you will add an Event Grid subscription to the SavePlateData function. This will ensure that the events sent to the Event Grid topic containing the savePlateData event type are routed to this function.

1. With the `SavePlateData` Function open, select **Integration** in the left-hand menu, select **Event Grid Trigger (eventGridEvent)**, then select **Create Event Grid subscription**.

    ![In the SavePlateData blade code window, the Add Event Grid subscription link is selected.](media/saveplatedata-add-eg-sub.png 'SavePlateData blade')

2. On the **Create Event Subscription** blade, specify the following configuration options:

    a. **Name**: Unique value for the subscription name similar to **saveplatedatasub** (ensure the green check mark appears).

    b. **Event Schema**: Select **Event Grid Schema**.

    c. For **Topic Type**, select **Event Grid Topics**.

    d. Select your **Subscription** and **ServerlessArchitecture** resource group.

    e. For resource, select the Event Grid Topic you created in Exercise 1, Task 3.

    f. For Event Types, select **Add Event Type**.

    g. Enter `savePlateData` for the new event type value. This will ensure this Function is only triggered by this Event Grid event type.

    h. Leave Azure Function as the Endpoint Type.

3. Leave the remaining fields at their default values and select **Create**.

    ![In the Create Event Subscription blade, fields are set to the previously defined values.](media/saveplatedata-eg-sub.png "Create Event Subscription")

### Task 3: Add an Azure Cosmos DB output to the SavePlateData function

In this task, you will add an Azure Cosmos DB output binding to the SavePlateData function, enabling it to save its data to the Processed collection.

1. Close the Edit Trigger blade if it is still open. Select **+ Add output** under `Outputs` within Integrations. In the `Create Output` blade that appears, select the **Azure Cosmos DB** binding type.

    ![The Add Output link is highlighted with an arrow pointing to the highlighted binding type in the Create Output blade.](media/function-output-binding-type.png "Create Output")

2. Scroll down in the Create Output form, then select **New** underneath to the Azure Cosmos DB account connection field.

    ![The new button is selected next to the Azure Cosmos DB account connection field.](media/image49.png 'New button')

    > **Note**: If you see a notice for "Extensions not installed", select **Install**.

    ![A message is displayed indicating the Cosmos DB Extensions are not installed. The Install link is selected.](media/cosmos-extension-install.png 'Cosmos DB Extensions not installed')

3. Select your Cosmos DB account from the list that appears.

4. Specify the following configuration options in the Azure Cosmos DB output form:

    a. For **Database name**, type **LicensePlates**.

    b. For the **Collection Name**, type **Processed**.

    ![Under Azure Cosmos DB output the following field values display: Document parameter name, outputDocument; Collection name, Processed; Database name, LicensePlates; Azure Cosmos DB account connection, tollbooths_DOCUMENTDB.](media/saveplatedata-cosmos-integration.png 'Azure Cosmos DB output section')

5. Select **OK**.

    > **Note**: you should wait for the template dependency to install if you were prompted earlier.

### Task 4: Create Function to save manual verification info to Azure Cosmos DB

In this task, you will create a new Function triggered by Event Grid which outputs information to Azure Cosmos DB about photos that need to be manually verified.

1. Close the `SavePlateData` function. Select the **+ Add** button within the **Functions** blade of the Function App.

    ![In the left-hand menu next to the Functions item, the + button is highlighted along with its tooltip Create new displaying.](media/new-function-button.png "Create new function")

2. elect **Develop in portal** and then enter **event grid** into the template filter form, then select the **Azure Event Grid trigger** template. In the **New Function** field enter `QueuePlateForManualCheckup`, then select **Add**.

    ![Event grid is entered into the search field, and in the results, Azure Event Grid trigger tile displays.](media/queueplate-function-create.png 'Event grid trigger')

3. Select **Code + Test**, then replace the code in the new `QueuePlateForManualCheckup` Function with the following:

    ```javascript
    module.exports = async function(context, eventGridEvent) {
        context.log(typeof eventGridEvent);
        context.log(eventGridEvent);

        context.bindings.outputDocument = {
            fileName: eventGridEvent.data['fileName'],
            licensePlateText: '',
            timeStamp: eventGridEvent.data['timeStamp'],
            resolved: false
        };

        context.done();
    };
    ```

4. Select **Save**.

7. If you see the following error about Application Insights not being configured, ignore for now. We will add Application Insights in a later exercise.

    ![Error about App Insights not being installed.](media/app-insights-error-message.png "App Insights error")

### Task 5: Add an Event Grid subscription to the QueuePlateForManualCheckup Function

In this task, you will add an Event Grid subscription to the QueuePlateForManualCheckup function. This will ensure that the events sent to the Event Grid topic containing the queuePlateForManualCheckup event type are routed to this function.

1. With the `QueuePlateForManualCheckup` Function open, select **Integration** in the left-hand menu, select **Event Grid Trigger (eventGridEvent)**, then select **Create Event Grid subscription**.

    ![In the QueuePlateForManualCheckup Integration blade, the Create Event Grid subscription link is selected.](media/queueplateformanualcheckup-add-eg-sub.png "QueuePlateForManualCheckup blade")

2. On the **Create Event Subscription** blade, specify the following configuration options:

    a. **Name**: Unique value for the subscription name similar to **queueplateformanualcheckupsub** (ensure the green check mark appears).

    b. **Event Schema**: Select **Event Grid Schema**.

    c. For **Topic Type**, select **Event Grid Topics**.

    d. Select your **Subscription** and **ServerlessArchitecture** resource group.

    e. For resource, select the Event Grid Topic you created in Exercise 1, Task 3.

    f. For Event Types, select **Add Event Type**.

    g. Enter `queuePlateForManualCheckup` for the new event type value. This will ensure this function is only triggered by this Event Grid type.

    h. Leave Azure Function as the Endpoint Type.

3. Leave the remaining fields at their default values and select **Create**.

    ![In the Create Event Subscription blade, fields are set to the previously defined values.](media/manualcheckup-eg-sub.png)

### Task 6: Add an Azure Cosmos DB output to the QueuePlateForManualCheckup Function

In this task, you will add an Azure Cosmos DB output binding to the QueuePlateForManualCheckup function, enabling it to save its data to the NeedsManualReview collection.

1. Close the Edit Trigger blade if it is still open. Select **+ Add output** under `Outputs` within Integrations. In the `Create Output` blade that appears, select the **Azure Cosmos DB** binding type.

    ![The Add Output link is highlighted with an arrow pointing to the highlighted binding type in the Create Output blade.](media/function-output-binding-type.png "Create Output")

2. Specify the following configuration options in the Azure Cosmos DB output form:

    a. For **Database name**, enter **LicensePlates**.

    b. For **Collection Name**, enter **NeedsManualReview**.

    c. Select the **Cosmos DB account connection** you created earlier.

    ![In the Azure Cosmos DB output form, the following field values display: Document parameter name, outputDocument; Collection name, NeedsManualReview; Database name, LicensePlates; Azure Cosmos DB account connection, tollbooths_DOCUMENTDB.](media/manualcheckup-cosmos-integration.png 'Azure Cosmos DB output form')

3. Select **OK**.

## Exercise 4: Monitor your Functions with Application Insights

**Duration**: 15 minutes

Application Insights can be integrated with Azure Function Apps to provide robust monitoring for your functions. In this exercise, you will view telemetry in the Application Insights account that you created when provisioning the Function Apps. Since you assigned the Application Insights account to the Function Apps when creating them, the Application Insights telemetry key was added to the Function App configuration for you.

> **Note:**  Make sure you have the Blob Storage Connection string to the Account you created in Exercise 1, Task 1 available.

### Help references

|                                                               |                                                                                |
| ------------------------------------------------------------- | :----------------------------------------------------------------------------: |
| **Description**                                               |                                    **Links**                                   |
| Monitor Azure Functions using Application Insights            |     https://docs.microsoft.com/azure/azure-functions/functions-monitoring      |
| Live Metrics Stream: Monitor & Diagnose with 1-second latency | https://docs.microsoft.com/azure/application-insights/app-insights-live-stream |

### Task 1: Use the Live Metrics Stream to monitor functions in real time

1. Open the **TollboothMonitor** Application Insights instance from within your lab resource group.

    ![The Application Insights instance is highlighted in the resource group.](media/resource-group-application-insights.png "Application Insights")

2. In Application Insights, select **Live Metrics Stream** underneath Investigate in the menu.

    ![In the TollBoothMonitor blade, in the pane under Investigate, Live Metrics Stream is selected. ](media/live-metrics-link.png 'TollBoothMonitor blade')

3. In Visual Studio Code close any open project and open the `UploadImages` folder you cloned as part of this repository.

4. Open a new Terminal in Visual Studio Code by selecting **Terminal** in the menu and **New Terminal**.

    ![In Visual Studio Code, with New Terminal selected. ](media/vscode-uploadimages.png 'New Terminal')

5. In the new Terminal run the following commands. When you paste your Blob Storage connection string make sure to enclose in double quotes.

```
dotnet build

dotnet run "DefaultEndpointsProtocol=https;AccountName={YOURACCOUNT};AccountKey={YOURACCOUNTKEY};EndpointSuffix=core.windows.net"
```

>**Note:** On Windows, if the files are located under a longer root path, such as `C:\Users\workshop\Downloads\`, then you will encounter build issues in later steps: `The specified path, file name, or both are too long. The fully qualified file name must be less than 260 characters, and the directory name must be less than 248 characters.`

8. When prompted, enter **1** and press **ENTER**. This uploads a handful of car photos to the images container of your Blob storage account.

    ![The Terminal, showing images being uploaded.](media/image69.png 'Visual Studio Code Terminal output')

9. Switch back to your browser with the Live Metrics Stream still open in Application Insights. You should start seeing new Telemetry arrive, with the number of servers online at the bottom, the incoming request rate, CPU process amount, etc. You can select some of the sample Telemetry in the list to the side to view output data.

    ![The Live Metrics Stream window displays information for the two online servers. Displaying line and point graphs including incoming requests, outgoing requests, and overall health. To the side is a list of Sample Telemetry information. ](media/image70.png 'Live Metrics Stream window')

10. Leave the Live Metrics Stream window open, and return to Visual Studio Code. Re-run the `dotnet run` command and this time enter **2** and press **ENTER**. This will upload 1,000 new photos.

    ![The Terminal, showing images being uploaded.](media/image71.png 'Visual Studio Code Terminal output')

11. Switch back to the Live Metrics Stream in your browser and observe the activity as the photos are uploaded. You can see the number of servers online, which translate to the number of Function App instances that are running between both Function Apps. You should also notice things such as a steady cadence for the Request Rate monitor, the Request Duration hovering below \~200ms second, and the Incoming Requests roughly matching the Outgoing Requests.

    ![In the Live Metrics Stream window, two servers are online. Under Incoming Requests. the Request Rate heartbeat line graph is selected, as is the Request Duration dot graph. Under Overall Health, the Process CPU heartbeat line graph is also selected, the similarities between this graph and the Request Rate graph under Incoming Requests are highlighted for comparison.](media/image72.png 'Live Metrics Stream window')

12. After the upload has run for a while, you can cancel it, but leave the Live Metrics Stream window open.

### Task 2: Observe your Functions dynamically scaling when resource-constrained

In this task, you will change the Computer Vision API to the Free tier. This will limit the number of requests to the OCR service to 10 per minute. Once changed, run the UploadImages console app to upload 1,000 images again. The resiliency policy programmed into the FindLicensePlateText.MakeOCRRequest method of the ProcessImage function will begin exponentially backing off requests to the Computer Vision API, allowing it to recover and lift the rate limit. This intentional delay will greatly increase the function's response time, thus causing the Consumption plan's dynamic scaling to kick in, allocating several more servers. You will watch all of this happen in real time using the Live Metrics Stream view.

1. Open your Computer Vision API service by opening the **ServerlessArchitecture** resource group, and then selecting the **Cognitive Services** service name.

2. Select **Pricing tier** under Resource Management in the menu. Select the **F0 Free** pricing tier, then choose **Select**.

    > **Note**: If you already have an **F0** free pricing tier instance, you will not be able to create another one.

    ![In the Cognitive Services blade, under Resource Management, the Pricing tier item is selected. In the Choose your pricing tier blade, the F0 Free option is selected.](media/image73.png 'Choose your pricing tier blade')

3. Switch to Visual Studio Code, run the **UploadImages** project again, then enter **2** and press **ENTER**. This will upload 1,000 new photos.

    ![The Terminal, showing images being uploaded.](media/image71.png 'Visual Studio Code Terminal output')

4. Switch back to the Live Metrics Stream window and observe the activity as the photos are uploaded. After running for a couple of minutes, you should start to notice a few things. The Request Duration will start to increase over time. As this happens, you should notice more servers being brought online. Each time a server is brought online, you should see a message in the Sample Telemetry stating that it is "Generating 2 job function(s)", followed by a Starting Host message. You should also see messages logged by the resilience policy that the Computer Vision API server is throttling the requests. This is known by the response codes sent back from the service (429). A sample message is "Computer Vision API server is throttling our requests. Automatically delaying for 16000ms".

    > **Note**: If you select a sample telemetry and cannot see its details, drag the resize bar at the bottom of the list up to resize the details pane.

    ![In the Live Metrics Stream window, 11 servers are now online.](media/image74.png 'Live Metrics Stream window')

5. After the upload has run for a while, you can cancel it.

6. In the Azure Portal, navigate back to the **Computer Vision** API and set the pricing tier back to **S1 Standard**.

## Exercise 5: Explore your data in Azure Cosmos DB

**Duration**: 15 minutes

In this exercise, you will use the Azure Cosmos DB Data Explorer in the portal to view saved license plate data.

### Help references

|                       |                                                         |
| --------------------- | :------------------------------------------------------ |
| **Description**       |                         **Links**                       |
| About Azure Cosmos DB | https://docs.microsoft.com/azure/cosmos-db/introduction |


> **Note:** Ensure that your IP address is added to the IP list under the Firewall settings in your Azure Cosmos DB account. If not, then you will not able to see the License Plates data within Azure Cosmos DB.

### Task 1: Use the Azure Cosmos DB Data Explorer

1. Open your Azure Cosmos DB account by opening the **ServerlessArchitecture** resource group, and then selecting the **Azure Cosmos DB account** name.

2. Select **Data Explorer** from the menu.

    ![In the Data Explorer blade, Data Explorer is selected from the left menu.](media/data-explorer-link.png 'Tollbooth - Data Explorer blade')

3. Expand the **Processed** collection, then select **Items**. This will list each of the JSON documents added to the collection.

4. Select one of the documents to view its contents. The first four properties are ones that were added by your functions. The remaining properties are standard and are assigned by Cosmos DB.

    ![In the tree-view beneath the LicensePlates Cosmos DB, the Processed collection is expanded with the Items item selected. On the Items tab, a document is selected, and to the side, the JSON data associated with the document is displayed. The first four properties of the document (fileName, licencePlateText, timeStamp, and exported) are displayed along with the standard Cosmos DB properties.](media/data-explorer-processed.png 'Tollbooth - Data Explorer blade')

5. Expand the **NeedsManualReview** collection, then select **Items**.

6. Select one of the documents to view its contents. Notice that the filename is provided, as well as a property named "resolved". While this is out of scope for this lab, those properties can be used together to provide a manual process for viewing the photo and entering the license plate.

    ![In the tree-view beneath the LicensePlates Cosmos DB, the NeedsManualReview collection is expanded, and the Items item is selected. On the Items tab, a document is selected, and to the side, the JSON properties of the document are displayed. The first four properties of the document (fileName, licencePlateText, timeStamp, and resolved) are shown along with the standard Cosmos DB properties.](media/data-explorer-needsreview.png 'Tollbooth - Data Explorer blade')

7. Select the ellipses (...) next to the **Processed** collection and select **New SQL Query**.

    ![In the tree-view beneath the LicencePlates Cosmos DB, the Processed collection is selected. From its right-click context menu, New SQL Query is selected.](media/data-explorer-new-sql-query.png 'Tollbooth - Data Explorer blade')

8. Modify the SQL query to count the number of processed documents that have not been exported:

    ```sql
    SELECT VALUE COUNT(1) FROM c WHERE c.exported = false
    ```

9. Execute the query and observe the results. In our case, we have 669 processed documents that need to be exported.

    ![In the Query window, the previously defined SQL query displays. Under Results, the number 669 is highlighted.](media/cosmos-query-results.png 'Query 1 tab')

## Exercise 6: Create the data export Logic App

**Duration**: 30 minutes

In this exercise, you create a new Azure Logic App for your data export workflow. This Logic App will execute periodically and call your ExportLicensePlates function, then conditionally send an email if there were no records to export.

### Help references

|                                      |                                                                                                               |
| ------------------------------------ | :-----------------------------------------------------------------------------------------------------------: |
| **Description**                      |                                                    **Links**                                                  |
| What are Logic Apps?                 |                 https://docs.microsoft.com/azure/logic-apps/logic-apps-what-are-logic-apps                    |
| Call Azure Functions from Logic Apps | https://docs.microsoft.com/azure/logic-apps/logic-apps-azure-functions%23call-azure-functions-from-logic-apps |

### Task 1: Create the Logic App

1. Open the [Azure Portal Create Logic App Blade](https://portal.azure.com/#create/Microsoft.EmptyWorkflow). 

2. On the **Create Logic App** blade, specify the following configuration options:

    a. Specify the Resource Group **ServerlessArchitecture**.

    a. For **Logic app name**, type a unique value similar to **TollBoothLogic** (ensure the green check mark appears).

    c. Select the **Region** option for the location, then select the same **Location** as your Resource Group region.

    d. Ensure **Associate with integration service environment** is unchecked.

    d. Ensure **Enable Log Analytics** is unchecked.

    ![In the Create logic app blade, fields are set to the previously defined values.](media/create-logic-app.png 'Create logic app blade')

5. Select **Review + create**, then select **Create**. Open the Logic App once it has been provisioned.

6. In the Logic App Designer, scroll through the page until you locate the _Start with a common trigger_ section. Select the **Recurrence** trigger.

    ![The Recurrence tile is selected in the Logic App Designer.](media/image82.png 'Logic App Designer')

7. Enter **15** into the **Interval** box, and make sure Frequency is set to **Minute**. This can be set to an hour or some other interval, depending on business requirements.

8. Select **+ New step**.

    ![Under Recurrence, the Interval field is set to 15, and the + New step button is selected.](media/image83.png 'Logic App Designer Recurrence section')

9. Enter **Functions** in the filter box, then select the **Azure Functions** connector.

    ![Under Choose an action, Functions is typed in the search box. Under Connectors, Azure Functions is selected.](media/image85.png 'Logic App Designer Choose an action section')

10. Select your Function App whose name ends in **FunctionApp**, or contains the ExportLicensePlates function.

    ![Under Azure Functions, in the search results list, Azure Functions (TollBoothFunctionApp) is selected.](media/logic-app-function-app-action.png 'Logic App Designer Azure Functions section')

11. Select the **ExportLicensePlates** Function from the list.

    ![Under Azure Functions, under Actions (2), Azure Functions (ExportLicensePlates) is selected.](media/logic-app-select-export-function.png 'Logic App Designer Azure Functions section')

12. This Function does not require any parameters. Select **+ New step**, then search for **condition**. Select the **Condition** Control option from the Actions search result.

    ![In the logic app designer, in the ExportLicensePlates section, the parameter field is left blank. In the Choose an action box, condition is entered as the search term and the Condition Control item is selected from the Actions list.](media/logicapp-add-condition.png 'Logic App Designer ExportLicensePlates section')

13. For the **value** field, select the **Status code** parameter. Make sure the operator is set to **is equal to**, then enter **200** in the second value field.

    > **Note**: This evaluates the HTTP status code returned from the ExportLicensePlates Function, which will return a 200 (OK) code when license plates are found and exported. Otherwise, it sends a 204 (NoContent) status code when no license plates were discovered that need to be exported. We will conditionally put a message on a queue if any response other than 200 is returned.

    ![The first Condition field displays Status code. The second, drop-down menu field displays is equal to, and the third field is set to 200.](media/logicapp-condition.png 'Condition fields')

14. We will ignore the True condition because we don't want to perform an action if the license plates are successfully exported. Select **Add an action** within the **False** condition block.

    ![Under the Conditions field is an If true (green checkmark) section, and an if false (red x) section. In the If false section, the Add an action button is selected.](media/logicapp-condition-false-add.png 'Logic App Designer Condition fields if true/false ')

15. Enter **Azure Queues** in the filter box, then select the **Put a message on a queue** action.

    ![In the Choose an action box, Azure Queues is entered as the search term. From the Actions list, Put a message on a queue is selected.](media/logicapp-send-message.png 'Azure Queues put a message on queue')

16. In the Put a message on a queue Action, provide the following values:

    a. For **Connection Name** enter **FailedExportQueueConnection**.

    b. For **Storage Account** select the Storage Account from Exercise 1, Task 1 in which you created the **failedexports** queue.

    c. Select **Create**.

    ![In the Put a message on a queue Action, fields are set to the previously defined values.](media/logic-app-storage-connection.png 'Logic App Designer , Storage Connection fields')

17. The Action will change once connected. For **Queue Name** select **failedexports**. Enter an appropriate message into the **Message field**

    ![In the Put a message on a queue Action, fields are set to the previously defined values.](media/logic-app-message-queue-message.png 'Logic App Designer , Queue message fields')

    > **Note:** if you don't see 'failedexports' as an option it may be that you selected the wrong storage account. You can use **Change connection** to connect to the right account.

18. Select **Save** at the top left in the editor tool bar to save your Logic App.

19. Select **Run** to execute the Logic App.

    ![The Run button is selected on the Logic Apps Designer blade toolbar.](media/logicapp-start.png 'Logic Apps - Run')

20. While in the Logic Apps Designer, you will see the run result of each step of your workflow. A green checkmark is placed next to each step that successfully executed, showing the execution time to complete. This can be used to see how each step is working, and you can select the executed step and see the raw output.

    ![In the Logic App Designer, green check marks display next to Recurrence, ExportLicensePlates, Condition, and Send an email steps of the logic app.](media/image96.png 'Logic App Designer ')

21. In your browser navigate to the Azure Storage Account that contains the queue you are using and then review the new message that was published due to the failed export.

    ![In the Azure Portal with a Storage Queue open, showing a message on the queue.](media/storage-queue-failed-message.png 'Azure Storage Queue showing message in queue')

## Exercise 7: Complete ExportLicensePlates Function App

**Duration**: 10 minutes

In this exercise, finsish adding code to your Function App and push code to GitHub so it is deployed automatically to Azure.

### Task 1: Finish your ExportLicensePlates Function code and push changes to GitHub to trigger deployment

1. In Visual Studio Code select **Open Folder...** and navigate to the **TollBooth** folder (`hands-on-lab\starter\TollBooth\TollBooth\`) and then choose **Select Folder**.

2. From the **Edit** menu in Visual Studio Code select **Find in files** and then search for 'TODO'.

3. The results will show you a list of TODO tasks, where each task represents one line of code that needs to be completed.

    ![A list of TODO tasks, including their description, project, file, and line number are displayed.](media/vs-task-list.png 'TODO tasks')

4. Open `DatabaseMethods.cs`.

5. The following code represents the completed task in `DatabaseMethods.cs`:

    ```csharp
        // TODO 5: Retrieve a List of LicensePlateDataDocument objects from the collectionLink where the exported value is false.
        licensePlates = _client.CreateDocumentQuery<LicensePlateDataDocument>(collectionLink,
                new FeedOptions() { EnableCrossPartitionQuery=true,MaxItemCount = 100 })
            .Where(l => l.exported == false)
            .ToList();
        // TODO 6: Remove the line below.
    ```

6. Make sure that you deleted the following line under TODO 6: `licensePlates = new List<LicensePlateDataDocument>();`.

7. Save your changes then open `FileMethods.cs`.

8. The following code represents the completed task in `FileMethods.cs`:

    ```csharp
    // TODO 7: Asyncronously upload the blob from the memory stream.
    await blob.UploadFromStreamAsync(stream);
    ```

9. Save your changes.

10. In Visual Studio Code, click on the Git Extension in the left navigation and you should see a series of Changes listed. Enter an appropriate Message and hit Ctrl+Enter to commit them (on Mac Command+Enter).

11. From the Source Control ellipses menu select **Push** to send your changes to GitHub. This may take a few moments to complete.

    ![In Visual Studio Code's Git Extension with the 'Push' option highlighted.](media/git-push.png 'Visual Studio Code Git Extension - Push')

7. In Microsoft Edge, open the **Actions** section of your repository on GitHub and you will see the Action running.

    ![On GitHub showing a running GitHub Action.](media/github-actions-running.png 'GitHub Actions running')

## Exercise 8: Rerun the Logic App and verify data export

**Duration**: 10 minutes

With the latest Function App code changes in place, re-run your Logic App and verify that the files are successfully exported.

>**Note:** if wish to run through these Exercises multiple times you will need to upload new license plate files using the `UploadImages` application from Exercise 4, Task 1.

### Task 1: Run the Logic App

1. Open your **ServerlessArchitecture** resource group in the Azure portal, then select your Logic App.

3. Select **Run Trigger**, then select **Recurrence** to immediately execute your workflow.

    ![In the TollBoothLogic Logic app blade, Run Trigger and Recurrence are selected.](media/image114.png 'TollBoothLogic blade')

4. Select the **Refresh** button next to the Run Trigger button to refresh your run history. Select the latest run history item. If the expression result for the condition is **true**, then that means the CSV file should've been exported to Blob storage. You can see that the False path has not executed because of the grey cross.

    ![In Logic App Designer, in the Condition section, under Inputs, true is highlighted.](media/image115.png 'Logic App Designer ')

### Task 2: View the exported CSV file

1. Open your **ServerlessArchitecture** resource group in the Azure portal, then select your **Storage account** you had provisioned to store uploaded photos and exported CSV files.

2. In the Overview pane of your storage account, select **Containers**.

    ![In the Overview blade, Containers is selected.](media/storage-containers.png 'Services section')

3. Select the **export** container.

    ![Export is selected under Name.](media/image117.png 'Export option')

4. You should see at least one recently uploaded CSV file. Select the filename to view its properties.

    ![In the Export blade, under Name, a .csv file is selected.](media/blob-export.png 'Export blade')

5. Select **Edit** in the blob properties window to view the contents of the CSV.

    ![A CSV file displays with the following columns: FileName, LicensePlateText, TimeStamp, and LicensePlateFound.](media/csv.png 'CSV file')

6. The **ExportLicensePlates** Function updates all of the Cosmos DB records it exported by setting the exported value to true. This makes sure that only new records since the last export are included. Verify this by re-executing the script in Azure Cosmos DB (Excercise 5, Step 9) that counts the number of items in the Processed collection where the exported property is 'false'. It should return 0 unless you've subsequently uploaded new photos.

## After the hands-on lab

**Duration**: 5 minutes

In this exercise, attendees will deprovision any Azure resources that were created in support of the lab.

### Task 1: Delete the resource group in which you placed your Azure resources

1. In the Azure Portal, navigate to the blade of your **Resource Group** and select **Delete** in the command bar at the top.

    ![In the Export blade, under Name, a .csv file is selected.](media/delete-rg.png 'Export blade')

2. Confirm the deletion by re-typing the **resource group name** and selecting **Delete**.

3. If you created a different Resource Group for your Lab Virtual Machine, be sure to delete that as well.

4. Optionally, delete the GitHub repository you created for this lab by selecting **settings** and then **Delete this repository** from the GitHub website. You can always Fork the original again in future.

You should follow these steps provided _after_ attending the Hands-on lab.