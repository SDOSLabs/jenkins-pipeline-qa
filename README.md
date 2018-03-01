# jenkins-pipeline-qa
Project created by the QA department to provide a simple pipeline to run your Appium Test using Jenkins in Android. 
_______________________________________

## 1. Requirements:

The parameters that you need to run this project are the following:

- __JOB_GIT_URL:__ Code Repository URL of the Automated Test.
- __JOB_GIT_BRANCH:__ Branch to download the code from.
- __JOB_APK_NAME:__ Application Name to launch on the emulator. 
- __JOB_EMULATOR_VERSION:__ Android Emulator API Version to launch. On our system we have created 3 emulators: emulator_25, emulator_26, emulator_27. The pipeline will open the emulador_+"JOB_EMULATOR_VERSION" selected.
- __JOB_EMULATOR_PLATFORM_VERSION:__ Android Emulator Version needed for the Appium Core tu run (Appium Capability)
- __JOB_PLATFORM_NAME:__ Device Platform Name to launch the tests.(Android by default).
- __JOB_APPIUM_SUITE:__ Name of the Test Suite to execute
- __JOB_SLACK_CHANNEL_NOTIFICATION:__ Name of the Slack channel (from the SDOS Slack) to notify the result of the job execution.

These parameters are the needed capabilities to be able to run the Appium Test along with the Appium Core of SDOS.

## 2. Stages:

- __1.__ Initial Configuration
- __2.__ Download GIT Code
- __3.__ Execute ADB Server
- __4.__ Launch Android Emulador
- __5.__ Run Appium Test
- __6.__ SonarQube
- __7.__ Post Build actions

_______________________________________

This project is licensed under the terms of the Apache license.