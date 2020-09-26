Zero downtime patching
=========================

Your company is now out of the critical phase experienced in the previous lab and it’s time for monthly patching.
Key requirement here is to perform patching without disrupting services running and automatically perform OS and application patches while rebooting and keeping the services up & running in HA configuration.

In a real world environment, you can also incorporate any manual steps that may be required into the workflow process, for example HW upgrade or fixes.


Overview
========

In this lab exercise, you will perform OS upgrade on HANA systems without any downtime. During the upgrade application server will be serving connection through the backup server.

This scenario will be achieved with rolling update where only 1 HANA instance (50%) will be updated and reboot at any given time, because you have HA configured in your environment, it will automatically switch to the other instance. Likewise when it moves to working on the 2nd system, 1st system will be booted up and operational to receive new connections while the 2nd system is being upgraded.

Logging into Tower
==================

Your Ansible Tower instance url and credentials were supplied to you on the page created for this workshop.

Run Rolling Update Workflow template
======================


Step 1:
-------

Select **TEMPLATES**

Step 2:
-------

Click the rocketship icon ![Add](images/at_launch_icon.png) for the
**Lab 5 - Rolling Update**

Step 3:
-------

There are two branches in the workflow:

**1:** verify: check user experience during the update (ensure connection to database at all times)

**2:** rolling update: update OS and kernel to the latest level and reboot one-at-a-time on both HANA systems

Right-click in each box as it's running and open in a new window to observe both jobs running. You can keep the windows side-by-side to see the execution details.

Step 4:
-------

After the upgrade is finished, you will see the 'rolling update' job complete, the other job may still be running until it reaches the time-out period to observe.


Challenge Exercise: Non-rolling Update
======================

Now that you've done the proper way of upgrading your HANA systems with zero-down time, you may be wondering what would happen if you upgrade both HANA systems at the same time? This is clearly not recommended for production environment but if you're upto the challenge and want to see how the environment behaves you can perform this exercise.

In this exercise you will run through the same job template as in the previous lab **Lab 5 - Rolling Update**. This time you want to run on all HANA systems in parallel.

**Hint**: Did you notice that we included a variable **job_percentage: 50**, this controls the 'serial' strategy on the rolling update playbook. What value would you need to change this to so that it runs on both (all) HANA systems at the same time?

What was your observation? Was there any interruption in **Lab 5 - Verify** job?

**Hint**: For this exercise, we included a step in the playbook to reboot the systems even no updates are required to simulate an interruption.

