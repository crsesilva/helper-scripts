# To build Ansible FortiOS module support 

Create virtualenv

```shell
mkdir ~/fortinet-ansible
cd ~/fortinet-ansible
python3 -m venv .venv --prompt ansible-fortios
source .venv/bin/activate
```

Install Ansible

```shell
pip install ansible
```

Install Fortinet Fortios collections (version 2.4.0)

```shell
ansible-galaxy collection install fortinet.fortios:2.4.0
```

Or most recent

```shell
ansible-galaxy collection install fortinet.fortios
```

Now, test if module exist and installed

```shell
ansible-doc -t connection -l
```


```shell
ansible-doc -t connection -l | grep fortios
```

or 

```shell
ansible-galaxy collection list fortinet.fortios
```

Install Ansible netcommon module

```shell
ansible-galaxy collection install ansible.netcommon --force
```

Check Ansible variables

```shell
env | grep -i ansible
```

Create the `ansible.cfg` file

```ini
[defaults]
collections_path = ~/.ansible/collections
collections_scan_sys_path = False
```


Set Ansible inventory (inventory/fortinet)

```ini
[G_FORTINET]
fortinet01 ansible_host=192.168.10.98

[G_FORTINET:vars]
ansible_connection=httpapi
ansible_httpapi_use_ssl=yes
ansible_httpapi_validate_certs=no
ansible_network_os=fortinet.fortios.fortios
ansible_user=admin
ansible_password=123456
```

Create playbook to change hostname

```yaml
- hosts: G_FORTINET
  gather_facts: no
  tasks:
  - name: Set Fortinet hostname
    fortinet.fortios.fortios_system_global:
        system_global:
            hostname: teste-host
```

Run playbook

```shell
ansible-playbook -i inventory/fortinet fortios.yml
```

Output

```shell
$ ansible-playbook -i inventory/fortinet fortios.yml 

PLAY [fortigay] ********************************************************************************************************************************************************************************************************

TASK [Set Fortinet hostname] ************************************************************************************************************************************************************************************************************************
ok: [fortinet01]

PLAY RECAP *************************************************************************************************************************************************************************************************************
fortinet01                 : ok=1    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   
```


# Verify Ansible configuration

```shell
ansible-config dump | grep -i collection
```

