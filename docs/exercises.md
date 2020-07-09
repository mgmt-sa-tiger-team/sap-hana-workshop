# Create New Exercise Content

We encourage folks to create new exercise content, fork with your own content, and customize the workshop any way you want.  This will give you the flexibility to customize workshops for your own friends, customers, community or project!

## Table of Contents

* [Using your own fork](#using-your-own-fork)

# Using your own fork

When a workshop is provisioned, the control node for every workbench (where the Red Hat Ansible Automation is installed and executed from) will load solution exercises into `~/{{workshop}}-workshop`.  For example if you are running the `networking` workshop the home directory for every student will have `~/home/networking-workshop`.

This can be customized!  There are three variables that you can change with your provisioner code

   - `ansible_workshops_url` - points to the git repo where you want to load exercises from.  By default this uses [https://github.com/ansible/workshops.git](https://github.com/ansible/workshops.git) if this is not specified.
   - `version` - points to the git [branch](https://git-scm.com/docs/git-branch) for the specified git repo.  By default this uses `master`
   - `refspec` - points to the git [refspec](https://git-scm.com/book/en/v2/Git-Internals-The-Refspec).  By default this is set to `""` (nothing).

These variables are used in the `control_node` role which can found here: `provisioner/roles/control_node/tasks/main.yml`
