# Spring Boot on IBM Cloud with IBM Continuous Delivery

The goal of this lab is to demonstrate deploying a simple Java web application to IBM Cloud with a DevOps pipeline for managing the application lifecycle. 

As part of this deployment, several actions are happening automatically through the use of configuration files found in the [.bluemix](.bluemix) folder. The two primary actions that are automatically configured to occur are deploying two different microservices from two different repositories and integrating multiple IBM Cloud services into the deployed applications. 

## Table of Contents

- [Prerequisites](#prerequisites)
- [Deployment Steps](#deployment-steps)
  - [Admin Dashboard Microservice](#admin-dashboard-microservice)
- [Access Cloudant DB service](#cloudant-db)
- [Updating Individual Microservices](#updating-individual-microservices)
- [Deployment Configuration](#deployment-configuration)
- [Associated Repositories](#associated-repositories)
- [Security Vulnerability Scanning](#security-vulnerability-scanning)
- [Deploying To Kubernetes](#deploying-to-kubernetes)

## Prerequisites

1. A IBM Cloud account is required to complete this lab.

    To sign up for a new account visit [IBM Cloud](https://cloud.ibm.com/ "Welcome to IBM Cloud")

2. A Slack team is an **optional** requirement to complete this lab.

    To sign up for a new team visit [slack.com](https://slack.com/)

## Deployment Steps

1. Click the following Deploy to IBM Cloud button to load the initial toolchain setup:

    [![Deploy to IBM Cloud](https://cloud.ibm.com/devops/setup/deploy/button.png)](https://bluemix.net/deploy?repository=https://github.com/garage-milan/spring-boot-continuous-delivery)

2. Once the Tool Integrations have loaded, click on each of the four different integrations to see what settings are available to be changed. 

    **Note:** that the fields for GitHub, Eclipse Orion Web IDE, and Delivery Pipeline are automatically populated with data and no input should be required by the user unless changes to what was populated are desired.

    <img src="media/ToolIntegrations.png" width="500">
    
    **Optional:** Slack Integration. Skip to Step 4 if you choose not to configure it. 
    
    Slack Integration allows your team to be notified of events occuring in your Delivery Pipeline. To configure this integration, create an incoming webhook for your Slack team using the [Slack API](https://api.slack.com/incoming-webhooks). Once created, fill in the three fields listed under the Slack Integration.
    
    <img src="media/BluemixSlackIntegration.png" width="500">

4. Click the "Create" button in the bottom right area of the window to instruct IBM Cloud to create a toolchain for you using the defined settings.

    <img src="media/BluemixCreateButton.png" width="200">

5. The page that loads after clicking create is the application's Toolchain. Here is the overview of all of the integrated services, the flow of the project, and easy navigation to each of the individual components of the application.

    <img src="media/MicroservicesToolchain.png" width="500">

   **Note:** In this view, Slack was not configured in the previous step before clicking "Create". Slack can be configured at any time after creation of the Toolchain.

6. As soon as the "Create" button was pressed, IBM Cloud went to work deploying the application via the defined pipeline configurations. Click on the "Delivery Pipeline" box that starts with the words "spring-boot-continuous..." (shown below) in the Toolchain to view the code deployment pipeline of our primary application.

    <img src="media/DeliveryPipeline.png" width="200">

7. The pipeline page will dynamically update with the status of each step in the pipeline. When the deployment has finished running the bar on the top of the "Deploy Stage" will turn green.

In Progress                |  Finished
:-------------------------:|:-------------------------:
![](media/PipelineInProgress.png)  |  ![](media/PipelineFinished.png)

8. Once the "Deploy Stage" has turned green, indicating the job is complete, the link to the deployed application can be found in the mid-section of the "Deploy Stage" card.

    <img src="media/PipelineAppLink.png" width="300">

9. Upon clicking the application's URL, a new tab will open to the application's main page.

Create a New Account                |  Account Created
:-------------------------:|:-------------------------:
![](media/AccountImage1.png)  |  ![](media/AccountImage2.png)

### Admin Dashboard Microservice

10. Return to the Toolchain page and click on the other pipeline that has not yet been viewed ("admin-spring-boot...").

    <img src="media/AdminPipeline.png" width="200">
    
11. Once the pipeline page has loaded a similar set of deployment steps to our primary microservice can be found. Once the deployment has completed, click on the url listed in the middle of the "Deploy Stage" card. The URL will look similar to the one shown below.

    <img src="media/adminUrlExample.png" width="500">

12. After the application's webpage has loaded a simulated admin dashboard will show the names and account ids created from the other microservice's webpage. The admin and main application are both communicating with the same Cloudant DB instance in the backend but have no direct communication between each other.

Full Account List                |  Searching Accounts
:-------------------------:|:-------------------------:
![](media/AdminApp1.png)  |  ![](media/AdminApp2.png)

-----

## Cloudant DB

The following steps will show how to access the account that was just created in the Cloudant DB database

1. Return to [IBM Cloud](https://cloud.ibm.com/ "Welcome to IBM Cloud") and open the [Services Dashboard](https://console.ng.bluemix.net/dashboard).

    <img src="media/FindService2.png" width="200">

2. Once on the Services Dashboard, locate your Cloudant DB and click on it to open the service description page.

    <img src="media/FindService3.png" width="700">

3. Click on the "LAUNCH" button near the top right of the page to load into the Cloudant DB dashboard.

4. Once the Cloudant DB dashboard has finished loading, select the databases tab on the left.

    <img src="media/OpenDB2.png" width="200">

5. Click on the "Account" database. On the right you will see the account that was created from the web app above.

    <img src="media/DBEntry1.png" width="300">

6. Click on the database entry to view more details.

    <img src="media/DBEntry2.png" width="500">
    
# Updating Individual Microservices

One of the great things about how Toolchains can be configured is that it can have multiple stand alone pipelines and code repositories. The way this Toolchain has been configured sets up two independent GitHub repositories and then two independent delivery pipelines. What that means is if you commit a change to one repository it will deploy that change automatically into its associated pipeline but the other repository and pipeline will be completely unaffected.

To demonstrate this, choose either of the two GitHub repositories to edit. For our demonstration, we are going to edit the main application's code. For simplicity, we're going to edit the `index.jps` file via the GitHub web interface.

1. Navigate to your cloned version of the project on [GitHub.com](https://github.com).

    <img src="media/ClonedRepo.png" width="500">

2. Navigate to the `index.jsp` file: src -> main -> webapp -> index.jsp

    ```https://github.com/[username]/spring-boot-continuous-delivery-123456/blob/master/src/main/webapp/index.jsp```
    
    **Note:** The above example URL will be different depending on your GitHub username and the string of numbers assigned to your cloned version of the application.
    
3. Open the index.jsp file for editing:

    <img src="media/EditIndex.png" width="300">
    
4. Change the application's header to "Create a new account. Edited!". See the following for reference:

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Create a new account. Edited!</title>
</head>
<body>
    <h2>Create a new account</h2>
    <p>Enter your name and click submit to open a new account.</p>
    <form method="post" action="/account">
	    <label for="name">Name:</label>
	    <input type="text" name="name" id="name"><br><br>
	    <input type="submit" value="Submit">
    </form>
</body>
</html>
```

5. Save and commit the change to your index.jsp

    <img src="media/CommitFile.png" width="300">
    
6. Return to your deployment pipelines to view the deployment progress. The application may deploy quickly, if it is done deploying before you can navigate back to it you can view recent deployment logs to verify that only the primary application was deployed with the recent commit. Click [here](https://console.ng.bluemix.net/devops/toolchains/) to navigate back to the Toolchain overview dashboard.

    <img src="media/OtherPipelineUnaffected.png" width="1000">
    
7. Finally, open the primary application's webpage again to view the recently committed change.

    <img src="media/WebpageEdited.png" width="500">

-----

# Deployment Configuration

In this application we use several different files that each server an important part of customizing our IBM Cloud deployment. Below we will give a brief overview of each file's function.

## toolchain.yml

File location: [.bluemix/toolchain.yml](.bluemix/toolchain.yml)

The toolchain.yml file is used to define the desired application name, integrated services, descriptions, environment properties, and more. This file is what pulls in the pipeline.yml, deploy.json, and custom Toolchain image used in this application.

For our application we have integrated GitHub as a code repository as well as for issue tracking, Eclipse Orian Web IDE for editing code via the web interface, and a pipeline to deploy the application from the integrated GitHub repository.

## deploy.json

File location: [.bluemix/deploy.json](.bluemix/deploy.json)

The deploy.json file is what allows user customization of the Toolchain via the UI before the user clicks deploy but after the user has clicked the "Deploy to Bluemix" button from the GitHub repository.

For our application we have configured the deploy.son file to capture and require three fields that will be used as environment variables by the toolchain.yml: selected-region, selected-organization, and selected-space. We have created a three column form that allows users to select fields for each of the afore mentioned variables if the defaults are not desired. Finally, we have called a helper function to validate the input variables.

## pipeline.yml

File location: [.bluemix/pipeline.yml](.bluemix/pipeline.yml)

The pipeline.yml file is used to configure the IBM Cloud Pipeline for an application. A toolchain can have multiple pipeline files. One way to rename the pipeline.yml files is to add a prefix to the pipeline.yml file's name: example-pipeline.yml. In the pipeline.yml file we can specify things such as build steps, environment variables, custom scripts and instructions, etc.

For our application we have configured the pipeline.yml to build our maven project to a custom location and then deploy the built .war file from the custom location. Additionally, in our pipeline.yml file we have specified the application memory limit using the `cf` command:

```cf push "${CF_APP}" -p "springboot-demo.war" -m 256M```

-----

# Associated Repositories

One of the primary aspects of this lab is to demonstrate simplified deployment of microservices from multiple repositories. As part of the setup for the different microservices a unique deployment pipeline cis reated for and connected to each of the individual microservice's repositories. By creating a different pipeline for each repository we are enabling each service to have its code updated and re-deployed completely independent of the operation of the other microservice; that is to say, by having two pipelines we can update the code for one microservice and the other microservice will be completely unaffected and unaware anything happened.

- Admin Dashboard Microservice: [https://github.com/IBM/spring-boot-continuous-delivery-admin](https://github.com/IBM/spring-boot-continuous-delivery-admin)

   **Note:** No direct interaction with the associated repositories is required. All of the deployment interaction is done through the current repository.
   
-----

# Security Vulnerability Scanning

Easily scan your web application for security vulnerabilites using the [Application Security on Cloud](https://console.ng.bluemix.net/catalog/services/application-security-on-cloud) service on IBM Cloud.

1. From the IBM Cloud Catalog click **Security** from the left-side menu, or simply search for **Application Security on Cloud** in the search prompt.

    <img src="media/BluemixSecurityServices.png">
    
2. Click on the **Application Security on Cloud** service.
    
    <img src="media/AppSecurityOnCloud.png">

3. Choose a service name, or keep the pre-populated name. Click **Create**.
    
    <img src="media/AppSecurityCreate.png" width="700">

4. You will see three application options for scanning. Click **Web**.
    
    <img src="media/AppSecurityWebScan.png" width="700">

5. The next screen will ask you to choose a scan type. Click **Dynamic**.
    
    <img src="media/AppSecurityScanType.png" width="700">

6. For site location choose **Public Network**.
    
    <img src="media/AppSecurityNetwork.png" width="700">

7. You will then be asked for a site URL. Enter you Bluemix application URL and click **Continue**.
    
    <img src="media/AppSecurityURL.png" width="700">
    
    **Note**: You may be required to add a verification file to ensure your application URL is valid. If this occurs, place the `IBMDomainVerification.html` file in the `src/main/webapp/` directory of your cloned repo. Wait for the application to redeploy before scanning.

8. The next screen will contain various scan settings, leave these set to the defaults. Click **Scan**.
    
    <img src="media/AppSecurityScanSettings.png" width="700">
    
    Your security vulnerability scan will start running.
    
    <img src="media/AppSecurityScanInProgress.png" width="700">
    
    Once the scan is complete you will see the total number of vulnerabitites and the associated severity ratings.
    
    <img src="media/AppSecurityResults.png" width="700">

9. You can choose to download the detailed security report, and work to remediate the issues found.
    
    <img src="media/AppSecurityReport.png" width="700">

-----

# Deploying to Kubernetes
One deployment option is to deploy this application to kubernetes. For this option, we will provision a lite kubernetes cluster via the IBM Bluemix Container Service and deploy the service using the [manifests/deployment.yml](/manifests/deployment.yml) file.

#### Provision and configure IBM Bluemix Container Service
1) Log into the [IBM Cloud console](https://cloud.ibm.com) and create a lite [Kubernetes cluster](https://console.bluemix.net/containers-kubernetes/launch) named `mycluster`. This will take several minutes to provision. 
2. Create a Cloudant instance (if you haven't already) named `sample-java-cloudant-cloudantNoSQLDB`.
3. Install [kubectl](https://kubernetes.io/docs/tasks/tools/install-kubectl/). Kubectl is the standard way to interact with Kubernetes clusters. You will configure `kubectl` to point to your Kubernetes cluster hosted on the IBM Bluemix Container Service.
3. Download the [IBM Cloud CLI](https://console.bluemix.net/docs/cli/reference/bluemix_cli/index.html#install_bluemix_cli)
3. Make sure you login, then install the IBM Bluemix Container Service plug-in.
```sh
bx login # -sso if using single sign on
bx plugin install container-service -r Bluemix
bx plugin list # To verifiy the plugin has been installed
bx cs init # Initialize the container service plugin
```
6. List the clusters. You should see the cluster you created: `mycluster`. If you don't, make sure you have waited enough time for the cluster to be provisioned. You should see the cluster state as "Ready" in the IBM Cloud console.
```sh
bx cs clusters
```
7. Configure your `kubectl` to point to your Kubernetes cluster.
```sh
bx cs cluster-config mycluster
```
This should give you an `export KUBE_CONFIG` line. Copy and paste this to configure your `kubectl` to point to your Kubernetes cluster.
For example:
```
export KUBECONFIG=/Users/johnzaccone/.bluemix/plugins/container-service/clusters/mycluster/kube-config-par01-mycluster.yml
```
8. Verify that your `kubectl` is configured. The following should show: `mycluster`.
```sh
kubectl config get-clusters
```
You can now send commands to your Kubernetes cluster!

#### Bind the Cloudant Service
We will use the IBM Cloud CLI to create a binding between our Cloudant instance and our Kubernetes cluster.
1. List the services.
```sh
bx service list
```
2. Bind the Cloudant service.
```sh
bx cs cluster-service-bind mycluster default sample-java-cloudant-cloudantNoSQLDB
```
The command above loads a Kubernetes secret into your cluster that contains information to connect to your Cloudant instance. The application is configured to read this secret when it is deployed.

#### Deploy the Application
The [manifests/deployment.yml](manifests/deployment.yml) contains the Kubernetes defition to deploy the application. Deploy the application.
```sh
kubectl apply -f manifests
```
#### Test the Application
The deployment uses NodePort to expose the application on all nodes in the cluster. To access the application, first get IP of the worker node in the cluster.
```sh
kubectl get nodes
```
Then lists the services to get the port for your application.
```sh
kubectl get services
NAME                  CLUSTER-IP     EXTERNAL-IP   PORT(S)          AGE
spring-boot-service   10.10.10.177   <nodes>       9080:30052/TCP   20min
