# gluster-openshift
This ansible playbook is for installing container native storage (CNS) to Openshift Container Platform 3.4 and (Openshift Origin 1.4 NOT TESTED).

# What you need to get this rocking

- OCP 3.4 with three app nodes
- Subscription for Gluster storage (eval or proper production one)
- Access to OCP master via Ansible
- At least one spare brick in each three Openshift app nodes

# Setting up

All variables that you need to change are in `playbooks/group_vars/all` file
