# Ansible Role: cnos-vlag-2tier-spine - Multiple Layer vLAG Spine Configuration
---
<add role description below>

This is a template driven playbook for a multiple layer vLAG configuration. The role applies to the vLAG peers. The vLAG peers are set up as two pairs that are connected with each other in a North-South orientation. These switches will be referred to as spine switches.

The templates used in this configuration example are different for North and South spine switches. They will be used to configure the spine switches. To separate between North and South spine switches, a condition flag that specifies this will be present in the */etc/anisble/hosts* file. The templates will be executed with different settings for the peer IP configuration that are suitable to each spine switch. The peer IP has to be specified for each spine switch in the */etc/anisble/hosts* file with the tag ID `peerip`. Since there are two pairs of spine switches, there will be two sets of values used in the templates.

The configuration commands and their results can be verified in the *commands* and *results* directories.

For more details, see [Configuring a multiple layer vLAG network](http://systemx.lenovofiles.com/help/index.jsp?topic=%2Fcom.lenovo.switchmgt.ansible.doc%2Fconfiguring_a_multiple_layer_vlag_using_ansible.html&cp=0_3_1_0_7).


## Requirements
---
<add role requirements information below>

- Ansible version 2.3 or later ([Ansible installation documentation](http://docs.ansible.com/ansible/intro_installation.html))
- Lenovo switches running CNOS version 10.2.1.0 or later
- an SSH connection to the Lenovo switch (SSH must be enabled on the network device)


## Role Variables
---
<add role variables information below>

The values of the variables used need to be modified to fit the specific scenario in which you are deploying the solution. To change the values of the variables, you need to visits the *vars* directory of each role and edit the *main.yml* file located there. The values stored in this file will be used by Ansible when the template is executed.

The syntax of *main.yml* file for variables is the following:

```
<template variable>:<value>
```

You will need to replace the `<value>` field with the value that suits your topology. The `<template variable>` fields are taken from the template and it is recommended that you leave them unchanged.

Available variables are listed below, along with description:

Variable | Description
--- | ---
`username` | Specifies the username used to log into the switch
`password` | Specifies the password used to log into the switch
`flag` | Configures the condition flag associated with the switch
`slot_chassis_number1` | Specifies the ethernet port (*slot number/port number*)
`portchannel_interface_number1` | Specifies the LAG number (*1-4096*)
`portchannel_mode1` | Configures the LAG type (**on** - static LAG, **active** - active member of a LACP LAG, **passive** - passive member of a LACP LAG)
`slot_chassis_number2` | Specifies the ethernet port (*slot number/port number*)
`portchannel_interface_number2` | Specifies the LAG number (*1-4096*)
`slot_chassis_number3` | Specifies the ethernet port (*slot number/port number*)
`portchannel_interface_number3` | Specifies the LAG number (*1-4096*)
`slot_chassis_number4` | Specifies the ethernet port (*slot number/port number*)
`portchannel_interface_number4` | Specifies the LAG number (*1-4096*)
`switchport_mode1` | Configures the switch port mode (**access** - the port can be part of only a single VLAN, **trunk** - the port can be part of any number of VLANs)
`slot_chassis_number5` | Specifies the ethernet port (*slot number/port number*)
`stp_mode1` | Configures the STP mode (**mst** - MSTP, **rapid-pvst** - Rapid PVST+, **disable** - STP is disabled)
`vlag_tier_id1` | Configure the vLAG tier ID (*1-512*)
`vlag_instance_number1` | Configure the vLAG instance (*1-64*)
`vlag_instance_number2` | Configure the vLAG instance (*1-64*)
`vlag_instance_number3` | Configure the vLAG instance (*1-64*)


## Dependencies
---
<add dependencies information below>

- username.iptables - Configures the firewall and blocks all ports except those needed for web server and SSH access.
- username.common - Performs common server configuration.
- /etc/ansible/hosts - You must edit the */etc/ansible/hosts* file with the device information of the switches designated as spine switches. You may refer to *cnos-vlag-2tier-spine-hosts* for a sample configuration.

Ansible keeps track of all network elements that it manages through a hosts file. Before the execution of a playbook, the hosts file must be set up.

Open the */etc/ansible/hosts* file with root privileges. Most of the file is commented out by using **#**. You can also comment out the entries you will be adding by using **#**. You need to copy the content of the hosts file for the role into the */etc/ansible/hosts* file. The sample hosts file for the role is located in the main directory.
  
```
[cnos-vlag-2tier-spine]
10.240.178.76   username=<username> password=<password> deviceType=g8272_cnos condition=north peerip=10.240.178.77
10.240.178.77   username=<username> password=<password> deviceType=g8272_cnos condition=north peerip=10.240.178.76
10.240.178.78   username=<username> password=<password> deviceType=g8272_cnos condition=south peerip=10.240.178.79
10.240.178.79   username=<username> password=<password> deviceType=g8272_cnos condition=south peerip=10.240.178.78
```
**Note:** You need to change the IP addresses, including the IP addresses of the vLAG peers, to fit your specific topology. You also need to change the `<username>` and `<password>` to the appropriate values used to log into the specific Lenovo network devices.


## Example Playbook
---
<add playbook samples below>

To execute an Ansible playbook, use the following command:

```
ansible-playbook cnos-vlag-2tier-spine.yml -vvv
```

`-vvv` is an optional verbos command that helps identify what is happening during playbook execution. The playbook for each role of the multiple layer vLAG configuration solution is located in the main directory of the solution.

```
- hosts: cnos-vlag-2tier-spine
  roles:
    - cnos-vlag-2tier-spine
```


## License
---
<add license information below>
Copyright (C) 2017 Lenovo, Inc.

Licensed under the Apache License, Version 2.0 (the "License");
you may not use this file except in compliance with the License.
You may obtain a copy of the License at

    http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and
limitations under the License.
