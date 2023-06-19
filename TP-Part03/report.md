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