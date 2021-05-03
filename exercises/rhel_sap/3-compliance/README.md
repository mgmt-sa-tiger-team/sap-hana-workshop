Evaluate SAP Environment with Red Hat Insights
==============================================

**NOTES:** This lab is currently based on RHEL 8.1 which is the most recent minor release certified by SAP as of 4/20/2021. RHEL 8.4 includes the ability to remediate Insights findings from https://cloud.redhat.com/ however this is not the case for 8.1. If you are not using a Satellite in this lab you will not be able to automate remediation though you will still be able to review findings and download playbooks to effect remediation manually.

Your HANA environment is up and running. What can you do to improve it?

Red Hat Insights is a component of RHEL that will help you detect and address
- security issues (not just patching) and help to avoid breaches that cost money and erode your brand value
- compliance issues and help to avoid violations that cost money and erode your brand value
- performance and availability issues that cost money and erode internal and external confidence
- consistency issues that reduce efficiency and cost money
and do all of the above consistently, mostly automatically, using version controlled artifacts with very little effort which saves money and builds internal and external confidence

Overview
========

In this lab exercise we will walk through the process of evaluating your SAP environment, due to the requirement for live subscriptions this lab will be conducted as a demonstration rather than an interactive lab. Your servers were automatically registered to Insights during the deployment process. We'll cover the approach using Satellite first

Logging into Satellite
======================

Open a browser window or tab, navigate to the Satellite instance provided in your Workbench Information and enter your credentials.

![Satellite Dashboard](images/3-lab-satellite-dashboard.png)

Hover your mouse of **Insights** on the right hand navigation menu and then up to **Actions**

![Navigate to Insights Actions](images/3-lab-navigate-to-insights-actions.png)

From this view

![Insights Actions 1](images/3-lab-insights-actions-1.png)

scroll down until you see the **SAP** topic and click on **View More**

![Insights Actions 2](images/3-lab-insights-actions-2.png)

In this view we can see issues that Insights has identified issues which are specific to SAP. In particular we'll take a look at the issue in resource limits. Click on the text highlighted in this example.

![Resource limits 1](images/3-lab-insights-resource-limits-1.png)

Details of the issue can be found by clicking through the link for one of the affected hosts. In the view below we can see that Insights has detected that we have not set our limits for the number of open files as high as is required.

![Resource limits 1a](images/3-lab-insights-resource-limits-1a.png)

From the next view we can see which systems are affected by this issue. Select the VMs that you own in your view then click on **Actions** and **Create a new Plan/Playbook**

![Resource limits 2](images/3-lab-insights-resource-limits-2.png)

Enter a name for your plan and click **Save**

![Resource limits 3](images/3-lab-insights-resource-limits-3.png)

We have two options here, one that involves a reboot and one that does not. Given that this is an SAP HANA clustered environment where a reboot must be carefully coordinated we're going to go with the non-reboot approach and click **Save**.

![Resource limits 4](images/3-lab-insights-resource-limits-4.png)

This will take us to the screen below where we can simply click on the **Run Playbook** button.

![Resource limits 5](images/3-lab-insights-resource-limits-5.png)

This takes us to a status screen where we can keep and eye on progres.

![Resource limits 6](images/3-lab-insights-resource-limits-6.png)

When the playbook completes we have a status indicating the percentages of successes as well as any failures which we need to follow up on. We can also drill down to individual hosts for more details.

![Resource limits 7](images/3-lab-insights-resource-limits-7.png)

Clicking on one of the hosts we've been working with (yours will be named differently) we can see the output of the playbook. Note the changes made hightlighted in red.

![Resource limits 8](images/3-lab-insights-resource-limits-8.png)

While not necessary we could also verify the status manually by running a remote job on one or all of the hosts. To do that here we'll hover over **Hosts** in the left navigation bar and then click on **All Hosts**

![Resource limits 9](images/3-lab-insights-resource-limits-9.png)

In the next panel we select one of more of the hosts that we've updated (depending on the number of students in your class you may want to use the search bar as we've done here) then click on **Select Action** and from the pull down menu select ** Schedule Remote Job**.

![Resource limits 10](images/3-lab-insights-resource-limits-10.png)

In the next screen we can see that the host name has already been filled in. We will enter the command "cat /etc/security/limits.d/99-sap.conf" to verify that the modifications have been made and then click **Submit**

![Resource limits 11](images/3-lab-insights-resource-limits-11.png)

This will take you to a window where we can follow the progress of this ad hoc job.

![Resource limits 12](images/3-lab-insights-resource-limits-12.png)

Once completed we can again click on the host name to see the output of the job.

![Resource limits 13](images/3-lab-insights-resource-limits-13.png)


Logging into cloud.redhat.com
=============================

Open a browser window or tab and navigate to https://cloud.redhat.com/ and log in with your credentials


![cloud.redhat.com Home](images/3-lab-cloud-home.png)


Your initial dashboard will look something like this one


![Insights Dashboard](images/3-insights-dashboard.png)

In this view we can see a number of applications provided by Insights. The major features we will examine today are:
1. Advisor
1. Vulnerability
1. Compliance
1. Patch
1. Drift

Applications 
===========
Advisor 
-------
**Advisor** provides recommendations to improve system performance, reliability and security in a number of formats.
  1. The first is simply known as Recommendations. This view can be sorted and filtered to help you determine what changes you want to make where they are applicable, the rick and criticality and whether or not the issues can be automatically remediated.
  ![Insights Advisor Recommendations Main](images/3-insights-advisor-reccomendations-main.png)
  1. Advisor also has a Systems view which allows a different view for going straight to a specific system or group of systems, filtering on partial host names allows you to leverage naming conventions such as all system with hostnames beginning in **"sap"**.
  ![Advisor Systems](images/3-insights-advisor-systems.png)
  1. In this example we will focus on our file server which we can find by the first few characters of its hostname.
  ![Advisor Systems Single System](images/3-insights-advisor-systems-single-system.png)
  1. By clicking on the hostname we can evaluate the recommendations and make a decision on whether to take action either immediately or during a future time such as Change Control Window. In this case we can see that we have five recommendation two of which can be addressed automatically by Ansible
  ![Advisor Recommendations](images/3-insights-advisor-reccomendations.png)
  1. We can select these two items and then click the **Remediate** button
 ![Advisor Remediation](images/3-insights-advisor-remediation-1.png)
  1. This will take us to a dialog where we will be prompted to either append to an existing Playbook or create a new one. In this case we will create a new playbook.
  ![Advisor Remediation 2](images/3-insights-advisor-remediation-2.png)
  1. Clicking Next takes us to a dialog where we can choose to delay the reboot required for out TCP to a later time but still put the fix in place to away the reboot. In this case we will simply move ahead with the reboot and proceed to create the playbook.
  ![Advisor Remediation 3](images/3-insights-advisor-remediation-3.png)
  1. We will receive confirmation of the creation of the playbook and by clicking on the playbook name can go straight on a dialog which allows us to download the playbook for local use or to execute it directly from the Insights interface.
  ![Advisor Remediation 4](images/3-insights-advisor-remediation-4.png)
  1. In the next panel we have the dialog which will allow us to execute the playbook as well as a warning that there will be a reboot and the opportunity to turn off the reboot functionality, again putting the fix in place but delaying the reboot for a future time.
  ![Advisor Remediation 5](images/3-insights-advisor-remediation-5.png)
  1. After clicking the **Execute playbook** button we will see a summary before the final confirmation to execute.
  ![Advisor Remediation 6](images/3-insights-advisor-remediation-6.png)
  1. Once the execution commences we will be taken to a status page for the execution in progress.
  ![Advisor Remediation 7](images/3-insights-advisor-remediation-7.png)
  1. We can view the status of the playbook execution here (will need to catch interim navigation screen shots.)
  ![Advisor Remediation 8b](images/3-insights-advisor-remediation-8b.png)
  1. We can view the playbook progress
  ![Advisor Remediation 8c](images/3-insights-advisor-remediation-8c.png)
  1. When execution completes we will be provided with the status
  ![Advisor Remediation 8](images/3-insights-advisor-remediation-8.png)
  1. Now the system viewed through Advisor shows no recommendations
  ![Advisor Remediation 9](images/3-insights-advisor-remediation-9.png)
  1. Advisor also provides the ability to view information by Topics such as SAP. The Topics section represents specialized areas of expertise where Red Hat and our partners, such as SAP, provide expert guidance on recommended practice and tuning with references to both Red Hat and partner documentation such as SAP Rules.
  ![Advisor Topics 1](images/3-insights-advisor-topics-1.png)
  1. Clicking into a Topic provides a view of issues which you may want to address which are specific to that area. In this case we can see an Important issue with a very low risk of change which can be addressed with an Ansible playbook.
  ![Advisor Topics 2](images/3-insights-advisor-topics-2.png)
  1. Clicking into that issue we receive additional information as well as a list of systems that should be addressed.
  ![Advisor Topics 3](images/3-insights-advisor-topics-3.png)
  1. Clicking through one of the hostnames reveals the details of this issue as well as other issues applicable to this system.
  ![Advisor Topics 4](images/3-insights-advisor-topics-4.png)
  1. For this demonstration we'll go back up a level using the back arrow and address just this issue on all four affected systems. We'll select all four systems and then click **Remediate** button
  ![Advisor Topics 5](images/3-insights-advisor-topics-5.png)
  1. In the next dialog we will choose to create a new playbook, provide a meaningful name and then click **Next**
  ![Advisor Topics 6](images/3-insights-advisor-topics-6.png)
  1. We are provided with a summary of what the playbook will do, clarification that a reboot is not requires and we will click **Create**
  ![Advisor Topics 7](images/3-insights-advisor-topics-7.png)
  1. We will receive a message that the playbook has been created and then we will click either on the link in the notification or on the **Remediations** button in the left navigation panel. In this instance we will click through the link in the notification.
  ![Advisor Topics 8](images/3-insights-advisor-topics-8.png)
  1. Next we will note the action(s) to be taken, if there were multiple actions in the playbook we could choose to remove one or more. To make the required changes we will click **Execute playbook**.

Vulnerability
-------------
The Vulnerability application provides a security focussed view of our subscribed systems as well as remediation capabilities for issues using the same capabilities as in Advisor. For Vulnerability and the remaining applications we will walk through, but not remediate, issues. Feel free to explore.

1. **CVEs** provides a view of CVEs that affect any of our subscribed systems.
  1. The results can be sorted by a number of factors, in the case we sorted by **Severity**
  ![Vulnerability CVE 1](images/3-insights-vulnerability-cve-1.png)
  1. Clicking into the first vulnerability we receive basic information on the vulnerability, the exposed systems and are able to remediate the issue with an Ansible playbook.
  ![Vulnerability CVE 2](images/3-insights-vulnerability-cve-2.png)
  1. Clicking through the **View in Red Hat CVE database** link takes us to a page with deeper information on the issue.
  ![Vulnerability CVE 3](images/3-insights-vulnerability-cve-3.png)
1. The **Systems** view allows us to filter and sort our systems to help us make decisions on where to focus our efforts.
  1. In this case we have sorted by the number of **Applicable CVEs**. We will drill into the system with the greatest number of vulnerabilities.
  ![Vulnerability Systems 1](images/3-insights-vulnerability-systems-1.png)
  1. In this view we can again sort, select and remediate the vulnerabilities.
  ![Vulnerability Systems 2](images/3-insights-vulnerability-systems-2.png)

Compliance
----------
The **Compliance** application provides a convenient interface to leverage SCAP (Security Content Automation Protocol) to ensure our systems are compliant with various standard and custom policies. 
1. **Reports** allows us to view audit results
![Compliance Reports 1](images/3-insights-compliance-reports-1.png)
  1. Drilling into a report tile allows us to see which systems are included in the report. Note that we have set a **Compliance threshold** of 90% which allows us to focus on more critically non-compliant systems and report improvement, potentially tightening up thresholds as we progress. Again we can select one of more systems and remediate issues with an Ansible playbook.
  ![Compliance Reports 2](images/3-insights-compliance-reports-2.png)
1. **SCAP Policies** allows us to manage our policies.
![Compliance Policies 1](images/3-insights-compliance-policies-1.png)
1. **Systems** allows us to review systems that have SCAP policies associated with them.
![Compliance Systems 1](images/3-insights-compliance-systems-1.png)

Patch
-----
The **Patch** application allows viewing all Red Hat errata whether for Security, Bugfix  or Enhancement.
1. The **Advisories** view allows us to search and sort based on characteristics of the patch
![Patch Advisories 1](images/3-insights-patch-advisories-1.png)
1. The **Systems** view allows viewing all subscribed systems which have patches available and sorting on system name, the number of applicable advisories or the last time the system checked in.
![Patch Systems 1](images/3-insights-patch-systems-1.png)
  1. Clicking on a system name will allow us to sort applicable patches by name, publish date, type and synopsis. We can also select one or more patches and create an Ansible playbook to perform remediation.
  ![Patch Systems 1](images/3-insights-patch-systems-2.png)
  
Drift
-----
The **Drift** application allows us to compare two or more systems to each other or to an established baseline
![Drift Comparison 1](images/3-insights-drift-comparison-1.png)
1. Clicking the **Add to comparison** button brings up a dialog to allow us to add systems we want to compare. Once all systems are selected we click **Submit**
![Drift Comparison 2](images/3-insights-drift-comparison-2.png)
1. In this view we can see a comparison of three SAP servers and the fact that there is a great deal of disparity between them.
![Drift Comparison 3](images/3-insights-drift-comparison-3.png)

Conclusion
==========

Red Hat Insights provides unparalleled system optimization based over a quarter of a century of supporting Linux in an Enterprise environment and incorporating expert guidance from our hardware and software partners, including **SAP**.
