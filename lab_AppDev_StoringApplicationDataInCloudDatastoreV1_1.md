# Overview
In this lab, you will review the case study application, an online Quiz. You will store application data for the Quiz application in Cloud Datastore.

The Quiz application skeleton has already been written for you. You will clone a repository that contains the skeleton using Google Cloud Shell, 
review the code using the Cloud Shell editor, and view it using the Cloud Shell web preview feature.

Then you will modify the code that stores data to use Cloud Datastore.

# Objectives

In this lab, you will learn how to perform the following tasks:

Harness Cloud Shell as your development environment.

Integrate Cloud Datastore into a NodeJS application.

# Setup
GCP Console Ressources 
#####################################

Username : student-02-fe67c16402f6@qwiklabs.net

Password : p8bWT5Lkp4K

GCP Project ID : qwiklabs-gcp-02-bc332d3234bc


#####################################

# Previewing the Case Study Application
In this section, you will access Cloud Shell, clone the git repository containing the Quiz application, and run the application.

- **Clone source code in Cloud Shell**

1. On the Cloud Platform Console, click Activate Google Cloud Shell. **(>_)**

2. If a dialog box appears, click Start Cloud Shell.

3. To clone the repository for the class, execute the following command:
> git clone https://github.com/GoogleCloudPlatform/training-data-analyst

- **Configure and run the case study application**
1. To change the working directory, execute the following command:
> cd ~/training-data-analyst/courses/developingapps/nodejs/datastore/start

2. To export an environment variable, GCLOUD_PROJECT that references the GCP Project ID, execute the following command:
> export GCLOUD_PROJECT=$DEVSHELL_PROJECT_ID

**Note:** GCP Project ID in Cloud Shell
While working in Cloud Shell, you will have access to the Project ID in the $DEVSHELL_PROJECT_ID environment variable.

3. To install the application dependencies, execute the following command:
> npm install
__The installation may take a couple of minutes.__

4. To run the application, execute the following command:
> npm start

__Review the case study application__

1. In **Cloud Shell**, click **Web preview > Preview on port 8080** to preview the quiz application.

> You should see the user interface for the web application. The three main parts to the application are:

>> 1. Create Question
>> 2. Take Test
>> 3. Leaderboard

2. In the navigation bar, click __Create Question.__



