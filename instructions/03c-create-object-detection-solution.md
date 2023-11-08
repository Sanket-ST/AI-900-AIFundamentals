# Module 03b: Explore object detection

## Lab overview

*Object detection* is a form of computer vision in which a machine learning model is trained to classify individual instances of objects in an image, and indicate a *bounding box* that marks its location. You can think of this as a progression from *image classification* (in which the model answers the question "What is this an image of?") to building solutions where we can ask the model "What objects are in this image, and where are they?"

For example, a road safety initiative might identify pedestrians and cyclists as being the most vulnerable road users at traffic intersections. By using cameras to monitor intersections, images of road users could be analyzed to detect pedestrians and cyclists in order to monitor their numbers or even change the behaviour of traffic signals.

The **Custom Vision** cognitive service in Microsoft Azure provides a cloud-based solution for creating and publishing custom object detection models. In Azure, you can use the Custom Vision service to train an object detection model based on existing images. There are two elements to creating an object detection solution. First, you must train a model to detect the location and class of objects using labelled images. Then, when the model is trained you must publish it as a service that can be consumed by applications.

To test the capabilities of the Custom Vision service to detect objects in images, we'll use a simple command-line application that runs in the Cloud Shell. The same principles and functionality apply to real-world solutions, such as websites or mobile apps.

## Lab objectives
In this lab, you will perform:
- Create a Cognitive Services resource
- Create a Custom Vision project

## Estimated timing: 60 minutes

## Architecture Diagram

![](media/Module3b.png)

## Exercise 1: Create a *Cognitive Services* resource

### Task 1: Create a *Cognitive Services* resource

You can use the Custom Vision service by creating either a **Custom Vision** resource or a **Cognitive Services** resource.

> **Note**: Not every resource is available in every region. Whether you create a Custom Vision or Cognitive Services resource, only resources created in [certain regions](https://azure.microsoft.com/global-infrastructure/services/?products=cognitive-services) can be used to access Custom Vision services. For simplicity, a region is pre-selected for you in the configuration instructions below.

Create a **Cognitive Services** resource in your Azure subscription.

1. In the Azure Portal click the **&#65291;Create a resource** button.

   ![Start Cloud Shell by clicking on the icon to the right of the top search box](media/ai900mod1img1.png)
   
1. In the Marketplace page search for **Cognitive Services** and Select **Cognitive Services**.

    ![Start Cloud Shell by clicking on the icon to the right of the top search box](media/ai900mod3bimg1.png)
     
1. On Cognitive Services Page Click on **Create**. 
     
     ![Start Cloud Shell by clicking on the icon to the right of the top search box](media/ai900mod3bimg2.png)

1. Create a **Cognitive Services** resource with the following settings:

    - **Subscription**: Use existing subscription. **(1)**
    - **Resource group**:Select **AI-900-Module-03b-<inject key="DeploymentID" enableCopy="false"/> (2)**
    - **Region**: Select **<inject key="location" enableCopy="false" /> (3)**
    - **Name**: Enter **AI-900-cognitive-<inject key="DeploymentID" enableCopy="false"/> (4)**
    - **Pricing tier**: Standard S0 **(5)**
    - **By checking this box I acknowledge that I have read and understood all the terms below**: Check the box. **(6)**
    -  Click **Review + Create (7)**
    
     ![Start Cloud Shell by clicking on the icon to the right of the top search box](media/ai900mod3bimg3.png)

1. After successfully completing the validation process, click on the **Create** button located in the lower left corner of the page.

1. Wait for deployment to complete(it can take a few minutes), and then click on the **Go to resource** button, this will take you to your Cognitive Services.
 
1. View the **Keys and Endpoint** page for your Cognitive Services resource. You will need the endpoint and keys to connect from client applications.
   
   >**Note**: Copy and save the **KEY 1** and **Endpoint** value to NotePad for future reference to connect from client applications. 

### Task 2: Create a Custom Vision project

To train an object detection model, you need to create a Custom Vision project based on your training resource. To do this, you'll use the Custom Vision portal.

1. In a new browser tab, open the Custom Vision portal at [https://customvision.ai](https://customvision.ai?azure-portal=true), and sign in using the Microsoft account associated with your Azure subscription.
1. In the **Terms of Service** box, check the box and click on **I agree**.

     ![Start Cloud Shell by clicking on the icon to the right of the top search box](media/ai900mod3bimg4.png)

1. Click **New project**.

   ![Start Cloud Shell by clicking on the icon to the right of the top search box](media/ai900mod3bimg5.png)

1. Create a new project with the following settings:

    - **Name**: Traffic Safety **(1)**
    - **Description**: Object detection for road safety **(2)**
    - **Resource**: **AI-900-cognitive-<inject key="DeploymentID" enableCopy="false"/> (3)**
    - **Project Types**: Object Detection **(4)**
    - **Domains**: General \[A1] **(5)**
        >**Note**: Under the **Resource** dropdown if you don't find the resource that you created previously in the Azure portal, kindly refresh the page and reperform the task.
    - Click on **Create Project (6)**

      ![Start Cloud Shell by clicking on the icon to the right of the top search box](media/ai900mod3bimg6.png)
    
1. Wait for the project to be created and opened in the browser.

### Task 3: Add and tag images

To train an object detection model, you need to upload images that contain the classes you want the model to identify, and tag them to indicate bounding boxes for each object instance.

1. Download and extract the training images from [https://aka.ms/traffic-images](https://aka.ms/traffic-images). The extracted folder contains a collection of images of cyclists and pedestrians.

1. In the Custom Vision portal, in your **Traffic Safety** object detection project, select **Add images** and upload all of the images in the extracted folder.

      ![Start Cloud Shell by clicking on the icon to the right of the top search box](media/ai900mod3bimg7.png)

1. After the images have been uploaded, select the first one to open it.

1. Hold the mouse over any object (cyclist or pedestrian) in the image until an automatically detected region is displayed. Then select the object, and if necessary resize the region to surround it. Alternatively, you can simply drag around the object to create a region.

    >**Note**: When the object is tightly selected within the rectangular region, enter the appropriate tag for the object (*Cyclist* or *Pedestrian*) and use the **Tag region** (**+**) button to add the tag to the project.

     ![Screenshot of an image with a tagged region in the Image Detaol dialog box.](media/tag-image-3b.png)

1. Use the **Next** **(>)** link on the right to go to the next image, and tag its objects. Then just keep working through the entire image collection, tagging each cyclist and pedestrian.

   As you tag the images, note the following:

    - Some images contain multiple objects, potentially of different types. Tag each one, even if they overlap.
    - After a tag has been entered once, you can select it from the list when tagging new objects.
    - You can go back and forward through the images to adjust tags.

        ![Screenshot of an image with a tagged region in the Image Detaol dialog box.](media/multiple-objects-3b.png)

1. When you have finished tagging the last image, close the **Image Detail** editor, and on the **Training Images** page, under **Tags**, select **Tagged** to see all of your tagged images:

      ![Picture1](media/tagged-images-3b.png)

### Task 4: Train and test a model

Now that you've tagged the images in your project, you're ready to train a model.

1. In the Custom Vision project, click **Train** to train an object detection model using the tagged images. 

    ![Start Cloud Shell by clicking on the icon to the right of the top search box](media/ai900mod3bimg8.png)
   
1. Select the **Quick Training** option, Again click **Train**.
 
    ![Start Cloud Shell by clicking on the icon to the right of the top search box](media/ai900mod3bimg9.png)

  
    > **Tip**: Training may take several minutes. While you're waiting, check out [Video analytics for smart cities](https://www.microsoft.com/research/video/video-analytics-for-smart-cities/), which describes a real project to use computer vision in a road safety improvement initiative.

1. When training is complete, review the *Precision*, *Recall*, and *mAP* performance metrics - these measure the prediction goodness of the object detection model, and should all be reasonably high.

1. Adjust the **Probability Threshold** on the left, increasing it from 50% to 90%, and observe the effect on the performance metrics. This setting determines the probability value that each tag evaluation must meet or exceed to be counted as a prediction.

      ![Screenshot of performance metrics for a trained model.](media/performance-metrics-3b.png)

1. At the top right of the page, click **Quick Test**, and then in the **Image URL** box, enter `https://aka.ms/pedestrian-cyclist` and view the results.

   ![Screenshot of performance metrics for a trained model.](media/quicktest-3b.png)
      
   >**Note**: In the pane on the right, under **Predictions**, each detected object is listed with its tag and probability. Select each object to see it highlighted in the image.

   >**Note**: The predicted objects may not all be correct - after all, cyclists and pedestrians share many common features. The predictions that the model is most confident about have the highest probability values. Use the **Threshold Value** slider to eliminate objects with a low probability. You should be able to find a point at which only correct predictions are included (probably at around 85-90%).

      ![Screenshot of performance metrics for a trained model.](media/test-detection-3b.png)

1. Then close the **Quick Test** window.

### Task 5: Publish the object detection model

Now you're ready to publish your trained model and use it from a client application.

1. Click **&#128504; Publish**.

   ![Photograph of a group of pedestrians.](media/ai900mod3bimg10.png)
 
1. To publish the trained model with the following settings:
    - **Model name**: traffic-safety **(1)**
    - **Prediction resource**: **AI-900-cognitive-<inject key="DeploymentID" enableCopy="false"/> (2)**
    - Click **Publish (3)**

1. After publishing, click the *Prediction URL* (&#127760;) icon to see the information required to use the published model.

      ![Start Cloud Shell by clicking on the icon to the right of the top search box](media/ai900mod3bimg11.png)
       
Later, you will need the appropriate URL and Prediction-Key values to get a prediction from an Image URL, so keep this dialog box open and carry on to the next task.

### Task 6: Prepare a client application

To test the capabilities of the Custom Vision service, we'll use a simple command-line application that runs in the cloud shell on Azure.

1. Switch back to the browser tab containing the Azure portal, and select the **Cloud shell** (**[>_]**)  button at the top of the page to the right of the search box. This opens a cloud shell pane at the bottom of the portal.

    ![Start Cloud Shell by clicking on the icon to the right of the top search box](media/powershell-portal-guide-1.png)

1. The first time you open the Cloud Shell, you may be prompted to choose the type of shell you want to use (*Bash* or *PowerShell*). If so, select **PowerShell**.

    ![Start Cloud Shell by clicking on the icon to the right of the top search box](media/ai900mod1img6.png)

 1. If you are prompted to create storage for your Cloud Shell, ensure your subscription is selected and click on **Show advanced settings**.
    
    ![Start Cloud Shell by clicking on the icon to the right of the top search box](media/ai900mod1img7.png)

 1. Please make sure you have selected your resource group **AI-900-Module-03b-<inject key="DeploymentID" enableCopy="false"/> (1)** and enter **blob<inject key="DeploymentID" enableCopy="false"/> (2)** for the **Storage account Name** and enter **blobfileshare<inject key="DeploymentID" enableCopy="false"/>(3)** for the **File share Name**, then click on **Create Storage (4)**.
 
      When the cloud shell is ready, it should look similar to this:
    
      ![Start Cloud Shell by clicking on the icon to the right of the top search box](media/ai900mod1img8.png)

      > **Tip**: Ensure that the type of shell indicated on the top left of the Cloud Shell pane is *PowerShell*. If it is *Bash*, switch to *PowerShell* by using the drop-down menu.

     > **Note**:  that you can resize the cloud shell by dragging the separator bar at the top of the pane, or by using the **&#8212;**, **&#9723;**, and **X** icons at the top right of the pane to minimize, maximize, and close the pane. For more information about using the Azure Cloud Shell, see the [Azure Cloud Shell documentation](https://docs.microsoft.com/azure/cloud-shell/overview).

1. In the command shell, enter the following commands to download the files for this exercise and save them in a folder named **ai-900** (after removing that folder if it already exists)

    ```PowerShell
    rm -r ai-900 -f
    ```
    ```PowerShell
    git clone https://github.com/MicrosoftLearning/AI-900-AIFundamentals ai-900
    ```

1. After the files have been downloaded, enter the following commands to change to the **ai-900** directory and edit the code file for this exercise:

    ```PowerShell
    cd ai-900
    ```
    ```PowerShell
    code detect-objects.ps1
    ```

    Notice how this opens an editor like the one in the image below:

      ![Screenshot of the code editor in the cloud shell.](media/code-editor-3b.png)

     > **Tip**: You can use the separator bar between the cloud shell command line and the code editor to resize the panes.

1. Don't worry too much about the details of the code. The important thing is that it starts with some code to specify the prediction URL and key for your Custom Vision model. You'll need to update these so that the rest of the code uses your model.

   > **Note**: Get the *prediction URL* and *prediction key* from the dialog box you left open in the browser tab for your Custom Vision project. You need the versions to be used *if you have an image URL*.

    Use these values to replace the **YOUR_PREDICTION_URL** and **YOUR_PREDICTION_KEY** placeholders in the code file.
    After pasting the Prediction URL and Prediction Key values, the first two lines of code should look similar to this:

    
     > $predictionUrl="https..."   
     > $predictionKey ="1a2b3c4d5e6f7g8h9i0j...."


1. After making the changes to the variables in the code, press **CTRL+S** to save the file. Then press **CTRL+Q** to close the code editor.

### Task 7: Test the client application

Now you can use the sample client application to detect cyclists and pedestrians in images.

1. In the PowerShell pane, enter the following command to run the code:

    ```PowerShell
    ./detect-objects.ps1 1
    ```

    This code uses your model to detect objects in the following image:

     ![Photograph of a pedestrian and a cyclist.](media/create-object-detection-solution/road-safety-1.jpg)

1. Review the prediction, which lists any objects detected with a probability of 90% or more, along with the coordinates of a bounding box around their location.

1. Now let's try another image. Run this command:

    ```PowerShell
    ./detect-objects.ps1 2
    ```

    This time the following image is analyzed:

    ![Photograph of a group of pedestrians.](media/create-object-detection-solution/road-safety-2.jpg)

    Hopefully, your object detection model did a good job of detecting pedestrians and cyclists in the test images.

    >**Note**: If you are not able to see the Result in Powershell, then nevigate back to the custom vision portal, go to the **predictions** tab, you see the details of the images as showned below:

   ![Photograph of a group of pedestrians.](media/ai900mod3bimg12.png)

   > **Congratulations** on completing the task! Now, it's time to validate it. Here are the steps:
   > - Click Lab Validation tab located at the upper right corner of the lab guide section and navigate to the Lab Validation tab.
   > - Hit the Validate button for the corresponding task.
   > - If you receive a success message, you can proceed to the next task. If not, carefully read the error message and retry the step, following the instructions in the lab guide.
   > - If you need any assistance, please contact us at labs-support@spektrasystems.com. We are available 24/7 to help you out.

### Learn more

This exercise shows only some of the capabilities of the Custom Vision service. To learn more about what you can do with this service, see the [Custom Vision page](https://azure.microsoft.com/services/cognitive-services/custom-vision-service/).

### Review
In this lab, you have completed:
- Create a Cognitive Services resource
- Create a Custom Vision project

## You have successfully completed this lab.
