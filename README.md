# VMware Cloud Foundation management domain host setup for Cisco UCSM rack server
This repository contains Ansible playbooks for configuring Cisco UCSM managed C-Series (tested Cisco UCS C240 M5) servers. This repository aligns with the following design guide: https://www.cisco.com/c/en/us/td/docs/unified_computing/ucs/UCS_CVDs/flexpod_vcf_design.html. This repository can be used to setup UCS policies and Service Profile Templates as well as to perform initial configuration of the ESXi hosts for deploying VCF using Cloud Builder. To run these playbooks, Cisco UCS C-Series servers should be connected via Cisco UCS Fabric Interconnects as shown in the figure.

<img width="817" alt="UCSM-Overview" src="https://user-images.githubusercontent.com/89957595/206741425-320d7c90-cd1a-473b-bd8f-6ded1ed08fd4.png">

# Cisco UCS Manager and ESXi Configuration
The playbooks in this repository perform following high level functions:
## Cisco UCS
- Equipment Tasks to setup server ports and uplink port-channels
- Admin tasks to configure DNS, NTP, Timezone, and Organization
- LAN tasks to configure VLANs, Mgmt. and MAC address pools, various LAN policies, and vNIC templates
- Server tasks to configure UUID pools, BIOS, boot, disk, power, maintenance and Server Profile templates
After successfully executing UCS playbooks, you can use the server profile template to derive 4 server profiles and install ESXi software on local disk.
## ESXi Config
- Setup NTP server
- Setup hostname, domain and DNS server
- Enable SSH on the hosts
- Set vSwitch0 MTU to 9000
- Set default port-group VLAN (management)
After running the ESXi playbooks, log into each ESXi host using SSH and re-generate self signed certificates using the following commands:
```
/sbin/generate-certificates
/etc/init.d/hostd restart && /etc/init.d/vpxa restart
```
The ESXi hosts are now ready for VCF cloud builder configuration. 

# Execution Package Requirement
To execute various ansible playbooks, a linux based system will need to be setup with Ansible and following packages:
- https://galaxy.ansible.com/cisco/ucs
- https://github.com/ansible-collections/community.vmware 

# Setting up Variables
All the variables used in this framework are defined in the following locations:
- Variable that require customer inputs are part of group_vars/all.yml
- Variable that do not typically require customer input (e.g. VLAN assignment to VDS or policy names) are present under role_name/defauls/main.yml.

# Playbook Execution
To execute the playbook, you will need to follow these steps:
1. Physically connect the hardware and perform the initial configuration so UCS can be accessed over network using its management IP address
2. Setup a Linux (or similar) host and install Ansible, git and required UCS and VMware packages
3. Clone the repository using git
4. Update the inventory file to provide the access information for UCSM and ESXi host information
5. Update variables in group_vars/all.yml to match your environment
6. Execute the Setup_UCS.yml playbook to setup all the policies and server profile templates `ansible-playbook ./Setup_UCS.yml -i inventory`
7. Manually derive 4 server profiles
8. Install ESXi on the 4 servers and configure the management interface to access ESXi hosts
9. Execute the prepare_esxi_host.yml playbook to prep the ESXi hosts `ansible-playbook ./prepare_esxi_hosts.yml -i inventory`
10. Regenerate the self signed certificates on all 4 ESXi hosts manually or use the playbook: `ansible-playbook ./regenerate_esxi_hosts_certs.yml -i inventory`

At this time, ESXi servers will be ready for VCF cloud builder to setup the management domain. 
