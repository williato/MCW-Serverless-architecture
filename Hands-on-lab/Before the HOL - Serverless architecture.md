![Microsoft Cloud Workshops](https://github.com/Microsoft/MCW-Template-Cloud-Workshop/raw/master/Media/ms-cloud-workshop.png 'Microsoft Cloud Workshops')

<div class="MCWHeader1">
Serverless architecture
</div>

<div class="MCWHeader2">
Before the hands-on lab setup guide
</div>

<div class="MCWHeader3">
January 2021
</div>

Information in this document, including URL and other Internet Web site references, is subject to change without notice. Unless otherwise noted, the example companies, organizations, products, domain names, e-mail addresses, logos, people, places, and events depicted herein are fictitious, and no association with any real company, organization, product, domain name, e-mail address, logo, person, place or event is intended or should be inferred. Complying with all applicable copyright laws is the responsibility of the user. Without limiting the rights under copyright, no part of this document may be reproduced, stored in or introduced into a retrieval system, or transmitted in any form or by any means (electronic, mechanical, photocopying, recording, or otherwise), or for any purpose, without the express written permission of Microsoft Corporation.

Microsoft may have patents, patent applications, trademarks, copyrights, or other intellectual property rights covering subject matter in this document. Except as expressly provided in any written license agreement from Microsoft, the furnishing of this document does not give you any license to these patents, trademarks, copyrights, or other intellectual property.

The names of manufacturers, products, or URLs are provided for informational purposes only and Microsoft makes no representations and warranties, either expressed, implied, or statutory, regarding these manufacturers or the use of the products with any Microsoft technologies. The inclusion of a manufacturer or product does not imply endorsement of Microsoft of the manufacturer or product. Links may be provided to third party sites. Such sites are not under the control of Microsoft and Microsoft is not responsible for the contents of any linked site or any link contained in a linked site, or any changes or updates to such sites. Microsoft is not responsible for webcasting or any other form of transmission received from any linked site. Microsoft is providing these links to you only as a convenience, and the inclusion of any link does not imply endorsement of Microsoft of the site or the products contained therein.

© 2020 Microsoft Corporation. All rights reserved.

**Contents**

- [Serverless architecture before the hands-on lab setup guide](#serverless-architecture-before-the-hands-on-lab-setup-guide)
  - [Requirements](#requirements)
  - [Before the hands-on lab](#before-the-hands-on-lab)
    - [Task 1: Create a new Azure Resource group](#task-1-create-a-new-azure-resource-group)
    - [Task 2: Validate connectivity to Azure](#task-5-validate-connectivity-to-azure)
    - [Task 3: Clone and explore the TollBooth starter solution](#task-6-download-and-explore-the-tollbooth-starter-solution)

# Serverless architecture before the hands-on lab setup guide

## Requirements

**Please complete these steps at least one day prior to the lab!**

- Microsoft Azure subscription (non-Microsoft subscription). You can create a free account at https://azure.com/free.
- Local machine or an Azure virtual machine configured with:
  - Git (https://git-scm.com/) - accept all defaults when installing.
  - Azure Functions Core Tools (https://docs.microsoft.com/azure/azure-functions/functions-run-local#install-the-azure-functions-core-tools)
  - .NET Core 3.1 (https://dotnet.microsoft.com/download/dotnet-core/3.1) SDK
  - .NET 5 (https://dotnet.microsoft.com/download/dotnet/5.0) SDK
  - Visual Studio Code (https://code.visualstudio.com/download). Install these extensions ([instructions for extensions](https://code.visualstudio.com/docs/editor/extension-gallery)):
    - Azure Functions
    - C#
  - Modern web browser such as Microsoft Edge (https://www.microsoft.com/edge) - make sure to set it as your default (edge://settings/defaultBrowser)
- GitHub account. You can create a free account at https://github.com/join.

Once Git is installed you will need to configure two defaults before first use. Open a command prompt and run these two commands, substituting values as required:

```
git config --global user.name "Your Name"
git config --global user.email "youremail@yourdomain.com"
```

## Before the hands-on lab

**Duration**: 10 minutes

In this exercise, you will set up your environment you will use for the rest of the exercises. This will involve downloading the sample application and creating your Azure resource group for the lab.

### Task 1: Create a new Azure Resource group

1. Open the [Azure Portal Resource Groups Blade](https://portal.azure.com/#blade/HubsExtension/BrowseResourceGroups).

2. Select **Add**.

   ![In Resource Groups Blade and the Add button is selected.](images/Setup/image9.png 'Azure Portal Resoure Groups')

3. Specify the name of the resource group as **ServerlessArchitecture**, and choose the Azure region to which you want to deploy the lab. This resource group will be used throughout the rest of the lab.

   - Select **Review + Create**. This will show you a summary of changes. 
   - On the final screen Select **Create** to create the resource group.

   ![In the Create a resource group blade, the Resource group field displays ServerlessArchitecture.](images/Setup/image10a.png 'Resource group blade')

### Optional: setup an Azure VM for Labs.

If you wish to use an Azure Virtual Machine (VM) for the labs then please use [VM setup instructions](Before%20the%20HOL%20-%20Setup%20Azure%20VM.md) before proceeding with further setps here.

### Task 2: Validate connectivity to Azure

1. Launch Visual Studio Code and select the Azure Extension.

   ![Visual Studio Code left navigation with Azure Extension selected.](images/Setup/image14.png 'Visual Studio Code Azure Extension')

2. In the navigation pane either select **Sign in to Azure...**. If prompted, make sure you use Microsoft Edge (or the browser you installed) and log into the Azure Subscription you want to use. If you don't have a subscription select **Create a Free Azure Account** and follow the setps to sign up.

   ![In Visual Studio Code, Azure Functions Navigation Pane.](images/Setup/image15.png 'Azure Functions Navigation Pane')

   Once you are successfully logged in you will see the Subscription name show in the navigation pane.

### Task 3: Fork, Clone and explore the TollBooth starter solution

> Make sure to use a web browser that is logged into your GitHub account to perform these actions.

1. Navigate to https://github.com/sjwaight/MCW-Serverless-architecture and at the top right select 'Fork'. Once created you will be automatically taken to your copy of the repository.

   ![On GitHub, selecting Fork option for repository.](images/Setup/image16.png 'Select the Fork button to create a copy')

3. In Visual Studio Code select the Git Extension and then select **Clone Repository**. In the Command Palette select **Clone from GitHub**. You will be prompted to sign into your GitHub account.

   ![In Visual Studio Code, selecting Clone Repository button.](images/Setup/image17.png 'Selecting Clone Repository button')

   ![In Visual Studio Code, selecting Clone from GitHub.](images/Setup/image18.png 'Select Clone from GitHub')

2. Once signed in to GitHub, return to Visual Studio Code and select the repository you just forked and clone it to your local machine. Select an appropriate local folder to which to clone to.

5. Navigate to `Hands-on lab\starter` folder in the repository you just cloned.

6. In the **TollBooth** folder you can see the following projects:

   - TollBooth (.NET Core 3.1)
   - UploadImages (.NET 5)
   
   > **Note**: The UploadImages project is used for uploading a handful of car photos for testing scalability of the serverless architecture.

7. Navigate back to the **starter** subfolder and open the **license plates** subfolder. It contains sample license plate photos used for testing out the solution. One of the photos is guaranteed to fail OCR processing, which is meant to show how the workload is designed to handle such failures. The **copyfrom** folder is used by the UploadImages project as a basis for the 1,000 photo upload option for testing scalability.

Congratulations! You're now ready to start the lab work!