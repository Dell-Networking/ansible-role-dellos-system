System Role for Dell EMC Networking OS
======================================

This role facilitates the configuration of global system attributes. It supports the configuration of hostname, SNMP server, NTP server,logging servers, management route, and CLI users. This role is abstracted for OS6, OS9, and OS10.


Installation
------------

```
ansible-galaxy install Dell-Networking.dellos-system
```

Requirements
------------
This role requires an SSH connection for connectivity to your Dell EMC Networking device. You can use any of the built-in Dell EMC Networking OS connection variables, or the ``provider``
dictionary.

Role Variables
--------------

``dellos_system``(dictionary) contains the hostname (dictionary). 
The hostname is the value of the variable ``hostname`` that corresponds to the name of the OS device.This role is abstracted using the variable ``ansible_net_os_name`` that can take the following values: dellos6, dellos9 and dellos10.

Any role variable with a corresponding state variable set to absent negates the configuration of that variable. For variables with no state variable, setting an empty value for the variable negates the corresponding configuration.

The variables and its values are case-sensitive.

``hostname`` contains the following keys:

|        Key | Type                      | Notes                                                                                                                                                                                     |
|------------|---------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| unique_hostname | boolean: true, false* | Configures unique hostname in the blade switch. This key is only supported for blade switch (MXL) in OS9 devices.  |
| enable_password | string              | Configures enable password. This key is only supported in OS6 and OS9. |
| snmp_contact | string | Configures SNMP contact information.  |
| snmp_location | string |Configures SNMP location information. |
| snmp_community | list | Configures the SNMP community information. See the following snmp_community.* keys for each list item. |
| snmp_community.name | string (required)         | Configures the SNMP community string.                                                                                                                                                                |
|snmp_community.access_mode | string, choices: ro, rw           | Configures the access mode for the community.                                                                                                                                             |
| snmp_community.state | string, choices: absent, present*   | If set to absent, deletes the SNMP community information.                                                                                                               |
| snmp_host | list | Configures SNMP hosts to receive SNMP traps. See the following snmp_host.* keys for each list item. For OS10 devices, this key is not supported. |
| snmp_host.ip | string (required) | Configures IP address of the SNMP trap host. |
| snmp_host.communitystring | string | Configures SNMP community string of the trap host. |
| snmp_host.udpport | string | Configures UDP number of the SNMP trap host. For OS9 devices, the value range is 0-65535 and 1-65535 for OS6 devices. |
| snmp_host.state | string, choices: absent, present* | If set to absent, deletes the SNMP trap host. |
| snmp_traps | list | Configures SNMP traps. See the following snmp_traps.* keys for each list item. This key is supported only in OS9 and OS6. |
| snmp_traps.name | string | Enables SNMP traps.   |
| snmp_traps.state | string, choices: absent, present* | If set to absent, deletes the SNMP trap. |
| mtu | integer | Configures Maximum Transmission Unit for all interfaces. This key is supported only in OS6. |
| users | list | Configures user account. See the following users.* keys for each list item. |
|   users.username | string (required)         | Configures username. The username must adhere to specific format guidelines. Valid usernames begin with A-Z, a-z, or 0-9 and can also contain any of the following characters: @#$%^&*-_= +;<>,.~  |
|   users.password | string                    | Configures password set for the username. Password length must be at least eight characters in OS10 and OS6 devices.                                              |
|       users.role | string                    | Configures the role assigned to the user. This key is not supported in OS6.                                                                      |
|  users.privilege | int                | Configures the privilege level for the user. For OS9 devices, the permitted values are integers between 0 and 15. For OS6 devices, the value can be either 0, 1, or 15. For OS9 and OS6, if you omit this key, the default privilege is 1. This key is not supported in OS10.                                      |
|     users.state | string, choices: absent, present*     | If set to absent, deletes a user account.                                                                                                                                 |
| sntp | list | Configures the NTP server. See the following sntp.* keys for each list item. |
|         sntp.ip | string (required)         | Configures the IPv4 address for the NTP server. The value must be in the form of A.B.C.D                                                                                             |
|     sntp.state | string, choices: absent, present*     | If set to absent, deletes the NTP server.                                                                                                                                          |
| logging | list | Configures the logging server. See the following logging.* keys for each list item. |
|         logging.ip | string (required)         | Configures the IPv4 address for the logging server. The value must be in the form of A.B.C.D                                                                                                 |
|     logging.state | string, choices: absent, present*     | If set to absent, deletes the logging server.                                                                                                             |
| management_rt | list | Configures the management routes. This key is not supported in OS10 and OS6. |
| management_rt.ip | string (required) | Configures IP destination prefix for management route. For IPV4, the value must be in the form of A.B.C.D and for IPv6 the value must be in the form of A:B:C:D::E  |
| management_rt.ipv4 | boolean: true*, false | Specifies management route is an IPv4 or IPv6 address. When this is set to false or undefined, it sets IP as IPv6. |
| management_rt.state | string, choices: absent, present* | If set to absent, deletes the management route.|
| tacacsserver | dictionary | Configures TACACS server and enables AAA authentication. See the following tacacsserver.* keys for each list item. This key is supported in OS6 and OS9.|
| tacacsserver.key | string (required), choices: 7, WORD | Configures authentication key for a TACACS server. For configuring a hidden key, this key value should be 7. For other cases, any string can be accepted as authentication key. | 
| tacacsserver.hiddenkey | string | Configures hidden key string for authentication. The value should be of length 256 for OS6. | 
| tacacsserver.tacacsgroup | list | Configures TACACS server group. This key is supported only in OS9.|
| tacacsgroup.name | string (required) | Configures name for the TACACS server group. | 
| tacacsgroup.host | list | Configures list of hosts in a TACACS server group. See the following host.* keys for each list item. |           
| host.ip | string (required)| Configures IP address of the TACACS server host. |
| host.timeout | integer, default=10 | Configures the timeout value for the TACACS server to reply. |  
| host.state | string, choices: absent,present | Configuring this key as absent deletes the host from a TACACS server group. |
| tacacsgroup.state | string, choices: absent,present | If set to absent, deletes a TACACS server group. |
| tacacsserver.aaa_tacacsgroup | string | Configures the name of the TACACS server group to enable AAA authentication. This key is supported only in OS9. |
| tacacsserver.aaa_tacacshost | list | Configures authentication list for TACACS server hosts. See aaa_tacacshost.* keys for each list item. This key is supported in OS9 and OS6.|
| aaa_tacacshost.name | string | Configures the name of the authentication list. |
| aaa_tacacshost.login_or_enable | string, choices: login,enable | Configures the authentication list to logins or enable. |
| aaa_tacacshost.use_password | string, choices: line,local,enable,none | Configures Username/Password used for authentication. Local Username/Password can not be used for enable authentication. |
| aaa_tacacshost.state | string, choices: absent, present* | If set to absent, deletes the AAA authentication for tacacshost. |
| tacacshost | list | Configures list of hosts in a TACACS server group. This key is supported in OS9 and OS6.|
| tacacshost.ip | string (required) | Configures IP address of the TACACS server host. For IPV4, the value must be in the form of A.B.C.D and for IPV6 the value must be in the form of A:B:C:D::E | 
| tacacshost.timeout | integer, default=10 | Configures the timeout value for the TACACS server to reply. |
| tacacshost.state | string, choices: absent,present* | If set to absent, deletes the host from a TACACS server group. |

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
|        host | yes      |            | Hostname or address for connecting to the remote device over the specified ``transport``. The value of this key is the destination address for the transport. |
|        port | no       |            | Port used to build the connection to the remote device. If the value of this key does not specify the value, the value defaults to 22. |
|    username | no       |            | Configures the username that authenticates the connection to the remote device. The value of this key authenticates the CLI login. If this key does not specify a value, the value of environment variable ANSIBLE_NET_USERNAME is used instead. |
|    password | no       |            | Specifies the password that authenticates the connection to the remote device. If this key does not specify the value, the value of environment variable ANSIBLE_NET_PASSWORD is used instead. |
|   authorize | no       | yes, no*   | Instructs the module to enter privileged mode on the remote device before sending any commands. If this key does not specify the value, the value of environment variable ANSIBLE_NET_AUTHORIZE is used instead. If not specified, the device attempts to execute all commands in non-privileged mode.|
|   auth_pass | no       |            | Specifies the password to use if required to enter privileged mode on the remote device. If ``authorize`` is set to no, then this key is not applicable. If this key does not specify the value, the value of environment variable ANSIBLE_NET_AUTH_PASS is used instead. |
|   transport | yes      | cli*       | Configures the transport connection to use when connecting to the remote device. This key supports connectivity to the device over CLI (SSH).  |
|    provider | no       |            | Convenient method that passes all of the above connection arguments as a dictonary object. All constraints (such as required, choices) must be met either by individual arguments or values in this dictonary. |


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
variables defined in the ``vars/main.yaml`` file at the role path.
It writes a simple playbook that only references the dellos-system role. 
By including the role, you automatically get access to all of the tasks to configure system
features. 


Sample ``hosts`` file:
 
    leaf1 ansible_host= <ip_address> ansible_net_os_name= <OS name(dellos9/dellos6/dellos10)>

Sample ``host_vars/leaf1``:

    hostname: leaf1
    provider:
      host: "{{ hostname }}"
      username: xxxxx 
      password: xxxxx
      authorize: yes
	  auth_pass: xxxxx 
      transport: cli
	  
Sample ``vars/main.yaml``:

	dellos_system:
      leaf1:
        unique_hostname: True
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
        tacacsserver:
          key: 7
          hiddenkey: 9ea8ec421c2e2e5bec757f44205015f6d81e83a4f0aa52fa
          tacacsgroup:
            - name: TACACS
              host:
                - ip: 2001:4898:f0:f09b::1000
                  timeout: 2
                  state: present
              state: present
          aaa_tacacsgroup: TACACS
          aaa_tacacshost:
            - name: default
              login_or_enable: login
              use_password: local
              state: present
            - name: console
              login_or_enable: login
              use_password: local
              state: present
          tacacshost:
            - ip: 2001:4898:f0:f09b::1000
              timeout: 2
              state: present


 
Simple playbook to setup system, ``leaf.yaml``:

    - hosts: leaf1
      roles:
         - Dell-Networking.dellos-system

Then run with:

    ansible-playbook -i hosts leaf.yaml

License
--------

Copyright (c) 2017, Dell Inc. All rights reserved.
 
Licensed under the Apache License, Version 2.0 (the "License");
you may not use this file except in compliance with the License.
You may obtain a copy of the License at
 
    http://www.apache.org/licenses/LICENSE-2.0
 
Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and
limitations under the License.
