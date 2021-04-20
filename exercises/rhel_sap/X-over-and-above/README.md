Over-and-above
=========================

Exercises below are supplementary only and optional. Not part of the regular workshop.

Challenge Exercise: Non-rolling Update
======================

Now that you've done the proper way of upgrading your HANA systems with zero-down time, you may be wondering what would happen if you upgrade both HANA systems at the same time? This is clearly not recommended for production environment but if you're upto the challenge and want to see how the environment behaves you can perform this exercise.

In this exercise you will run through the same job template as in the previous challenge exercise **Lab 5 - Test Unplanned Outage**. This time you want to run on both HANA systems in parallel.

**Hint**: Did you notice that we included a variable **job_percentage: 50**, this controls the 'serial' strategy on the rolling update playbook. What value would you need to change this to so that it runs on both (all) HANA systems at the same time?

What was your observation? Was there any interruption in **Lab 5 - Rolling OS Update Verify DB** job?

**Hint**: For this exercise, we included a step in the playbook to reboot the systems even no updates are required to simulate an interruption.

