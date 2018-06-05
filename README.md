# jenkins-pipeline-qa
Project created by the QA department to provide a simple pipeline to run your Appium Test using Jenkins in Android and iOS devices. 
_______________________________________

## 1. Requirements.

The parameters that you need to run this project are the following:

### Define Global environment variables on Jenkins.

To be able to execute the pipeline in any system you have configured your Jenkins, you need to define several environment variables on the global configuration in Jenkins. 

Fist, go to __Manage Jenkins__ -> __Configure System__ -> and on the __Global properties__ section, add the following environment variables:

-__ANDROID_HOME:__ Directory where the Android SDK is located.
-__GIT_CREDENTIAL:__ Git API Access Tokens. This key is generated from the User Settings.
-__JOB_DIRECTORY:__ Directory where the Project Files are located. 

### Variables needed on the Initial Configuration.

Once we had this environment variables set, we need to define several variables inside the pipeline, some of them will use the environment variables we defined on Jenkins.

-__JOB_FILES_DIRECTORY:__ Directory where the Project Files are located. We will use the JOB_DIRECTORY environment variable defined on Jenkins. 
-__PLATFORM_TOOL_DIRECTORY:__ Directory where the Platform Tools is located. We will use the ANDROID_HOME environment variable defined on Jenkins. 
-__EMULATOR_DIRECTORY:__ Directory where the Android Emulator is located. We will use the ANDROID_HOME environment variable defined on Jenkins. 
-__SUITE_PATH:__ Path of the Suite to execute

### Variables needed on the Job Configuration.

When we created a new job in Jenkins to execute our Appium Test using this pipeline, we need to create several parameters needed for the test to run. To define this set of parameter, we need to create a new job in Jenkins, and, on the __General section__, click on the option: _"This project is parameterized"_. The parameters we need to create are:  

- __JOB_GIT_URL:__ Code Repository URL of the Automated Test. _String parameter_.
- __JOB_GIT_BRANCH:__ Branch to download the code from. _String parameter_.
- __JOB_APP_NAME:__ Application Name to launch on the emulator. If the default value is empty, the app to test will be the one you set on the POM.xml. _String parameter_.
- __JOB_DEVICE_NAME:__ Emulator (already created on your system) to launch. _Choice paremeter_.
- __JOB_EMULATOR_PLATFORM_VERSION:__ Emulator Version needed for the Appium Test to run (Appium Capability). _Choice parameter_.
- __JOB_PLATFORM_NAME:__ Device Platform Name to launch the test: _android or ios_. _String parameter_
- __JOB_APPIUM_SUITE:__ Name of the Test Suite to execute. _Choice parameter_.
- __JOB_SLACK_CHANNEL_NOTIFICATION:__ Name of the Slack channel (from the your team Slack) to notify the result of the job execution. You need to previously configure Slack on the Manage Jenkins -> Configure System -> Global Slack Notifier Settings section in order to use it. _String parameter_.

These parameters are the needed capabilities to be able to run the Appium Test along with the Appium Core of SDOS.

Once you have all the parameters needed on your job, you just need to add on the Pipeline section, the URL of this repository and set the Script Path to the jenkinsfile on this repository.

## 2. Stages.

- __1. Initial Configuration:__ Stage to set the configuration we needed to run the test. Set several variables such as theh JOB_FILES_DIRECTORY or the SUITE_PATH. Also we clean the workspace before start. 
- __2. Download GIT Code:__ Stage to download the code from your repository using the JOB_GIT_URL and JOB_GIT_BRANCH parameters you set on Jenkins. 
- __3. Execute ADB Server:__ This stage is executed only if the JOB_PLATFORM_NAME parameter is Android. On this stage, we start the ADB server. 
- __4. Launch Android Emulador:__ This stage is executed only if the JOB_PLATFORM_NAME parameter is Android. On this stage, we launch the emulator we set on the JOB_DEVICE_NAME on Jenkins. The emulator should be created before execute this pipeline. 
- __5. Run Appium Test:__ On this stage, we run the Appium test with the configuration we set on the Jenkins parameters. First, the pipeline check if the app name is defined (JOB_APP_NAME) or not on the Jenkins parameter. If the JOB_APP_NAME is not set, the app to execute should be defined on the POM.xml. If the execution is success, we published the result using Junit Report Plugin, if the execution is not success (failed or unstable), we archive the screenshot of the failed test and then publish the execution test result using Junit Report Plugin. 
- __6. SonarQube:__ This stage use a Sonar Server configuration. In order to be able to see your Sonar analysis result, you need to configure your Sonar Server on: Manage Jenkins -> Configure System -> SonarQube servers section. 
- __7. Post Build actions:__ On this stage, the build execution result will be published on the Slack channel you defined on JOB_SLACK_CHANNEL_NOTIFICATION with a specific message of the execution result. 

## 3. More Information.

If you want to read more information about declarative pipelines and our way to integrate it on Jenkins, you can read the following article published on the SDOS Blog: https://sdos.es/integracion-continua-pasa-por-pipelines/
_______________________________________

This project is licensed under the terms of the Apache license.