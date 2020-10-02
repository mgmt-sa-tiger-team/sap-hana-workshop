Evaluate SAP Environment with Red Hat Insights
==============================================

Your HANA environment is up and running. What can you do to improve it?

Red Hat Insights is a component of RHEL that will help you detect and address 
- security issues (not just patching) and help to avoid breaches that cost money and erode your brand value 
- compliance issues and help to avoid violations that cost money and erode yur brand value 
- performance and availability issues that cost money and erode internal and external confidence
- consistency issues that reduce efficiency and cost money
do all of the above consistently, mostly automatically, with very little effort which saves money and builds internal and external confidence

Overview
========

In this lab exercise we will walk through the process of evaluating your SAP environment, due to the requirment for live subscriptions this lab will be conducted as a demonstration rahter than an interactive lab. Your servers were automatically registered to Insights duirng the deployment process. 

Logging into cloud.redhat.com
=============================

Open a browser and navigate to https://cloud.redhat.com/ and log in with your credentails

![cloud.redhat.com Home](images/3-lab-cloud-home.png)

Your initial dashboard will look something like this one

![cloud.redhat.com Home](images/3-insights-dashboard.png)

Insights Advisor 
================

Insights Advisor Provides reccomendations to improve system performance, reliability and security in a number of formats.

The first is simply known as Reccomendations. This view can be sorted and filtered to help you determine what changes you want to make where they are applicable, the rick and criticality and wheter or not the issues can be automatically remediated.

![cloud.redhat.com Home](images/3-insights-advisor-reccomendations-main.png)

Advisor also has a Systems view which allows a different view for going stright to a specific system or group of systems, filtering on partial host names allows you to leverage naming convenstions such as all system with hostnames begining in "sap".

![cloud.redhat.com Home](images/3-insights-advisor-systems.png)

In this example we will focus on our file server which we can find by the first few characters of its hostname.

![cloud.redhat.com Home](images/3-insights-advisor-systems-single-system.png)



