# RHEL for SAP AWS training provisioner

The `github.com/mgmt-sa-tiger-team/sap-hana-workshop` contains an Ansible Playbook `provision_lab.yml`, which is an automated lab setup for RHEL for SAP training on AWS (Amazon Web Services).  Set the `workshop_type` variable below to provision the corresponding workshop.

| Workshop | Workshop Type Var   |
|---|---|
| Red Hat Enterprise Linux for SAP Workshop | `workshop_type: sap`  |

## Table Of Contents

* [RHEL for SAP AWS training provisioner](#rhel-for-sap-aws-training-provisioner)
* [Table Of Contents](#table-of-contents)
* [Requirements](#requirements)
* [Lab Setup](#lab-setup)
  * [One Time Setup](#one-time-setup)
  * [Setup (per workshop)](#setup-per-workshop)
  * [IBM Community Grid](#ibm-community-grid)
  * [Accessing student documentation and slides](#accessing-student-documentation-and-slides)
  * [Accessing instructor inventory](#accessing-instructor-inventory)
  * [DNS](#dns)
* [Lab Teardown](#lab-teardown)
* [Demos](#demos)
* [FAQ](#faq)
* [More info on what is happening](#more-info-on-what-is-happening)
* [Getting Help](#getting-help)

## Requirements

* This provisioner must be run with Ansible Engine v2.8.0 or higher.
* AWS Account (follow directions on one time setup below)

## Lab Setup

### One Time Setup

[For One Time Setup - click here](../docs/setup.md)

### Setup (per workshop)

* Define the following variables in a file passed in using `-e @extra_vars.yml`

```yaml
---
# region where the nodes will live
ec2_region: us-east-1

# name prefix for all the VMs
ec2_name_prefix: TESTWORKSHOP

# creates student_total of workbenches for the workshop
student_total: 2

# Set the right workshop type, like network, rhel or f5 (see above)
workshop_type: sap

#####OPTIONAL VARIABLES

# turn DNS on for control nodes, and set to type in valid_dns_type
dns_type: aws

# password for Ansible control node
admin_password: your_password123

# Sets the Route53 DNS zone to use for Amazon Web Services
workshop_dns_zone: example.com

# automatically installs Tower to control node
towerinstall: true

# if set to true, this will install sap for each student in advance
sapinstall: false

## if you want to use satellite specify here, make sure it's accessible by student nodes
# satellite_host: X.X.X.X

## alternatively specify rhn credentials to register with RHN
rhn_username: abc
rhn_password: somepassword
rhn_subscription: subscription name

```

If you want to license Ansible Tower you must copy a license called tower_license.json into this directory.  If you do not have a license already please request one using the [Workshop License Link](https://www.ansible.com/workshop-license).

For more extra_vars examples, look at the following:

* [sample-vars-sap.yml](sample_workshops/sample-vars-sap.yml) - example for the RHEL for SAP Workshop

* Run the playbook:

```bash
ansible-playbook provision_lab.yml -e @extra_vars.yml
```

* Login to the AWS EC2 console and you will see instances being created.  For example:

```yaml
testworkshop-student1-ansible
````

### Accessing student documentation and slides

* Exercises and instructor slides are hosted at [http://mgmt-sa-tiger-team.github.io/sap-hana-workshop](http://mgmt-sa-tiger-team.github.io/sap-hana-workshop)

* Workbench information is stored in two places after you provision:

  * in a local directory named after the workshop (e.g. testworkshop/instructor_inventory)
  * By default there will be a website `ec2_name_prefix.workshop_dns_zone` (e.g. `testworkshop.rhdemo.io`)

    * **NOTE:** It is possible to change the DNS domain (right now this is only supported via a AWS Route 53 Hosted Zone) using the parameter `workshop_dns_zone` in your `extra_vars.yml` file.
    * **NOTE:** The playbook does not create the route53 zone and must exist prior to running the playbook.

### Accessing instructor inventory

* The instructor inventory will be copied to `/tmp` on student1's control_node as part of the control_nodes role.
* The instructor can see all assigned students and what their workbench is by visiting `ec2_name_prefix.workshop_dns_zone/list.php` (e.g. `testworkshop.rhdemo.io/list.php`)

### DNS

The provisioner currently supports creating DNS records per control node with valid SSL certs using [Lets Encrypt](https://letsencrypt.org/).  Right now DNS is only supported via AWS Route 53, however we are building it in a way that this can be more pluggable and take advantage of other public clouds.

This means that each student workbench will get an individual DNS entry.  For example a DNS name will look like this: `https://student1.testworkshop.rhdemo.io`

* **NOTE:** The variable `dns_type` defaults to `aws`.  This can also be set to `dns_type: none`.
* **NOTE:**  If Lets Encrypt fails, the workshop provisioner will still pass, and alert you of errors in the `summary_information` at the end of the `provision_lab.yml` Ansible Playbook.

## Lab Teardown

The `teardown_lab.yml` playbook deletes all the training instances as well as local inventory files.

To destroy all the EC2 instances after training is complete:

* Run the playbook:

```bash
ansible-playbook teardown_lab.yml -e @extra_vars.yml
```

* Optionally you can enable verbose debug output of the information gathered that drives the teardown process by passing the extra optional variable `debug_teardown=true`. Example:

```bash
ansible-playbook teardown_lab.yml -e @extra_vars.yml -e debug_teardown=true
```

Which will install all demos onto the Ansible Tower instance.  Not all demos will work on any `workshop_type`.  Please refer to the [Demo repository list](https://github.com/ansible/product-demos#demo-repository).

## FAQ

For frequently asked questions see the [FAQ](../docs/faq.md)

## More info on what is happening

The `provision_lab.yml` playbook creates a work bench for each student, configures them for password authentication, and creates an inventory file for each user with their IPs and credentials. An instructor inventory file is also created in the current directory which will let the instructor access the nodes of any student.  This file will be called `instructor_inventory.txt`

What does the AWS provisioner take care of automatically?

* AWS VPC creation (Amazon WebServices Virtual Private Cloud)
* Creation of an SSH key pair (stored at ./WORKSHOPNAME/WORKSHOPNAME-private.pem)
* Creation of a AWS EC2 security group
* Creation of a subnet for the VPC
* Creation of an internet gateway for the VPC
* Creation of route table for VPC (for reachability from internet)

## Getting Help

Please [file issues on Github](https://github.com/mgmt-sa-tiger-team/sap-hana-workshop/issues).  Please fill out all required information.  Your issue will be closed if you skip required information in the Github issues template.
