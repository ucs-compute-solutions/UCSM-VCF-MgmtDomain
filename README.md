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
## ESXi Config
- Setup NTP server
- Setup DNS server
- Enable SSH on the hosts
- Set vSwitch0 MTU to 9000
- Set default port-group VLAN (management)

