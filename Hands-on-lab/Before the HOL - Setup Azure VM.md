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

This guide provides steps required to setup a Windows Virtual Machine in Azure for the purposes of completing the labs.

> **NOTE:** these steps are not required if you intend to use a local development machine. Please return to the [main setup guide](Before%20the%20HOL%20-%20Serverless%20architecture.md) in this case.

You will need a client that supports RDP connections - this is available as part of Windows and can be downloaded if you are on a [Mac](https://docs.microsoft.com/windows-server/remote/remote-desktop-services/clients/remote-desktop-mac) or on [Linux](https://remmina.org/). 

**Contents**

- [Task: Set up a remote development environment](#task-set-up-a-development-environment)

### Task: Set up a remote development environment

1. Open the [Azure Portal](https://portal.azure.com/) and then open the Azure Cloud Shell by selecting the icon for it in the top navigation.

   ![Azure Portal navigation with Cloud Shell icon highlighted.](images/Setup/image11.png 'Azure Portal navigation with Cloud Shell icon highlighted')

2. When prompted, create a new Storage Account for your Cloud Shell environment. Create it in the Resource group you created earlier. You can select either Bash or PowerShell for your Cloud Shell environment.

3. Once the Cloud Shell prompt is loaded you can use the following command to create a new Windows server instance. Make sure to replace 'myVM' with a unique hostname. When prompted set a strong password.

   ```bash
   az vm create \
    --resource-group ServerlessArchitecture \
    --name myVM \
    --image win2016datacenter \
    --admin-username azureuser \
    --nsg-rule RDP \
    --size Standard_DS2_v2
   ```

   > If the request fails please check that the name ('myVM') you specified is unique and the VM size you selected is available in the region you are using. 

4. Once the machine is created you can download the RDP file to remotely connect to it. Navigate to the VM in the Azure Portal and then click **Connect** in the VM Overview blade.

   ![Azure Portal VM Blade with download RDP selected.](images/Setup/image10.png 'Download the RDP configuration selected')

5. Login to the newly created VM using RDP and the username and password you supplied earlier.

6. After the VM desktop loads, the Server Manager will open automatically.

7. Select **Local Server**.

   ![Local Server is selected from the Server Manager menu.](images/Setup/image5.png 'Server Manager menu')

8. On the right hand side of the Server Manager look for **IE Enhanced Security Configuration**, and then select where it reads **On**.

   ![The IE Enhanced Security Configuration setting is set to On. The On item is selected.](images/Setup/image6.png 'IE Enhanced Security Configuration')

   - Change to **Off** for Administrators and select **OK**.

   ![In the Internet Explorer Enhanced Security Configuration dialog box, under Administrators, the Off button is selected.](images/Setup/image7.png 'Internet Explorer Enhanced Security Configuration dialog box')

> **NOTE**: Some aspects of these labs require the use of the new Microsoft Edge (Chromium edition) browser. You may find yourself blocked if using Internet Explorer later in the lab.

9. Launch Internet Explorer and download [Microsoft Edge](https://www.microsoft.com/edge).

10. Follow the setup instructions and set Edge as the default browser by opening edge://settings/defaultBrowser and selecting **Make default**.

11. Please install the pre-requisite software on the VM:

   - Git (https://git-scm.com/) - accept all defaults when installing
   - Visual Studio Code (https://code.visualstudio.com/download). Install these extensions ([instructions for extensions](https://code.visualstudio.com/docs/editor/extension-gallery)):
      - Azure Functions
      - C#
   - .NET Core 3.1 (https://dotnet.microsoft.com/download/dotnet-core/3.1) SDK
   - .NET 5 (https://dotnet.microsoft.com/download/dotnet/5.0) SDK

Once Git is installed you will need to configure two defaults before first use. Open a command prompt and run these two commands, substituting values as required:

```
git config --global user.name "Your Name"
git config --global user.email "youremail@yourdomain.com"
```

You can now return the [main setup guide](Before%20the%20HOL%20-%20Serverless%20architecture.md).