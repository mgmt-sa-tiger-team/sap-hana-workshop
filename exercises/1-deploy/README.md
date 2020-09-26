Deploying RHEL SAP HANA Environment
=========================

We have a requirement for a new two node HANA Cluster and the virtual hardware has been provisioned. Each participant connects to a hosted environment which includes

- Workstation
- Satellite
- Tower

In addition to these systems, you will provision a new RHEL 8.1 based SAP HANA environment:

- 2 SAP HANA VMs in an HA config
- 1 SAP App Server VM

![Lab Architecture](images/1-lab-arch.png)

Ansible playbooks and workflow templates are provided to automatically provision these systems onto your student environment. As part of this process:

- VMs will be provisioned from a template via Satellite, this will automatically create the objects needed to manage the RHEL subscriptions for the new systems on the Satellite systems.
- Attach the correct subscription on Satellite
- Mount binary packages and other prerequisite steps to prepare the environment for a succesfull SAP deployment

Overview
========

In this lab exercise, you will log into Tower and request a new SAP HANA environment and validate it's correctly configured.

Logging into Tower
==================

Your Ansible Tower instance url and credentials were supplied to you on the page created for this workshop.

Provision New RHEL SAP Environment
======================

For your lab environment, there are multiple job templates created that will help you automate the deployment of a SAP environment:

- Lab 1 - Deploy VM via SAT (deploy multiple VMs in the landscape)
- Lab 1 - Wait for IP (ensure VMs are fully up and running)
- Lab 1 - Initial Setup (attach SAP subscription and other prereqs)

Additionally a Workflow template **Lab 1 - Deploy Environment** is provided for a push button deployment of the entire landscape.


First, run **Lab 1 - Deploy Environment** workflow template to identify any vulnerable systems.

Step 1:
-------

Select **TEMPLATES**

Step 2:
-------

Click the rocketship icon ![Add](images/at_launch_icon.png) for the
**ab 1 - Deploy Environment**

Step 3:
-------

When prompted, in **Other Prompts** tab, enter sap3vm* (make sure you type '*' at the end as this will match multiple VMs)

Select **NEXT**.

Step 4:
-------

When prompted, in **Survey** tab, enter sap3vm this time.

Select **NEXT** and preview the inputs.

Step 5:
-------

Select LAUNCH ![SurveyL](images/1-survey-launch.png)

Step 6:
-------

Sit back, watch the magic happen

One of the first things you will notice is the summary section. This
gives you details about your job such as who launched it, what job template
it’s running, what the status is, i.e. pending, running, or complete.

![Workflow Summary](images/1-wf-summary-details.png)


Step 7:
-------

When the workflow job has successfully completed, you should have 3 systems in your inventory and ready for further automation.

Validating Inventory
======================

In this section, you will validate the inventory that contains newly provisioned VMS.

Step 1:
-------

After running the workflow template, results will be now reflected in the inventory **satellite_inventory**.

Navigate to **INVENTORIES**

Step 2:
-------

Click **satellite_inventory**.

Step 3:
-------

Click **GROUPS** tab and select **foreman_content_view_rhelsaphanabase**.

Step 4:
-------

Review the newly provisioned systems, you can click on one of them and review the variables discovered from Satellite dynamically, these variables will be available for any automation as well as via REST API to any external system.

![Review Host](images/1-inventory-host-1-review.png)

Validating Deployment
======================

In this section, you will execute an ad-hoc command on your environment to validate the subscription status.

Step 1:
-------

After running the workflow template, results will be now reflected in the inventory **satellite_inventory**.

Navigate to **INVENTORIES**

Step 2:
-------

Click **satellite_inventory**.

Step 3:
-------

Click **GROUPS** tab and select **foreman_content_view_rhelsaphanabase**.


Step 4:
-------

Select each of the VMS by clicking the checkbox next to it. You will then see the **RUN COMMANDS**
button become enabled. Click it now.

![Review Host](images/1-ad-hoc-select-hosts.png)

This will pop up the **Execute Command** window. From here is where we
can run a single task against our hosts.

Fill out this form as follows

| Key                | Value           | Note                                                            |
|--------------------|-----------------|-----------------------------------------------------------------|
| Module             | `command`      |                                                                 |
| Arguments          |                 | subscription-manager status                                           |
| Limit              |                 | This will be pre-filled out for you with the hosts you selected |
| MACHINE CREDENTIAL | *Default*       | Select default credential provided                                                               |

![Run Command](images/1-adhoc-run-command.png)

Once you click **LAUNCH** you will be redirected to the Job log. Every
job and action in Ansible Tower is recorded and stored. These logs can
be auto-rotated and can also be exported automatically to another
logging system such as Splunk or ELK.

The first part of the log shows you the details of the job. This
includes information such as who launched the job, against what hosts,
and when.

![Command Log Details](images/1-adhoc-run-command-success.png)

The second part of the job log shows you the actual output from the
command. If your connection was successful, you should see a result such
as this.

![Command Log Details](images/1-adhoc-run-command-output.png)

The results should return for all 3 systems:

**Overall Status: Current**

This validated the environment, feel free to repeat the steps above and experiment further by running additional commands.