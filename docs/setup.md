# One Time Setup

Here are the setup directions you have to perform one time for the [../provisioner](provisioner).

# Table Of Contents

- [Setup](#setup)
- [Tower Instructions](#tower-instructions)

# Setup

1. Install Ansible if you have not done so already. We recommend using virtual environments.
   https://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html#virtual-environments
   
   * **NOTE:** You can skip this step if you plan to use Ansible Tower. See the [Tower Instructions](#tower-instructions).

2. Create an Amazon AWS account.

3. Create an Access Key ID and Secret Access Key.  Save the ID and key for later.

  - New to AWS and not sure what this step means?  [Click here](aws-directions/AWSHELP.md)

3. Install `boto` and `boto3`as well as `netaddr` and `passlib`

        pip install boto boto3 netaddr passlib

  **Are you using Tower?**  [Tower Instructions](#tower-instructions)

5. Set your Access Key ID and Secret Access Key from Step 2 under ~/.aws/credentials

```
[root@centos ~]# cat ~/.aws/credentials
[default]
aws_access_key_id = ABCDEFGHIJKLMNOP
aws_secret_access_key = ABCDEFGHIJKLMNOP/ABCDEFGHIJKLMNOP
```

6. Download and copy the SAP binaries into an AWS s3 storage [Click here](sap-directions/SAP.md)

7. Clone the workshops repo:

If you haven't done so already make sure you have the repo cloned to the machine executing the playbook

        git clone https://github.com/mgmt-sa-tiger-team/sap-hana-workshop.git
        cd sap-hana-workshop/provisioner

# Tower Instructions

Are you using Red Hat Ansible Tower to provision Ansible Automation Workshops? (e.g. is your control node Ansible Tower?)  Make sure to use umask for the installation of boto3 on the control node.
https://docs.ansible.com/ansible-tower/latest/html/upgrade-migration-guide/virtualenv.html

```
[user@centos ~]$ sudo -i
[root@centos ~]# source /var/lib/awx/venv/ansible/bin/activate
[root@centos ~]# umask 0022
[root@centos ~]# pip install --upgrade boto3
[root@centos ~]# deactivate
```
