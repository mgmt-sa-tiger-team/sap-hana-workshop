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

In this view we can see a number of applications provided by Insights. The major features we will examine today are:
1. Advisor
1. Vulnerability
1. Compliance
1. Patch
1. Drift

Applications 
============
Advisor 
-------
**Advisor** provides reccomendations to improve system performance, reliability and security in a number of formats.
  1. The first is simply known as Reccomendations. This view can be sorted and filtered to help you determine what changes you want to make where they are applicable, the rick and criticality and wheter or not the issues can be automatically remediated.
  ![cloud.redhat.com Home](images/3-insights-advisor-reccomendations-main.png)
  1. Advisor also has a Systems view which allows a different view for going stright to a specific system or group of systems, filtering on partial host names allows you to leverage naming convenstions such as all system with hostnames begining in **"sap"**.
  ![cloud.redhat.com Home](images/3-insights-advisor-systems.png)
  1. In this example we will focus on our file server which we can find by the first few characters of its hostname.
  ![cloud.redhat.com Home](images/3-insights-advisor-systems-single-system.png)
  1. By clicking on the hostname we can evaluate the reccomendations and make a decision on whetehr to take action either immediately or during a future time such as Change Control Window. In this case we can see that we have five reccomendation two of which can be addressed automatically by Ansible
  ![cloud.redhat.com Home](images/3-insights-advisor-reccomendations.png)
  1. We can select these two items and then click the **Remediate** button
 ![cloud.redhat.com Home](images/3-insights-advisor-remediation-1.png)
  1. This will take us a to a dialog where we will be prompted to either append to an exsiting Playbook or create a new one. In this case we will create a new playbook.
  ![cloud.redhat.com Home](images/3-insights-advisor-remediation-2.png)
  1. Clicking Next takes us to a dialog where we can choose to delay the reboot required for out TCP to a later time but sitll put the fix in place to away the reboot. In this case we will simply move a head with the reboot and proceed to create the playbook.
  ![cloud.redhat.com Home](images/3-insights-advisor-remediation-3.png)
  1. We will recieve confirmation fo the creation of the playbook and by clicking on the playbook name can go straight on a dialog which allow us to download the playbook for local use or to execute it directly from the Insights interface.
  ![cloud.redhat.com Home](images/3-insights-advisor-remediation-4.png)
  1. In the next panel we have the dialog which will allow us to execute the playbook as well as a warining that there will be a reboot and the opptortunity to turn off the reboot functionality, again putting the fix in place but delaying the reboot for a future time.
  ![cloud.redhat.com Home](images/3-insights-advisor-remediation-5.png)
  1. After clicking the **Execute playbook** button we will see a summary before the final confirmation to execute.
  ![cloud.redhat.com Home](images/3-insights-advisor-remediation-6.png)
  1. Once the execution commences we will be taken to a status page for the execution in progress.
  ![cloud.redhat.com Home](images/3-insights-advisor-remediation-7.png)
  1. We can view the status of the playbook execution here (will need to catch interim navigation screen shots.)
  ![cloud.redhat.com Home](images/3-insights-advisor-remediation-8b.png)
  1. We can view the playbook progress
  ![cloud.redhat.com Home](images/3-insights-advisor-remediation-8c.png)
  1. When execution completes we will be provided with the status
  ![cloud.redhat.com Home](images/3-insights-advisor-remediation-8.png)
  1. Now the system viewed through Advisor shows no reccomendations
  ![cloud.redhat.com Home](images/3-insights-advisor-remediation-9.png)
  1. Advisor also provides the ability to view information by Topics such as SAP
  ![cloud.redhat.com Home](images/3-insights-advisor-topics-1.png)
  1. Clicking into a Topic provides a view of issues which you may want to address which are specific to that area. In this case we can see an Important issue with a verly low risk of change which can be addressed with an Ansible playbook.
  ![cloud.redhat.com Home](images/3-insights-advisor-topics-2.png)
  1. Clicking into that issue we recive additional information as well as a list of systems that should be addressed.
  ![cloud.redhat.com Home](images/3-insights-advisor-topics-3.png)
  1. Clicking through one of the hostnames reveals the details of this issue as well as other issues apllicable to this system.
  ![cloud.redhat.com Home](images/3-insights-advisor-topics-4.png)
  1. For this demonstration we'll go back up a level using the back arrow and address just this issue on all four affected systems. We'll select all four systems and then click **Remediate** button
  ![cloud.redhat.com Home](images/3-insights-advisor-topics-5.png)
  1. In the next dialog we will choose to create a new playbook, provide a menaingful name and then click **Next**
  ![cloud.redhat.com Home](images/3-insights-advisor-topics-6.png)
  1. We are provided with a summarry of what the playbook will do, clarification that a reboot is not requires and we will click **Create**
  ![cloud.redhat.com Home](images/3-insights-advisor-topics-7.png)
  1. We will recieve a message that the playbook has been created and then we will click either on the link in the notification or on the **Remediations** button in the left navigation panel. In this instance we will click through the link in the notification.
  ![cloud.redhat.com Home](images/3-insights-advisor-topics-7.png)
  1. Next we will note the action(s) to be taken, if there were multiple actions in the playbook we could choose to remove one or more. To make the required changes we will click **Execute playbook**.

Vulnerability
-------------
The Vulnerability application provides a security focussed view of our subscribed systems as well as remediation capabilities for issues using the same capabilities as in Advisor. For Vulnerability and the remaining applications we will walk through, but not remediate, issues. Feel free to explore.

1. **CVEs** provides a vew of CVEs that affect any of our subscribed systems.
  1. The results can be sorted by a number of factors, in the case we sorted by **Severity**
  ![cloud.redhat.com Home](images/3-insights-vulnerability-cve-1.png)
  1. Clicking into the first vulnerability we recieve basic information on the vunlerability, the exposed systems and are able to remediate the issue with an Ansible playbook.
  ![cloud.redhat.com Home](images/3-insights-vulnerability-cve-2.png)
  1. Clicking through the **View in Red Hat CVE database** link takes us to a page with deeper information on the issue.
  ![cloud.redhat.com Home](images/3-insights-vulnerability-cve-3.png)
1. The **Systems** view allows us to filter and sort our systems to help us make decisions on where to focus our efforts.
  1. In this case we have sorted by the number of **Applicable CVEs**. We will drill into the system with the greatest number of vulnerabilities.
  ![cloud.redhat.com Home](images/3-insights-vulnerability-systems-1.png)
  1. In this view we can again sort, select and remediate the vulnerabilities.
  ![cloud.redhat.com Home](images/3-insights-vulnerability-systems-2.png)

Compliance
----------
The **Compliance** application provides a convenenient interface to leverage SCAP (Security Content Automation Protocol) to ensure our systems are compliant with various standard and custom policies. 
1. **Reports** allows us to view audit results
![cloud.redhat.com Home](images/3-compliance-reports-1.png)
  1. Drilling into a report tile allows us to see which systems are included in the report. Note that we have set a **Compliance threshold** of 90% which allows us to focus on more critically uncompliant systems and report improvement, potentially tightening up thresholds as we progress. Again we can select one of more systems and remediate issues with an Ansilbe playbook.
  ![cloud.redhat.com Home](images/3-compliance-reports-1.png)
1. **SCAP Policies** allows us to manage our policies.
![cloud.redhat.com Home](images/3-compliance-policies-1.png)
1. **Systems** allows us to review systems that have SCAP policies associated with them.
![cloud.redhat.com Home](images/3-compliance-systems-1.png)

Patch
-----






create and edit SCAP (Security Content Automation Protocol) Policies
-
- 
  
  
  

  
  
  
  
  
  




