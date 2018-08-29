Dynamically create host records in Infoblox using Ansible!

Create zones and add host records at the next available ip addresses using a Name Server Group

Requirements
------------

The infoblox-client installed on the targeted localhost machine. Ansible Core >= v2.5 for the infoblox modules and lookup plugin.

Role Variables
--------------
Example nios_provider supplied below. This should be vaulted in group_vars/localhost/main.yml for production use.

```yaml
nios_provider:
   #Out-of-the-box defaults specified here
   host: 192.168.1.2
   username: admin
   password: infoblox

wapi_version: 'v2.6'
```

Example Playbooks
-----------------
Most automation functionality in this repository is better demonstrated by calling of roles (see below).

Here are some of the common overrides at the playbook level of this repository's core role, dynamicInfoblox:

```
ansible-playbook create_dynamic_records.yml
ansible-playbook create_dynamic_records.yml -e "host_count=10"
ansible-playbook create_dynamic_records.yml -e "ansible_zone=redhat.com"
ansible-playbook create_dynamic_records.yml -e "ansible_subnet=10.10.10.0/24"
ansible-playbook create_dynamic_records.yml -e "static_csv=True"
```

Role Calls
-----------------
Overrides at the role level allow single playbooks to call the same roles in succession with new network information.

The default invocation creates a forward/reverse zone, subnet, and gateway address using out-of-the-box configurations, but does not generate additional hosts:
```yaml
    - hosts: localhost
      connection: local
      roles:
         - { role: dynamicInfoblox }
```
Specify a host_count to create (several) new host records at a time:
```yaml
         - { role: dynamicInfoblox, host_count: 10 }
```
Override the default zone:
```yaml
         - { role: dynamicInfoblox, ansible_zone: redhat.com }
```
Override the default subnet. The default gateway_address is automated to reflect changes overriden here:
```yaml
         - { role: dynamicInfoblox, ansible_subnet: 10.10.10.0/24 }
```
Import records using Infoblox's static csv import feature
```yaml
         - { role: dynamicInfoblox, static_csv=True }
```

Author Information
------------------
```
Branden Pleines
Bret Pleines
```
