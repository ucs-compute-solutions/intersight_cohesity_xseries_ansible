# Cohesity Infrastructure Deployment with Cisco X-Series Intel based All Flash nodes supporting both Cisco VIC 4th Gen and 5th Gen VIC

This repository  contains Ansible playbooks to configure the Cohesity on Cisco X-Series Infrastructure which includes : <br />
&emsp;&emsp; •	 Cisco UCS in Intersight Managed Mode (IMM) <br />


This repository can be used to automate the configuration Cohesity Infrastructure on Intel X210C All NVME nodes configured in Cisco X-Series Modular System  .

Details will be covered in the upcoming Cisco Validated Design document.

The CVD lays out the complete process for configuring the Cohesity Infrastructure on Cisco X-Series All NVMe nodes using Ansible. Since these playbooks are intended to save time in setting up a working Cohesity environment, a complete deployment architecture for Cohesity on Cisco X-Series is shown below is needed to execute the playbooks.

## Cohesity on X-Series - Physical Topologies




![XSeries architecture](https://user-images.githubusercontent.com/101294457/205809370-3237c9b3-c5e4-493a-a923-77484319fdec.png)

<br />

## How to execute these playbooks?

### High-Level Cohesity on X-Series Automation
<img width="1200" alt="Screen Shot 2022-09-15 at 5 03 58 PM" src="https://user-images.githubusercontent.com/25094641/190393678-6cafd996-fae2-4553-8f23-12cbe1c11293.png">

<br />
<br />


###  Cohesity on X-Series Automated Deployment Workflow
<img width="700" src="https://user-images.githubusercontent.com/3585414/144469881-a647e3fa-f48e-411c-b13c-822bfb9a15ea.png">
<br />
<br />


### Set up the execution environment
To execute various ansible playbooks, a Linux based system will need to be setup as described in the CVD with the packages listed at the following pages: <br />
•	Cisco Intersight: https://galaxy.ansible.com/cisco/intersight <br />


You might already have this collection installed.

- To check whether it is installed, run: `ansible-galaxy collection list`
- To install it, use: <br />
- `ansible-galaxy collection install cisco.intersight` (For Intersight Collection) <br />

<br />

### Intersight Configuration and Access Requirement

The Intersight playbooks in this repository perform following functions:

1. Create various pools required to setup a Server Profile Template
2. Create various policies required to setup a Server Profile Template
3. Create Server Profile Templates

After successfully executing the playbooks, one or many server profiles can easily derived and attached to the All NVMe node from Intersight dashboard.

**NOTE:** The addition of UCS to Intersight Account or configuration of Domain Profile to setup UCS is not part of this repository and will have to be performed manually before executing the playbooks.

**NOTE:** The playbooks do not create an organization and assume an organization (default or otherwise) has already been setup under Intersight account. The organization name must be updated in group_vars/all.yml(org_name) for successful execuation of the playbooks.

<br />

To execute the playbooks against your Intersight account, you need to complete following additional steps of creating an API key and saving the Secrets_File:

https://community.cisco.com/t5/data-center-and-cloud-documents/intersight-api-overview/ta-p/3651994

The API key and Secrets_Filename information is added to the group_vars/all.yml. The default Secrets_File value in all.yml assumes Secrets_File was copied to the same folder/directory where Ansible Playbooks were cloned (alongside inventory file).

<br />


### Setting up Variables

All the variables used in this framework are defined in the following locations:

1. Variable that require customer inputs are part of group_vars/
2. Variable that do not typically require customer input (e.g. descriptions etc.) are present under role_name/defauls/main.yml.
   Setup all the variables before executing the playbooks as detailed in the CVD. Intersight's pools and policies created using these playbooks are tagged with user_defined_prefix and "ansible" to easily filter the configuration.

<br />


### Playbook Execution Commands

1.	Setup Pools in Intersight: `ansible-playbook ./create_pools.yml -i inventory`
2.	Setup Policies in Intersight: `ansible-playbook ./create_server_policies.yml -i inventory`
3.	Setup Server Profile Template(s) in Intersight: `ansible-playbook ./create_server_profile_template.yml -i inventory`

<br />


### Post Configuration Tasks

Execution of first three playbooks in these repositories set up Server Profile Template in Intersight. After successfully executing the playbooks, one or more server profiles can easily derived and attached to the compute node from Intersight dashboard. KVM mounted DVD option or Intersight OS Install feature is available to install Cohesity OS to these newly derived servers.

<br />
