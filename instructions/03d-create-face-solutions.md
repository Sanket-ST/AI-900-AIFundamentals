# Module 03c: Explore face recognition

## Lab overview

Computer vision solutions often require an artificial intelligence (AI) solution to be able to detect human faces. For example, suppose the retail company Northwind Traders wants to locate where customers are standing in a store to best assist them. One way to accomplish this is to determine if there are any faces in the images, and if so, to identify the bounding box coordinates around the faces.

To test the capabilities of the Face service, we'll use a simple command-line application that runs in the Cloud Shell. The same principles and functionality apply in real-world solutions, such as web sites or phone apps.

## Lab objectives
  
In this lab, you will perform:
- Create a Face API resource and a Azure Storage Account.
- Configure and run a client application.

## Estimated timing: 60 minutes

## Architecture Diagram
![](media/Module3c.png)
 
### Exercise 1 : Create a Face API resource

## Task 1 : Create a Face API resource

You can use the Face service by creating a **Face** resource. (Face API is no longer available in Cognitive Services)

If you haven't already done so, create a **Face API** resource in your Azure subscription.

1. Click the **&#65291;Create a resource** button, search for *Face*, and create a **Face** resource with the following settings:
    - **Subscription**: *Use existing Azure subscription*.
    - **Resource group**: **AI-900-Module-03c-<inject key="DeploymentID" enableCopy="false" />**
    - **Region**: Select **<inject key="location" enableCopy="false"/>**
    - **Name**: Enter **ai900face-<inject key="DeploymentID" enableCopy="false"/>**
    - **Pricing tier**: **Standard S0**

1.  Click on **Review and create**.
   
1. After successfully completing the validation process, click on the **Create** button located in the lower left corner of the page.
   
1. Wait for deployment to complete(it can take a few minutes), and then click on the **Go to resource** button, this will take you to your Face API.

1. View the **Keys and Endpoint** page for your Face resource. click on Show keys, you will need the endpoint and keys to connect from client applications.

      >**Note**: Copy and save the **KEY 1** and **Endpoint** value to NotePad for future reference to connect from client applications. 

### Task 2: Run Cloud Shell

To test the capabilities of the Face service, we'll use a simple command-line application that runs in the Cloud Shell on Azure. 

1. In the Azure portal, select the **[>_]** (*Cloud Shell*) button at the top of the page to the right of the search box. This opens a Cloud Shell pane at the bottom of the portal. 

    ![Start Cloud Shell by clicking on the icon to the right of the top search box](media/create-face-solutions/ai900_03c-1.png)

1. The first time you open the Cloud Shell, you may be prompted to choose the type of shell you want to use (*Bash* or *PowerShell*). Select **PowerShell**. If you do not see this option, skip the step.  

1. If you are prompted to create storage for your Cloud Shell, ensure your subscription is selected and click on **show advanced settings**. Please make sure you have selected your resource group **AI-900-Module-03c-<inject key="DeploymentID" enableCopy="false"/>** and enter **blob<inject key="DeploymentID" enableCopy="false"/>** for the **Storage account name**. Next, enter **blobfileshare<inject key="DeploymentID" enableCopy="false"/>** For the **File share name**, then click on **Create Storage**.

    ![Create storage by clicking confirm.](media/create-face-solutions/create-a-storage.png)       

1. Make sure the type of shell indicated on the top left of the Cloud Shell pane is switched to *PowerShell*. If it is *Bash*, switch to *PowerShell* by using the drop-down menu.

    ![How to find the left hand drop down menu to switch to PowerShell](media/create-face-solutions/ai900_03c-3.png) 

1. Wait for PowerShell to start. You should see the following screen in the Azure portal:  

    ![Wait for PowerShell to start.](media/create-face-solutions/ai900_03c-4.png)

### Task 3: Configure and run a client application

Now that you have a custom model, you can run a simple client application that uses the Face service.

1. In the command shell, enter the following command to download the sample application and save it to a folder called ai-900.

    ```PowerShell
    git clone https://github.com/MicrosoftLearning/AI-900-AIFundamentals ai-900
    ```

1. The files are downloaded to a folder named **ai-900**. Now we want to see all of the files in your Cloud Shell storage and work with them. Type the following command into the shell:

     ```PowerShell
    code .
    ```

    Notice how this opens up an editor like the one in the image below: 

    ![The code editor.](media/create-face-solutions/ai900_03c-5.png) 

1. In the **Files** pane on the left, expand **ai-900** and select **find-faces.ps1**. This file contains some code that uses the Face service to detect and analyze faces in an image, as shown here:

    ![The editor containing code to detect faces in an image](media/create-face-solutions/ai900_03c-6.png)

1. Don't worry too much about the details of the code, the important thing is that it needs the endpoint URL and either of the keys for your Face resource. Copy these from the **Keys and Endpoints** page for your resource (Task 1, Step 5) and paste them into the code editor, replacing the **YOUR_KEY** with *KEY 1* and **YOUR_ENDPOINT** with *Endpoint* placeholder values, respectively.

    > **Tip**: You may need to use the separator bar to adjust the screen area as you work with the **Keys and Endpoint** and **Editor** panes.

    After pasting the key and endpoint values, the first two lines of code should look similar to this:

    
    > $key="1a2b3c4d5e6f7g8h9i0j...."    
    > $endpoint="https..."
    

1. After making the changes to the variables in the code, press **CTRL+S** to save the file. Then press **CTRL+Q** to close the code editor..

    The sample client application will use your Face service to analyze the following image, taken by a camera in the Northwind Traders store:

    ![An image of a parent using a cellphone camera to take a picture of a child in in a store](media/create-face-solutions/ai900_03c-7.jpg)

1. In the PowerShell pane, enter the following commands to run the code:

    ```PowerShell
    cd ai-900
    ```

     ```PowerShell
    ./find-faces.ps1 store-camera-1.jpg
    ```

1. Review the returned information, which includes the location of the face in the image. The location of a face is indicated by the top-left coordinates, and the width and height of a *bounding box*, as shown here:
    
    ![An image of a person with their face outlined](media/create-face-solutions/ai900_03c-8.jpg)
    ![An image of a person with their face outlined](media/resultai-9003c.png)

    >**Note**: Face service capabilities that return personally identifiable features are restricted. See https://azure.microsoft.com/blog/responsible-ai-investments-and-safeguards-for-facial-recognition/ for details.

1. Now let's try another image:

    ![An image of person with a shopping basket](media/create-face-solutions/ai900_03c-9.jpg)

    To analyze the second image, enter the following command:

    ```PowerShell
    ./find-faces.ps1 store-camera-2.jpg
    ```

1. Review the results of the face analysis for the second image.

1. Let's try one more:

    ![An image of person with a shopping cart](media/create-face-solutions/ai900_03c-10.jpg)

    To analyze the third image, enter the following command:

    ```PowerShell
    ./find-faces.ps1 store-camera-3.jpg
    ```

1. Review the results of the face analysis for the third image.

    > **Congratulations** on completing the task! Now, it's time to validate it. Here are the steps:
    > - Click Lab Validation tab located at the upper right corner of the lab guide section and navigate to the Lab Validation tab.
    > - Hit the Validate button for the corresponding task.
    > - If you receive a success message, you can proceed to the next task. If not, carefully read the error message and retry the step, following the instructions in the lab guide.
    > - If you need any assistance, please contact us at labs-support@spektrasystems.com. We are available 24/7 to help you out.

### Learn more

This simple app shows only some of the capabilities of the Face service. To learn more about what you can do with this service, see the [Face API page](https://azure.microsoft.com/services/cognitive-services/face/).

### Review
In this lab, you have completed:
- Create a Face API resource and a Azure Storage Account.
- Configure and run a client application.

## You have successfully completed this lab.
