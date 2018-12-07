# Deploying containerized Red Hat OpenShift® Container Platform 3.9 on Lenovo ThinkAgile HX cluster(Nutanix cluster)

## Instructions
Please, refer to [Reference Architecture document](???)
 for actual instructions.

### Clone the repository
`git clone git@github.com:intel/openshift-container-architecture.git`

### Creating inventory
Inventory file has to be filled manually.
Refer to *hosts.example* for possible variables.

`cp hosts.example /etc/ansible/hosts;
vim /etc/ansible/hosts`

### Provisioning system setup

`ansible-playbook ipxe-deployer/ipxe.yml`

### Preparing the nodes for OpenShift Container Platform

`ansible-playbook src/prerequisites/nodes_setup.yaml -k`

### Setting up multimaster HA
run:

`ansible-playbook src/keepalived-multimaster/keepalived.yaml`

### Create shared storage
Create a NFS storage container (named Redhat and granted ReadWriteMany access modes) in Nutanix cluster. And Create a directory (named registry and granted ReadWriteMany access modes) in storage container.

### Deploying OpenShift cluster

`atomic install --system     --set INVENTORY_FILE=/etc/ansible/hosts     --storage=ostree     --set PLAYBOOK_FILE=/usr/share/ansible/openshift-ansible/playbooks/prerequisites.yml     --set OPTS="-v"     registry.access.redhat.com/openshift3/ose-ansible:v3.9`
`atomic install –-system --storage=ostree  --set INVENTORY_FILE=/etc/ansible/hosts     --set PLAYBOOK_FILE=/usr/share/ansible/openshift-ansible/playbooks/deploy_cluster.yml     --set OPTS="-v"     registry.access.redhat.com/openshift3/ose-ansible:v3.9`
