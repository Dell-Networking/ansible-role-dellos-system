System Role for Dell EMC Networking OS
======================================
This role configures the global system configuration. This role is abstracted for OS6, OS9, and OS10.  
This role specifically enables configuration of the hostname, SNMP server, the NTP server, logging servers, the hostname, management route, and CLI users.
This role takes an object that matches the requirements described as follows and performs the necessary configuration. If an object is not defined, then it is ignored.


Installation
------------

```
ansible-galaxy install Dell-Networking.dellos-system
```

Requirements
------------
This role requires an SSH connection for connectivity to your Dell EMC Networking OS device. You can use any of the built-in Dell EMC Networking OS connection variables, or the ``provider``
dictionary.

Role Variables
--------------

``dellos_system``(dictionary) contains the hostname (dictionary). 
The hostname is the value of the variable ``hostname`` that corresponds to the name of the OS device.
This role is abstracted using the variable ``ansible_net_os_name`` that can take the following values: dellos6, dellos9 and dellos10.

Any role variable with corresponding state variable setting to *absent* negates the configuration of that variable. For variables with no state variable, setting empty value to the variable negates the corresponding configuration.

The variables and its values are **case-sensitive**.

**hostname** (dictionary) contains the following keys:

|        Key | Type                      | Notes                                                                                                                                                                                     |
|------------|---------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| snmp_contact | string | The ASCII string used to configure the contact information of the SNMP server.  |
| snmp_location | string | The ASCII string used to configure the location information of the SNMP server. |
| snmp_community | list | This key contains objects to configure the SNMP community string. See the following snmp_community.* keys for each list item. |
| snmp_community.name | string (required)         | The SNMP community string.                                                                                                                                                                |
|snmp_community.access_mode | string, choices: ro, rw           | The mode of access with the community string.                                                                                                                                             |
|     snmp_community.state | string, choices: absent, present*   | The absent state deletes a SNMP community string.                                                                                                                                 |
| snmp_host | list | This key contains objects to configure SNMP hosts to receive SNMP traps. See the following snmp_host.* keys for each list item. For OS10 devices, this variable is not supported. |
| snmp_host.ip | string (required) | The IP address of the SNMP trap host. |
| snmp_host.communitystring | string | The SNMP community string for the trap host. |
| snmp_host.udpport | string | The UDP number of the SNMP trap host. For OS9 devices, range of values accepted is 0-65535 and 1-65535 for OS6 devices. |
| snmp_host.state | string, choices: absent, present* | The absent state deletes the SNMP trap host. |
| users | list | This key contains objects to configure user accounts. See the following users.* keys for each list item. |
|   users.username | string (required)         | The unique username. The username must adhere to certain format guidelines. Valid usernames begin with A-Z, a-z, or 0-9 and can also contain any of the following characters: @#$%^&*-_= +;<>,.~  |
|   users.password | string                    | This key is the password set for the username.                              Password length must be atleast 8 characters in OS10 and OS6 devices.                                              |
|       users.role | string                    | Configures the role assigned to the user. For OS6 devices, this key is not supported.                                                                      |
|  users.privilege | int                | Configures the privilege level for the user. For OS9 devices, the permitted values are integers between 0 and 15. For OS6 devices, the value can be either 0,1 or 15. For OS9 and OS6, if you omit this key, the default privilege is 1. For OS10 devices, this key is not supported.                                      |
|     users.state | string, choices: absent, present*     | This key with the setting *absent* deletes a user account with the username.                                                                                                                                 |
| sntp | list | This key contains objects to configure the NTP server. See the following sntp.* keys for each list item. |
|         sntp.ip | string (required)         | Configures the IPv4 address for the NTP server. The value must be in the form of A.B.C.D                                                                                             |
|     sntp.state | string, choices: absent, present*     | This key with the setting *absent* deletes the NTP server.                                                                                                                                          |
| logging | list | This key contains objects to configure the logging server. See the following logging.* keys for each list item. |
|         logging.ip | string (required)         | Configures the IPv4 address for the logging server. The value must be in the form of A.B.C.D                                                                                                 |
|     logging.state | string, choices: absent, present*     | This key with the setting *absent* deletes the logging server.                                                                                                             |
| management_rt | list | This key contains objects to configure the management routes. For OS10 and OS6 devices, this variable is not supported. |
| management_rt.ip | string (required) | The IP address set as management route. For IPV4, the value must be in the form of A.B.C.D and for IPV6 the value must be in the form of A:B:C:D::E  |
| management_rt.ipv4 | boolean: true*, false | This key identifies whether the management route is an IPv4 or IPV6 address. The key with setting *false* or undefined key takes ip as IPV6. |
| management_rt.state | string, choices: absent, present* | The key with the setting *absent* deletes the management route. |


```
Note: Asterisk (*) denotes the default value if none is specified. 
```

Connection Variables
--------------------

Ansible Dell EMC Networking roles require the following connection information to establish 
communication with the nodes in your inventory. This information can exist in
the Ansible group_vars or host_vars directories, or in the playbook itself.


|         Key | Required | Choices    | Description                              |
| ----------: | -------- | ---------- | ---------------------------------------- |
|        host | yes      |            | The host name or address for connecting to the remote device over the specified *transport*. The value of *host* is the destination address for the transport. |
|        port | no       |            | The port used to build the connection to the remote device.  If the task does not specify the value, the port value defaults to 22. |
|    username | no       |            | Configures the username that authenticates the connection to the remote device. The value of *username* authenticates the CLI login. If the task does not specify the value, the value of environment variable ANSIBLE_NET_USERNAME is used instead. |
|    password | no       |            | Specifies the password that authenticates the connection to the remote device. If the task does not specify the value, the value of environment variable ANSIBLE_NET_PASSWORD is used instead. |
|   authorize | no       | yes, no*   | Instructs the module to enter privileged mode on the remote device before sending any commands. If not specified, the device attempts to execute all commands in non-privileged mode. If the task does not specify the value, the value of environment variable ANSIBLE_NET_AUTHORIZE is used instead. |
|   auth_pass | no       |            | Specifies the password to use if required to enter privileged mode on the remote device. If *authorize=no*, then this argument does nothing. If the task does not specify the value, the value of environment variable ANSIBLE_NET_AUTH_PASS is used instead. |
|   transport | yes      | cli*       | Configures the transport connection to use when connecting to the remote device. The *transport* argument supports connectivity to the device over CLI (SSH).  |
|    provider | no       |            | Convenient method that passes all of the above connection arguments as a dict object. All constraints (required, choices, etc.) must be met either by individual arguments or values in this dict. |

```
Note: Asterisk (*) denotes the default value if none is specified.
```

Dependencies
------------

The dellos-system role is built on modules included in the core Ansible code.
These modules were added in Ansible version 2.2.0.

Example Playbook
----------------

The following example uses the dellos.dellos-system role to completely set up
CLI users, the NTP server, the SNMP server, logging servers, and the hostname. 
The example creates a ``hosts`` file with the switch details and corresponding 
variables defined in the ``vars/main.yml`` file at the role path.
It writes a simple playbook that only references the dellos-system role. 
By including the role, you automatically get access to all of the tasks to configure system
features. 


Sample ``hosts`` file:

    [leafs]
    leaf1

Sample ``host_vars/leaf1``:

    hostname: leaf1
    provider:
      host: "{{ hostname }}"
      username: admin
      password: admin
      authorize: yes
	  auth_pass: "calvin"
      transport: cli
	  
Sample ``vars/main.yml``:

	dellos_system:
      leaf1:
        snmp_contact:  test
        snmp_location: chennai
        snmp_community:
          - name: public  
            access_mode: ro
            state: present
          - name: private 
            access_mode: rw
            state: present 
        snmp_host:
          - ip: 2001:4898:f0:f09b::2000
            communitystring: private
            udpport: 162
            state: present
        users:
          - username: test
            password: test
            role: sysadmin
            privilege: 0
            state: absent
        sntp:
          - ip: 2.2.2.2
            state: absent
        logging:
          - ip: 1.1.1.1
            state: absent
        management_rt:
          - ip: 10.16.148.254
            state: present
            ipv4: True

 
A simple playbook to setup system, ``leaf.yml``:

    - hosts: leafs
      roles:
         - Dell-Networking.dellos-system

Then run with:

    ansible-playbook -i hosts leaf.yml

License
--------

Copyright (c) 2016, Dell Inc. All rights reserved.
 
Licensed under the Apache License, Version 2.0 (the "License");
you may not use this file except in compliance with the License.
You may obtain a copy of the License at
 
    http://www.apache.org/licenses/LICENSE-2.0
 
Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and
limitations under the License.
