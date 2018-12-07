# deployment-scripts-for-OpenShift-Platform
deployment for OpenShift Container Platform for  DevOps and Hybrid Cloud
Deploying containerized Red Hat OpenShift® Container Platform 3.9 as system service with Container-Native Storage

## Instructions
Please, refer to [Reference Architecture document](???)
 for actual instructions.

### Clone the repository
`git clone git@github.com:intel/openshift-container-architecture.git`

### Creating inventory
Inventory file has to be filled manually.
Refer to *hosts.example* for possible variables.

`cp hosts.example /etc/ansible/hosts;
vi /etc/ansible/hosts`

### Switch Configuration (optional)
Update inventory group *[switches]* and run:

`ansible-playbook src/cnos-configuration/configure-networking.yaml`

### Provisioning system setup

`ansible-playbook ipxe-deployer/ipxe.yml`

### Preparing the nodes for OpenShift Container Platform

`ansible-playbook src/prerequisites/nodes_setup.yaml -k`

### Setting up multimaster HA
run:

`ansible-playbook src/keepalived-multimaster/keepalived.yaml`

### Deploying OpenShift cluster

`atomic install --system     --set INVENTORY_FILE=/etc/ansible/hosts     --storage=ostree     --set PLAYBOOK_FILE=/usr/share/ansible/openshift-ansible/playbooks/prerequisites.yml     --set OPTS="-v"     registry.access.redhat.com/openshift3/ose-ansible:v3.9`

`atomic install –-system --storage=ostree  --set INVENTORY_FILE=/etc/ansible/hosts     --set PLAYBOOK_FILE=/usr/share/ansible/openshift-ansible/playbooks/deploy_cluster.yml     --set OPTS="-v"     registry.access.redhat.com/openshift3/ose-ansible:v3.9`

### Deploy Openshift cluster on Lenovo ThinkAgile HX cluster

Please find deployment guide and script in [OCP_HX](https://github.com/lenovo/deployment-scripts-for-OpenShift-Platform/tree/master/OCP_HX)
