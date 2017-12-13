# Install Gluster storage to Openshift

DISCLAIMER: THIS SETUP WITH DEFAULTS IS NOT FOR PRODUCTION USE!

This ansible playbook is for installing container native storage (CNS) to Openshift Container Platform 3.4+.

# What you need to get this rocking

- Ansible
- OCP 3.4+ (3.7 no tested yet) with three app nodes
- Subscription for Gluster storage (eval or proper production one)
- Access to OCP master via Ansible
- At least one spare brick in each three Openshift app nodes
- Ansible hosts file that was used to install yuor OCP. At least same hosts.

# Setting up

All variables that you need to change are in `vars.yml` file.

You need to change at least following to match your env.

#### openshift_subscription_pool_id
Openshift Container Platform subscription pool id. You can fetch is with command `subscription-manager list --available --matches="*Openshift*"`

#### subscription_pool_id
Gluster storage subcription pool id. `subscription-manager list --available`

#### openshift_apps_domain
Gluster installation process goes forward so fast that host that executes cns-deploy will need entries to /etc/hosts file to resolv needed services fast enough.
#### openshift_router_ip
For /etc/hosts file
#### storage_hosts_for_ansible
List of target hosts for Ansible to define where to install actual storage nodes. Must be found from your Ansible hosts file.
#### storage_nodes
List of Openshift nodes running storage, double check with `oc get nodes`
#### storage_hosts
List of storage hosts IP addresses. Must be in same order as nodes in storage_nodes
#### storage_devices
Devices that will be used as storage on storage hosts/nodes.
#### storage_zones
This is used as hack to label storage nodes with correct zone label. Must be in same order as nodes in storage_nodes var.

# Installation

Playbook must be installed on hosts that has ansible and connection to all Openshift masters and nodes. This playbook relies on default what comes to ansible connection to remote hosts. Change connenction stuff to match your env either directly to playbook and to ansible configs.

Execute installation with following command after you have check your Ansible connection stuff and vars.yml.

```
git clone https://github.com/tahonen/gluster-openshift.git
cd gluster-openshfit
ansible-playbook playbooks/install_gluster.yml
```

After installation you can test was installation success by create app that needs storage like `oc new-app --template mongodb-persistent -p VOLUME_CAPACITY=1Gi`

# Customize installation

All template and config files are pretty much defauls that work. If you want to customize your installation you can change following files, but you need to know what you are doing :)

#### playbooks/roles/gluster_install/templates/topology.j2
Change this file if you want to create something else than single storage cluster with 3 zones.

#### playbooks/roles/gluster_install/templates/heketi.j2
Change if you need to change heketi config.

#### playbooks/roles/gluster_install/templates/glusterfs-storageclass.j2
Change if you need to modify or add stuff to storageclass definition. For example add labels.

#### playbooks/roles/gluster_install/tasks/main.yml
Change this file if you need to change how installation goes.
