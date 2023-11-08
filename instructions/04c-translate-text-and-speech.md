# Module 04b: Explore translation

## Lab overview

One of the driving forces that has enabled human civilization to develop is the ability to communicate with one another. In most human endeavours, communication is key.

Artificial Intelligence (AI) can help simplify communication by translating text or speech between languages, helping to remove barriers to communication across countries and cultures.

To test the capabilities of the Translator service, we'll use a simple command-line application that runs in the Cloud Shell. The same principles and functionality apply to real-world solutions, such as websites or phone apps.

## Lab objectives
In this lab, you will perform:
+ Create a *Cognitive Services* resource

## Estimated timing: 60 minutes

## Architecture Diagram
![](media/Module4b.png)

## Exercise 1: Create a *Cognitive Services* resource

### Task 1: Create a *Cognitive Services* resource

You can use the Computer Vision service by creating either a **Translator** resource or a **Cognitive Services** resource.

If you haven't already done so, create a **Cognitive Services** resource in your Azure subscription.

1. In the Azure Portal click the **&#65291;Create a resource** button.

     ![Start Cloud Shell by clicking on the icon to the right of the top search box](media/ai900mod1img1.png)
   
1. In the Marketplace page search for **Cognitive Services** and Select **Cognitive Services** 

     ![Start Cloud Shell by clicking on the icon to the right of the top search box](media/ai900mod3bimg1.png)
     
1. On Cognitive Services Page Click on **Create**. 
     
     ![Start Cloud Shell by clicking on the icon to the right of the top search box](media/ai900mod3bimg2.png)

1. Create a **Cognitive Services** resource with the following settings:

    - **Subscription**: Retain the Existing Subscription **(1)**.
    - **Resource group**: Select **AI-900-Module-04b-<inject key="DeploymentID" enableCopy="false"/> (2)**
    - **Region**: Select **<inject key="location" enableCopy="false"/> (3)**
    - **Name**: Enter **ai900cognitive-<inject key="DeploymentID" enableCopy="false"/> (4)**
    - **Pricing tier**: Standard S0 **(5)**
    - **By checking this box I acknowledge that I have read and understood all the terms below**: Select the checkbox. **(6)**

1. Click on **Review + Create (7)**.
   
     ![](media/ai900mod4bimg.png)
   
1. After successfully completing the validation process, click on the **Create** button located in the lower left corner of the page, and wait for deployment to complete. Then go to the deployed resource.
   
1. Wait for deployment to complete(it can take a few minutes), and then click on the **Go to resource** button, this will take you to your Cognitive Service.

1. View the **Keys and Endpoint** page for your resource. You will need the **location/region** and **key** to connect from client applications.

### Task 2: Run Cloud Shell

To test the capabilities of the Translation service, we'll use a simple command-line application that runs in the Cloud Shell on Azure. 

1. In the Azure portal, select the **[>_]** (*Cloud Shell*) button at the top of the page to the right of the search box. This opens a Cloud Shell pane at the bottom of the portal.

    ![Start Cloud Shell by clicking on the icon to the right of the top search box](media/powershell-portal-guide-1.png)

1. The first time you open the Cloud Shell, you may be prompted to choose the type of shell you want to use (*Bash* or *PowerShell*). Select **PowerShell**. If you do not see this option, skip the step.  

1. If you are prompted to create storage for your Cloud Shell, ensure your subscription is selected and click on **show advanced settings**. Please make sure you have selected your resource group **AI-900-Module-04b-<inject key="DeploymentID" enableCopy="false"/>** and enter **blob<inject key="DeploymentID" enableCopy="false"/>** for the **Storage account name** and enter **blobfileshare<inject key="DeploymentID" enableCopy="false"/>** For the **File share name**, then click on **Create Storage**.

    ![Create storage by clicking confirm.](media/translate-text-and-speech/create-a-storage.png)

1. Make sure the type of shell indicated on the top left of the Cloud Shell pane is switched to *PowerShell*. If it is *Bash*, switch to *PowerShell* by using the drop-down menu. 

    ![How to find the left hand drop down menu to switch to PowerShell](media/powershell-portal-guide-3.png) 

1. Wait for PowerShell to start. You should see the following screen in the Azure portal:  

    ![Wait for PowerShell to start.](media/powershell-prompt.png)

### Task 3: Configure and run a client application

Now that you have a custom model, you can run a simple client application that uses the Translation service.

1. In the command shell, enter the following command to download the sample application and save it to a folder called ai-900.

    ```PowerShell
    git clone https://github.com/MicrosoftLearning/AI-900-AIFundamentals ai-900
    ```

1. The files are downloaded to a folder named **ai-900**. Now we want to see all of the files in your Cloud Shell storage and work with them. Type the following command into the shell: 

     ```PowerShell
    code .
    ```

    Notice how this opens up an editor like the one in the image below: 

    ![The code editor.](media/powershell-portal-guide-4.png)

1. In the **Files** pane on the left, expand **ai-900** and select **translator.ps1**. This file contains some code that uses the Translator service:

    ![The editor containing code to use the Translator service](media/translate-code-4b.png)

1. Don't worry too much about the details of the code, the important thing is that it needs the region/location and either of the keys for your Cognitive Services resource. Copy the values of **KEY 1** and **Location/Region** value from *Keys and Endpoints* page for your resource from the Azure portal and paste them into the code editor.
     ![Find the key and endpoint tab in your Cognitive Services resource's left hand pane.](media/lab4b-1.png)

     > **Note:** The Translator service does not require the use of the Cognitive Service endpoint, so there is no need to modify the Translator service endpoint. Instead, a dedicated global endpoint is available specifically for the Translator service. 

1. At the top right of the editor pane, use the **...** button to open the menu and select **Save** to save your changes. Then open the menu again and select **Close Editor**.

    The sample client application will use the Translator service to do several tasks:
    - Translate text from English into French, Italian, and Chinese.
    - Translate audio from English into text in French

    Use the below link to hear the input audio which will be processed by this application and translates:
   
       https://www.microsoft.com/videoplayer/embed/RWORN0

    >**Note**: Copy the above link to your browser, and listen to the audio file. Do not use the Lab VM browser.

    >**Note**: A real application could accept the input from a microphone and send the response to a speaker, but in this simple example, we'll use pre-recorded input in an audio file.
    
1. In the Cloud Shell pane, enter the following command to run the code:

    ```PowerShell
    cd ai-900
    ./translator.ps1
    ```

1. Review the output. Did you see the translation from the text in English to French, Italian, and Chinese?  Did you see the English audio "hello" translated into text in French?

   > **Congratulations** on completing the task! Now, it's time to validate it. Here are the steps:
   > - Click Lab Validation tab located at the upper right corner of the lab guide section and navigate to the Lab Validation tab.
   > - Hit the Validate button for the corresponding task.
   > - If you receive a success message, you can proceed to the next task. If not, carefully read the error message and retry the step, following the instructions in the lab guide.
   > - If you need any assistance, please contact us at labs-support@spektrasystems.com. We are available 24/7 to help you out.

### Learn more

This simple app shows only some of the capabilities of the Translator service. To learn more about what you can do with this service, see the [Translator page](https://docs.microsoft.com/azure/cognitive-services/translator/translator-overview).

### Review
In this lab, you have completed:
- Create a *Cognitive Services* resource
  
## You have successfully completed this lab.
