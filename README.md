# Alerts in GCP

In Google Cloud Platform (GCP), alerts are notifications triggered by predefined conditions or events within your cloud resources. These conditions are typically set up to monitor the performance, health, or security of various GCP services such as virtual machines, databases, or storage buckets. When an alert condition is met, GCP can send notifications through various channels, including email, SMS, or integration with third-party alerting systems, allowing you to take immediate action to address issues and ensure the reliability and security of your cloud infrastructure. 

## Creation of Alerts in GCP using Jenkins

This repository contains Jenkinsfile which has script for creating alerts in GCP using Jenkins.

For using this template we have to update our <CREDENTIALS_ID> name in the following stage in  jenkinsfile for logging into GCloud:

`stage('Logging into Google Cloud and Get Access Token')`
            

Create a Jenkins pipeline using this repository and build that pipeline by passing parameters to it. Mention the High_CPU_Usage_Alert file containing alerts configurations.


![jenkins_pipeline](https://i.postimg.cc/j2cWP0ZH/Screenshot-from-2023-09-09-11-59-40.png)


Build the pipeline and it will create alerts in GCP.

To verify this move to:

GCP Project > Alerting > Policies
