# ansible

* Ansible is the automation tool or framework used for configuration management.

* It uses SSH to connect to remote servers and execute tasks.

* It is agentless, meaning you don't need to install any software on the managed nodes.

  
## Create Control Machine(1) and Target Machine(1 or more)

## Login to Control Machine:
via ssh :
* `ssh -i keypair.pem -o StrictHostKeyChecking=no ec2-user@ipofcontrolmachine`

* Install Python3 and pip.
* By default python is available in ec2 instance.

To install pip3:
* `sudo yum -y install python-pip`

Install ansible:
* `pip3 install ansible`

Check ansible version:
* `ansible --version`

## Create ansible-example (Any name) folder:
Create 2 files in the ansible-example folder and keypair of target machine(s)
* touch inventory
* touch ansible.cfg
* kepair.pem (Target machine(s) keypair)
* chmod 400 keypair.pem


# Open inventory file and add target hosts (3 Ways)
## vim inventory
 1) With IP
  * `ipoftargetmachine ansible_user=ec2-user ansible_ssh_private_key_file=/home/ec2-user/ansible-example/keypair.pem`
  * `ansible_ssh_common_args='StrictHostKeyChecking=no'`
    
2) With Target machine alias name (Here server is alias name)
* `server ansible_host=ipoftargetmachine ansible_user=ec2-user ansible_ssh_private_key_file=/home/ec2-user/ansible-example/keypair.pem`
* `ansible_ssh_common_args='StrictHostKeyChecking=no'`

3) Group of Target machines
* `[dev]`
* `ip1`
* `ip2`
* `[dev:vars]`
* `ansible_user=ec2-user ansible_ssh_private_key_file=/home/ec2-user/ansible-example/keypair.pem`
* `ansible_ssh_common_args='StrictHostKeyChecking=no'`


# Open ancible.cfg and add inventory file path
## vim ancible.cfg

* `[defaults]`
* `Inventory=./inventory`

# To ping Target machines

* `ansible ipoftargetmachine -m ping`
* `ansible all -m ping`
* `ansible server -m ping`
* `ansible dev -m ping`
* ansible -a "free -h" server
* ansible -a "uptime" server


# Install Packages in target machines from Control Machine 
## To install git in all the machines

# 1 Ad-Hoc Commands
`ansible all -m yum -a "name=git state=present" --become`

# 2 Playbooks: Your Automation Scripts

* Playbooks are written in YAML and define a set of tasks to be executed on managed nodes. 
* Playbooks provide a structured and reusable way to automate complex configurations and deployments.


```
---
  - name: Playbook
    hosts: dev
    become: yes
    become_user: root
    tasks:
      - name: ensure git is at the latest version
        yum:
          name: git
          state: latest
      - name: ensure apache is at the latest version
        yum:
          name: httpd
          state: latest
      - name: ensure apache is running
        service:
          name: httpd
          state: started






