# TP PART 03

## Inventory 

The inventory should be at ansible/inventories/inventory.yml inside of the project. Here is the content of my inventory at this point :

```yml
all:
 vars:
   ansible_user: centos # user on the server
   ansible_ssh_private_key_file: /home/robin/DevOps/id_rsa #path to the ssh private key
 children:
   prod:
     hosts: robinjarnoux.takima.cloud #server IP or name

```

## Commands

Here are some useful commands that I has to use

`ansible all -i path/to/setup.yml -m ping`

- -i is an option to specify the path of the inventory 
- -m is module path, here we are using the built in ping module
- all means we are running this on all the groups defined in the inventory

`ansible all -i inventories/setup.yml -m setup -a "filter=ansible_distribution*"`


- same thing but with the built in setup module , it gather useful variables about the remote host
- all does this on all hosts
- -a is to specify module arguments
- filter is a module argument, only the facts matching the pattern are returned

## Roles

`ansible-galaxy init roles/docker`

Creates a docker role

## Playbook

First we have playbook.yml

```yml
- hosts: all
  gather_facts: false
  become: yes
  roles:
    - docker
```

- we use the all keyword for the hosts
- we call the role docker from our playbook


Then in roles/docker/tasks/main.yml we have the docker setup list of tasks itself. Most of the code is self explanatory. Comments are added when more information is needed.


```yml
---
# tasks file for roles/docker
- name: Clean packages
  command:
    cmd: yum clean -y packages

- name: Install device-mapper-persistent-data
  yum:
    name: device-mapper-persistent-data
    state: latest

- name: Install lvm2
  yum:
    name: lvm2
    state: latest

- name: add repo docker
  command:
    cmd: sudo yum-config-manager --add-repo=https://download.docker.com/linux/centos/docker-ce.repo

- name: Install Docker
  yum:
    name: docker-ce
    state: present

- name: install python3
  yum:
    name: python3

- name: Pip install
  pip:
    name: docker
    executable: pip3 # this is necessary because pip isn't available on CentOS so we use pip3

- name: Make sure Docker is running
  service:
    name: docker
    state: started
  tags: docker

```

### Deploy your app