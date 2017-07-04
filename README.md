System Role for Dell EMC Networking OS
======================================

This role facilitates the configuration of global system attributes. This role is abstracted for dellos6, dellos9, and dellos10. It specifically enables configuration of hostname, NTP server and enable password for all three dellos. 
In addition to this dellos9 supports the configuration of management route, hash alogrithm, clock, line terminal, banner and reload type.


Installation
------------

```
ansible-galaxy install Dell-Networking.dellos-system
```

Requirements
------------
This role requires an SSH connection for connectivity to your Dell EMC Networking device. You can use any of the built-in Dell EMC Networking OS connection variables, or the ``provider`` dictionary.

Role Variables
--------------

Any role variable with a corresponding state variable set to absent negates the configuration of that variable. For variables with no state variable, setting an empty value for the variable negates the corresponding configuration.

The variables and its values are case-sensitive.

``dellos_system`` contains the following keys:

|        Key | Type                      | Notes                                                                                                                                                                                     |
|------------|---------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| unique_hostname | boolean: true, false* | Configures unique hostname in the blade switch. This key is only applicable for blade switch (MXL) in dellos9 devices.  |
| enable_password | string              | Configures enable password. This key is not applicable for dellos10. |
| mtu | integer | Configures Maximum Transmission Unit for all interfaces. This key is not applicable for dellos9 and dellos10. |
| sntp | list | Configures the NTP server. See the following sntp.* keys for each list item. |
|         sntp.ip | string (required)         | Configures the IPv4 address for the NTP server. The value must be in the form of A.B.C.D                                                                                             |
|  sntp.state | string, choices: absent, present*     | If set to absent, deletes the NTP server.                   |
| management_rt | list | Configures the management routes. This key is supported only for dellos9. |
| management_rt.ip | string (required) | Configures IP destination prefix for management route. For IPV4, the value must be in the form of A.B.C.D and for IPv6 the value must be in the form of A:B:C:D::E  |
| management_rt.ipv4 | boolean: true*, false | Specifies management route is an IPv4 or IPv6 address. When this is set to false or undefined, it sets IP as IPv6. |
| management_rt.state | string, choices: absent, present* | If set to absent, deletes the management route.|
| line_terminal | dictionary | Configures the terminal line. This key is not supported in dellos10 and dellos6. See the following line_terminal.* for each item. |
| line_terminal.&lt;terminal&gt; | dictionary | Configures the primary or virtual terminal line. The value can be of following: console &lt;line_number&gt; , vty &lt;line_number&gt;. See the following &lt;terminal&gt;.* for each item. |
| &lt;terminal&gt;.exec_timeout | string | Configures the EXEC timeout. The vlaue can be in the form "&lt;min&gt; &lt;sec&gt;" |
| &lt;terminal&gt;.exec_banner | boolean: true, false* | Configures the EXEC banner. |
| &lt;terminal&gt;.login_banner | boolean: true, false* | Configures the login banner. |
| &lt;terminal&gt;.motd_banner | boolean: true, false* | Configures the motd banner. |
| service_passwd_encryption | boolean: true, false | Configures system password encryption.  |
| hash_algo | dictionary | Configures hash algorithm commands. See the following hash_algo.* keys for each list item. This key is not applicable for dellos6.|
|hash_algo.algo | list         | Configures hashing algorithm. See the following algo.* for each list item.     |
| algo.name | string (required)       | Configures the name of hashing algorithm. |
| algo.mode | string (required)       | Configures the hashing algorithm mode. |
| algo.stack_unit | integer       | Configures stack unit for the hashing algorithm. This key is not supported in dellos10. |
| algo.port_set | integer       | Configures port pipe set for the hashing algorithm. This key is not supported in dellos10. |
| algo.state | string, choices: absent, present*     | If set to absent, deletes the hashing algorithm.            |
|hash_algo.seed | list         | Configures hashing algorithm seed. See the following seed.* for each list item. This key is not applicable for dellos10.  |
| seed.value | integer (required)       | Configures the hashing algorithm seed value. |
| seed.stack_unit | integer       | Configures stack unit for the hashing algorithm seed. |
| seed.port_set | integer       | Configures port pipe set for the hashing algorithm seed. |
| seed.state | string, choices: absent, present*     | If set to absent, deletes the hashing algorithm seed.      |
| banner | dictionary | Configures global banner commands. See the following banner.* keys for each list item. This key is not supported in dellos10 and dellos6. |
| banner.login | dictionary | Configures login banner. See the following login.* keys for each list item. |
| login.ack_enable | boolean: true, false       | Configures positive acknowledgment.  |
| login.ack_prompt | string       | Configures positive acknowledgment prompt.  |
| login.keyboard_interactive | boolean: true, false       | Configures keyboard interactive prompt. |
| login.banner_text | string       | Configures banner text for login banner. The value can be in the format 'c &lt;banner-text&gt; c', where 'c' is a delimiting character. |
| banner.exec | string       | Configures banner text for EXEC process creation banner. For dellos9, the value can be in the format 'c &lt;banner-text&gt; c', where 'c' is a delimiting character. For dellos6, the value can be in the format '" &lt;banner-text&gt; "', where " is a delimiting character. |
| banner.motd | string       | Configures banner text for message of the day banner. The value can be in the format 'c &lt;banner-text&gt; c', where 'c' is a delimiting character. For dellos6, the value can be in the format '" &lt;banner-text&gt; "', where " is a delimiting character. |
| load_balance | dictionary | Configures global traffic load balance. See the following load_balance.* keys for each list item. This key is not supported in dellos10 and dellos6.|
| load_balance.ingress_port | boolean: true, false       | Configure to use source port id for hashing algorithm. |
| load_balance.tcp_udp | boolean: true, false       | Configures to use tcp/udp ports in packets for hashing algorithm. |
| load_balance.ip_selection | string       | Configures IPV4 key fields to use in hashing algorithm. This key is mutually exclusive with load_balance.tcp_udp |
| load_balance.mac_selection | string       | Configures mac key fields to use in hashing algorithm. |
| load_balance.ipv6_selection | string       | Configures IPV6 key fields to use in hashing algorithm. This key is mutually exclusive with load_balance.tcp_udp. |
| load_balance.tunnel | dictionary    | Configures tunnel key fields to use in hashing algorithm. See the following tunnel.* for each item. |
| tunnel.hash_field | list    | Configures hash field selection. See the following hash_field.* for each item. |
| hash_field.name | string (required)       | Configures the hash field selection. |
| hash_field.header | string       | Configures header for load balance. |
| hash_field.state | string, choices: absent, present*     | If set to absent, deletes the hash key selection field      |
| clock | dictionary | Configures time-of-day clock. See the following clock.* keys for each list item.This key is not supported in dellos10 and dellos6. |
| clock.summer_time | dictionary    | Configures summer(day light) time. See the following summer_time.* for each list item.    |
| summer_time.timezone_name | string (required)       | Configures the timezone name. |
| summer_time.type | string (required)       | Configures absolute or recurring summer time. |
| summer_time.start_datetime | string       | Configures start datetime. The value can be in the format &lt;date&gt; &lt;month&gt; &lt;year&gt; &lt;hrs:mins&gt; |
| summer_time.end_datetime | string       | Configures end datetime. The value can be in the format &lt;date&gt; &lt;month&gt; &lt;year&gt; &lt;hrs:mins&gt; |
| summer_time.offset_mins | integer       | Configures offset mins to add. The value can be in range 1-1440.|
| summer_time.state | string, choices: absent, present*     | If set to absent, deletes the summer time clock.     |
| clock.timezone | dictionary    | Configures timezone. See the following timezone.* for each list item.     |
| timezone.name | string (required)       | Configures the timezone name. |
| timezone.offset_hours | integer       | Configures offset hours to add. The value can be in range -23-23.|
| timezone.offset_mins | integer       | Configures offset mins to add. The value can be in range 0-59.|
| timezone.state | string, choices: absent, present*     | If set to absent, deletes the time zone.     |
| reload_type | dictionary | Configures reload type. See the following reload_type.* keys for each list item. This key is not supported in dellos10 and dellos6. |
| reload_type.auto_save | boolean: true, false*     | Configures the auto save option for downloaded config/script file.     |
| reload_type.boot_type | string, choices: bmp-reload,normal-reload    | Configures the boot type.     |
| reload_type.boot_type_state | string, choices: absent, present*    | If set to absent, deletes the boot type.     |
| reload_type.config_scr_download | boolean: true, false*     | Configures whether config/script file needs to be downloaded.     |
| reload_type.dhcp_timeout | integer    | Configures dhcp timeout in mins. The value can be in range 0-50.     |
| reload_type.retry_count | integer    | Configures the number of retries for image and config download. The value can be in range 0-6.     |
| reload_type.relay | boolean: true, false*     | Configures addition of option 82 in DHCP client packets.     |
| reload_type.relay_remote_id | string    | Configures customize remote id.     |
| reload_type.vendor_class_identifier | boolean: true, false*     | Configures vendor-class-identifier for DHCP option 60. |


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

The following example uses the dellos.dellos-system role to completely set the NTP server, hostname, enable password, management route, hash alogrithm, clock, line terminal, banner and reload type. 
The example creates a ``hosts`` file with the switch details and corresponding variables. The hosts file should define the variable `` ansible_net_os_name `` with corresponding Dell EMC networking OS name.
It writes a simple playbook that only references the dellos-system role. 
By including the role, you automatically get access to all of the tasks to configure system features. 


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
	  
    dellos_system:
      unique_hostname: True
      sntp:
        - ip: 2.2.2.2
          state: absent
      service_passwd_encryption: true
      banner:
        exec: t hai t
        login:
          ack_enable: true
          ack_prompt: testbanner
          keyboard_interactive: true
          banner_text: cloginbannerc
        motd: t ansibletest t
      hash_algo:
        algo:
          - name: lag
            mode: xor1
            stack_unit: 0
            port_set: 0
            state: present
          - name: ecmp
            mode: xor1
            stack_unit: 0
            port_set: 0
            state: present
        seed:
          - value: 3
            stack_unit: 0
            port_set: 0
            state: present
          - value: 2
            state: present
      load_balance:
        ingress_port: true
        ip_selection: vlan dest-ip
        ipv6_selection: dest-ipv6 vlan
        tunnel:
          hash_field:
            - name: mac-in-mac
              header: tunnel-header-mac
              state: present
      clock:
        summer_time:
          timezone_name: PST
          type: date
          start_datetime: 2 jan 1993 22:33
          end_datetime: 3 jan 2017 22:33
          offset_mins: 20
        timezone:
          name: IST
          offset_hours: -5
          offset_mins: 20
      reload_type:
        auto_save: true
        boot_type: normal-reload
        boot_type_state: absent
        config_scr_download: true
        dhcp_timeout: 5
        retry_count: 3
        relay: true
        relay_remote_id: ho
        vendor_class_identifier: aa
      management_rt:
        - ip: 10.16.148.254
          state: present
          ipv4: True
      line_terminal:
        vty 0:
          exec_timeout: 40
          exec_banner: true
        vty 1:
          exec_timeout: 40 200
 
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
