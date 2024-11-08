# Hands-on Lab: Explore Automated Machine Learning in Azure ML

## Lab scenario

The model will use a dataset of historical bicycle rental details to predict the number of rentals expected on a given day. It leverages seasonal and meteorological features to enhance the accuracy of its forecasts

## Lab objectives

In this lab, you will complete the following tasks:

- Task 1: Create an Azure Machine Learning workspace
- Task 2: Create compute
- Task 3: Create a dataset
- Task 4: Run an automated machine learning job
- Task 5: Review the best model
- Task 6: Deploy a predictive service
- Task 7: Test the deployed service

## Estimated time: 45 minutes

## Exercise 1: Create an Azure Machine Learning workspace  

### Task 1: Create an Azure Machine Learning workspace

1. Select **+ Create a resource**, search for Machine Learning.

    ![Picture1](../media/ai900mod1img1.png)

1. In the Marketplace page search for **Azure Machine Learning** and Select **Azure Machine Learning**.
 
    ![Picture2](../media/ai900mod2cimg1.png)

1. On **Azure Machine Learning** Page Click on **Create**.

   ![Picture3](../media/ai900mod2cimg2.png)
   
1. create a new **Azure Machine Learning** resource with an *Azure Machine Learning* plan. Use the following settings:

    - **Subscription**: *Use the existing Azure subscription*
    - **Resource group**: Select **AI-900-Module-02-<inject key="DeploymentID" enableCopy="false"/>**
    - **Workspace name**: Enter **ai900workspace-<inject key="DeploymentID" enableCopy="false"/>**
    - **Region**: Select **<inject key="location" enableCopy="false"/>**
    - **Storage account**: *Note the default new storage account that will be created for your workspace*
    - **Key vault**: *Note the default new key vault that will be created for your workspace*
    - **Application insights**: *Note the default new application insights resource that will be created for your workspace*
    - **Container registry**: None (*one will be created automatically the first time you deploy a model to a container*)

   ![Picture4](../media/amlconfig.png)

1. Select **Review + create**.

1. After successfully completing the validation process, click on the **Create** button located in the lower left corner of the page.
   
1. Wait for deployment to complete, and then click on the **Go to resource** button, this will take you to your workspace resource.

1. Select **Launch studio** (or open a new browser tab and navigate to [https://ml.azure.com](https://ml.azure.com?azure-portal=true), and sign into Azure Machine Learning studio using your Microsoft account).

   ![amlstudio](../media/launchstudio.png)

1. Close any messages that are displayed.

1. In Azure Machine Learning studio, you should see your newly created workspace. If that is not the case, select your Azure directory in the left-hand menu. Then from the new left-hand menu select **Workspaces**, where all the workspaces associated to your directory are listed, and **select the one you created for this exercise**.

### Task 2: Create Compute instance and Compute cluster

1. Open [Azure Machine Learning Studio](https://ml.azure.com?azure-portal=true) and click the **menu icon (≡)** in the top left corner. This icon opens a navigation pane where you can access different sections of the interface. Make sure your screen is maximized to view all available options. From this pane, go to the **Manage** section and select **Compute**.

1. Within the **Compute** section, navigate to the **Compute instances** tab. To create a new instance, click on **+ New**, which will open the setup options for a new compute instance.

   ![Compute Instance](../media/instance.png)

1. In the **Create compute instance** pane, configure the following settings:

   - **Virtual machine type**: CPU
   - **Virtual machine size**:
      - Choose **Select from all options**
      - Search for and select **Standard_DS11_v2**
   
   - **Compute name**: Enter a unique name, such as **ai900instance<inject key="DeploymentID" enableCopy="false"/>**

1. Click **Review + Create** to initialize the compute instance.

   ![compute](../media/instance12.png)

> **Note**:The compute instance will start provisioning, and will take some time to be created. You can move onto the next step while the instance is provisioning.
 
1. On the **Compute** page, select the **Compute clusters** tab and to add a new compute cluster, click on **+ New** with the following settings. You'll use this to train a machine learning model:
 
      ![Picture1](../media/ai900mod2cimg5.png)
    - **Location**: Select <inject key="location" enableCopy="false" />
    - **Virtual machine tier**: Dedicated
    - **Virtual machine type**: CPU
    - **Virtual machine size**:
        - Choose **Select from all options**
        - Search for and select **Standard_DS11_v2**
    - Select **Next**
      ![Picture2](../media/ai900mod2cimg6.png)
    - **Compute name**: Enter **ai900compute-<inject key="DeploymentID" enableCopy="false"/>**
    - **Minimum number of nodes**: 0
    - **Maximum number of nodes**: 2
    - **Idle seconds before scale down**: 120
    - **Enable SSH access**: keep it as default
    - Select **Create**
 
       ![Picture3](../media/ai900mod2cimg7.png)

> **Note**:The compute cluster will take some time to be created. You can move onto the next step while you wait.

### Task 3: Create a dataset

1. Download the zip file from the link [https://aka.ms/bike-rentals](https://aka.ms/bike-rentals?azure-portal=true) in your web browser, open the file location and extract the file.
 
   ![Picture1](../media/extract.png)
 
1. In [Azure Machine Learning studio](https://ml.azure.com?azure-portal=true), expand the left pane by selecting the menu icon at the top left of the screen. View the **Data** page (under **Assets**). The Data page contains specific data files or tables that you plan to work with in Azure ML. You can create datasets from this page as well.
 
1. On the **Data** page, under the **Data assets** tab, select **+ Create**. Then configure a data asset with the following settings:
    * **Data type**:
        * **Name**: bike-rentals
        * **Description**: Bicycle rental data
        * **Type**: Tabular
    * Click on **Next**.
 
      ![Step 1](../media/step1.png)    
 
    * **Data source**: From local files.
    * Click on **Next**.
    * In the **Select a datastore** pane, make sure that **workspaceblobstore** is selected and click on **Next**.
      ![Step3](../media/step312.png)
 
    * In the **Choose a file or folder** pane, select **Upload files or folder** and select the **Upload file**.
    * Now, open the extracted **bike-data** folder and select the **daily-bike-share.csv** file and click on **Open**.
 
      ![Data](../media/datasrc1.png)
 
    * Click on **Next**.
 
    * **Settings**:
        * **File format**: Delimited
        * **Delimiter**: Comma
        * **Encoding**: UTF-8
        * **Column headers**: Only first file has headers
        * **Skip rows**: None
        * **Dataset contains multi-line data**: *do not select*
    * Click on **Next**.

      ![Settings](../media/settings.png)
 
    * **Schema**:
        * Include all columns other than **Path**
        * Review the automatically detected types
    * Click on **Next**.
 
    * **Review**
        * Select **Create**
 
1. After the dataset has been created, open it and view the **Explore** page to see a sample of the data. This data contains historical features and labels for bike rentals.
 
> **Citation**: *This data is derived from [Capital Bikeshare](https://www.capitalbikeshare.com/system-data) and is used in accordance with the published data [license agreement](https://www.capitalbikeshare.com/data-license-agreement)*.

### Task 4: Run an automated machine learning job

Follow the next steps to run a job that uses automated machine learning to train a regression model that predicts bicycle rentals.

1. In [Azure Machine Learning studio](https://ml.azure.com?azure-portal=true), expand the left pane by selecting the menu icon at the top left of the screen. View the **Automated ML** page (under **Authoring**).

1. Click on **+ New Automated ML job**. Create an Automated ML job with the following settings:

      ![Aml model](../media/aml.png)

    - **Basic settings**:
        - **Job name**: bike-rentals (1)
        - **New experiment name**: mslearn-bike-rental (2)
    - Click on **Next** (3).
      
       ![Step2](../media/aml1.png)
      
    - **Task type & data**: 
        - **Task type**: Regression *(the model predicts a numeric value).
        - **Select data**: Select **bike-rentals**.
     - Click on **Next**.

       ![Step3](../media/datatype1.png)

    - **Task settings**:
        - **Target column**: rentals(Integer) (*this is the label that the model is trained to predict*)(1)
          
       ![Step4](../media/step412.png)
          
        - Select **Additional configuration settings:**(2)
        - **Primary metric**: Select **Normalized root mean squared error**
        - **Explain best model**: Selected — *this option causes automated machine learning to calculate feature importance for the best model which makes it possible to determine the influence of each feature on the predicted label.*
        - **Use all supported models**: Unselected. *You'll restrict the job to try only a few specific algorithms.*
        - **Allowed models**: *Select only **RandomForest** and **LightGBM** — normally you'd want to try as many as possible, but each model added increases the time it takes to run the job.*
        - Click on **Save**.

            ![Step 5](../media/step42.png)

        - Expand **Limits**
            - **Metric score threshold**: 0.085 — *if a model achieves a normalized root mean squared error metric score of 0.085 or less, the job ends.*(3)
   
     - Click on **Next**(4).
          
    - **Compute**:
        - **Select compute type**: *Compute cluster*
        - **Select Azure ML compute cluster**: **ai900compute-<inject key="DeploymentID" enableCopy="false"/>**
     - Click on **Next** and Click on **Submit training job**
              
1. When you finish submitting the automated machine learning job details, it starts automatically. Wait for the status to change from *Preparing* to *Running*.

1. When the status changes to *Running*, view the **Models** tab and observe as each possible combination of training algorithm and pre-processing steps is tried and the performance of the resulting model is evaluated. The page automatically refreshes periodically, but you can also select **Refresh**. It might take 10 minutes or so before models start to appear, as the cluster nodes must be initialized before training can begin.

1. Wait for the job to finish. It might take a while — now might be a good time for a coffee break!

### Task 5: Review the best model

1. On the **Overview** tab of the automated machine learning job, note the best model summary.
    ![Screenshot of the best model summary of the automated machine learning job with a box around the algorithm name.](../media/use-automated-machine-learning/ai-900-overview.png)

    >**NOTE:**
    > You may see a message under the status "Warning: User specified exit score reached...". This is an expected message. Please continue to the next step.  
1. Select the text under **Algorithm name** for the best model to view its details.

1. Select the **Metrics** tab, use the arrows icon to expand the panel if it is not already expanded and select the **residuals** and **predicted_true** charts if they are not already selected. 

    ![Screenshot of the metrics tab with the residuals and predicted_true charts selected.](../media/use-automated-machine-learning/ai-900-matrix1.png)

    Scroll down and review the charts which show the performance of the model. The first chart shows the *residuals*, the differences between predicted and actual values, as a histogram, the second chart compares the predicted values against the true values.

1. Select the **Explanations(preview)** tab. Select an Explanation ID and then select **Aggregate feature importance** tab. This chart shows how much each feature in the dataset influences the label prediction, like this:

    ![Screenshot of the feature importance chart on the Explanations tab.](../media/use-automated-machine-learning/feature-importance1.png)

### Task 6: Deploy a predictive service

1. In [Azure Machine Learning studio](https://ml.azure.com?azure-portal=true), on the **Automated ML** page, select your automated machine learning job.

1. On the **Overview** tab, select the algorithm name for the best model.

    ![Screenshot of the best model summary with a box around the algorithm name on the details tab.](../media/use-automated-machine-learning/ai-900-algorithm.png)

1. On the **Model** tab, select the **Deploy** button and use the **web service** option.

      ![Step1](../media/ai900lab2img2.png)
      
3. To deploy the model with the following settings and then click on **Deploy**.
    - **Name**: predict-rentals
    - **Description**: Predict cycle rentals
    - **Compute type**: Azure Container Instance
    - **Enable authentication**: Selected

       ![Step2](../media/ai900lab2img1.png)

1. Wait for the deployment to start - this may take a few seconds. Then, in the **Model summary** section, observe the **Deploy status** for the **predict-rentals** service, which should be **Running**. Wait for this status to change to **Succeeded**, which may take some time. You may need to select **Refresh** periodically.

1. After the deployment status changes to **Succeeded**, an endpoint will be created. This endpoint is a web-accessible URL that serves as an interface to your deployed machine learning model, allowing you to send data to the model and receive real-time predictions.

### Task 7: Test the deployed service

1. In Azure Machine Learning Studio, navigate to the **Endpoints** section from the left-hand menu.

   ![Screenshot of location of Endpoints on the left hand menu.](../media/endpoints1-02.png)

2. On the **Endpoints** page, locate and open the **predict-rentals** real-time endpoint.

   ![Screenshot of location of Endpoints on the left hand menu.](../media/use-automated-machine-learning/endpoints-2.png)

   > **Note:** If the endpoint remains in an unhealthy state, it may take up to 30 minutes for the deployment status to change to **Healthy**. If the status still doesn’t update, delete the endpoint and repeat the steps from Task 5.

3. On the left-hand menu of Azure Machine Learning Studio, select **Notebooks**.

   > **Note:** Since Azure Machine Learning Studio doesn’t support V1 deployment testing, we’ll use the `predict.ipynb` notebook to send data to the "predict-rentals" model endpoint in Azure Machine Learning and receive predictions.

4. In the Notebooks section, click on **Add files** and select **Upload files**.

   ![uploadfiles](../media/upload1.png)

5. Select **Click to browse and select file(s)** and navigate to **C:\AllFiles**. Choose both the **input.json** and **predict.ipynb** files and click **Open**.

   ![uploadstep2](../media/upload2.png)

6. Tick the checkbox that says **"I trust the content of these files"** and then click **Upload**.

7. Once uploaded, open the **predict.ipynb** notebook. Make sure that **Python 3.10 SDK-v2** is selected in the kernels.

8. Run each code snippet in the notebook.

   ![ipynbfile](../media/jupyter.png)

   > **Note:** The **predict.ipynb** file contains the Python code that enables interaction with the deployed endpoint in Azure Machine Learning. It will send the input data (from `input.json`) to the endpoint and retrieve the predictions.

9. Once you execute the final code cell, you should see the prediction output, which will display the rental predictions.

   ![Result](../media/result12.png)

> **Congratulations** on completing the task! Now, it's time to validate it. Here are the steps:
 - Hit the Validate button for the corresponding task. If you receive a success message.
- If not, carefully read the error message and retry the step, following the instructions in the lab guide.
- If you need any assistance, please contact us at cloudlabs-support@spektrasystems.com. We are available 24/7 to help you out.

<validation step="2e4871a4-3e69-40a1-bbd1-dc026b54d5ac" />

### Summary

In this lab, you have created an Azure Machine Learning workspace, set up compute resources, registered datasets, ran automated ML jobs, reviewed and deployed the best model, and tested the deployed service.

### You have successfully completed this lab.
