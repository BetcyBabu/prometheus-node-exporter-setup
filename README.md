## Usage
------------------

Run ```ansible-galaxy install betcybabu.prometheus_node_exporter_setup```

```
[root@ip-172-31-32-205 ~]# ansible-galaxy install betcybabu.prometheus_node_exporter_setup
- downloading role 'prometheus_node_exporter_setup', owned by betcybabu
- downloading role from https://github.com/BetcyBabu/prometheus-node-exporter-setup/archive/main.tar.gz
- extracting betcybabu.prometheus_node_exporter_setup to /root/.ansible/roles/betcybabu.prometheus_node_exporter_setup
- betcybabu.prometheus_node_exporter_setup (main) was installed successfully
[root@ip-172-31-32-205 ~]#
```

Edit /etc/ansible/ansible.cfg and update roles_path to /root/.ansible/roles

```
[root@ip-172-31-32-205 ~]# grep roles_path  /etc/ansible/ansible.cfg
#roles_path    = /etc/ansible/roles
roles_path =/root/.ansible/roles
[root@ip-172-31-32-205 ~]#
```

Create inventory file, hosts

example: 

```
[root@ip-172-31-32-205 ~]# cat hosts
[server]
172.31.47.108 ansible_user="ec2-user" ansible_private_key_file="key.pem"
[targets]
172.31.45.52 ansible_user="ec2-user" ansible_private_key_file="key.pem"
172.31.35.159 ansible_user="ec2-user" ansible_private_key_file="key.pem"
[root@ip-172-31-32-205 ~]#
````

Create a playbook, main.yml 

```
---
- name: "Installation started"
  become: true
  hosts: all
  vars:
    URL_prometheus: https://github.com/prometheus/prometheus/releases/download/v2.32.1/prometheus-2.32.1.linux-amd64.tar.gz
    prometheus_version: 2.32.1
    prometheus_arch: amd64
    node_exporter_URL: https://github.com/prometheus/node_exporter/releases/download/v1.3.1/node_exporter-1.3.1.linux-amd64.tar.gz
    node_exporter_version: 1.3.1
    node_exporter_arch: amd64
    targets: [172.31.35.159, 172.31.45.52]
  roles:
    - betcybabu.prometheus_node_exporter_setup

```

Testing connectivity to servers registered in hosts file,

```
ansible -i hosts all -f1 -m ping
```

Syntax checking

```
[root@ip-172-31-32-205 ~]# ansible-playbook -i hosts main.yml --syntax-check

playbook: main.yml
[root@ip-172-31-32-205 ~]#
```

Running playbook

```
ansible-playbook -i hosts main.yml
```

Output

```
PLAY RECAP **********************************************************************************************************************************************************************************
172.31.35.159              : ok=8    changed=7    unreachable=0    failed=0    skipped=11   rescued=0    ignored=0
172.31.45.52               : ok=8    changed=7    unreachable=0    failed=0    skipped=11   rescued=0    ignored=0
172.31.47.108              : ok=12   changed=11   unreachable=0    failed=0    skipped=7    rescued=0    ignored=0
```


![image](https://user-images.githubusercontent.com/23291976/148403502-b0135ac5-3eba-43d0-b76f-262a1d7c23d1.png)








