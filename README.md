# Ansible workshop

This repo is a beginner guide to Ansible.

It will show you the main concepts and its benefits.

The repo has several git tags starting with the simplest ansible approach and will become a bit more sophisticated every time.

At the end of this tutorial you'll have automated the set up of a cross-cloud software defined network for containers using Weave net.

We'll deploy two containers one in DigitalOcean, another one in AWS that will communicate with each other.

This tutorial will assume that you have two machines running coreOS on DigitalOcean and AWS.

You can create them manually using the browser, or use something like terraform or docker-machine. We provide the docker-machine-bootstrap so you can use it and modify it for this purpose.

## Index

### Theory

What is it?

Inventory

Playbook - Roles, tasks and modules

Variables

Templates

Conditionals and loops

Debugging

Rolling upgrades


## Implementation steps

### Download dependencies. Ansible galaxy.

```git tag step-1```

Before reinventing the wheel you can try yo reuse.
Anisble galaxy is a website for sharing and downloading Ansible roles and a command line tool for managing and creating roles. You can download roles from Ansible galaxy or form your specific git repository.
Anisble allows you to define your dependencies with standalone roles in a yaml file. See ```requirements.yml```

```
- src: defunctzombie.coreos-bootstrap
  name: coreos_bootstrap
```

We'll use CoreOS machines in this tutorial. By default Ansible assumes it can find a /usr/bin/python on your remote system. Coreos machines are minimal and do not ship with any version of python. The [coreos-bootstrap role](https://github.com/defunctzombie/ansible-coreos-bootstrap) will install [pypy](http://pypy.org/) for us.

Certain settings in Ansible are adjustable via a [configuration file](http://docs.ansible.com/ansible/intro_configuration.html). See https://github.com/ansible/ansible/blob/devel/examples/ansible.cfg for very complete template.
We'll set here the target folder for our community roles

In ```ansible.cfg``` you can see:

```
[defaults]
roles_path = roles
```

Just run ```ansible-galaxy install -r requirements.yml```

### Boostrap ansible dependencies for CoreOS. The Inventory and the Playbook.

```git tag step-2```

We'll create an inventory so we can specify the target hosts. You can specify meaninful groups to your hosts in order to decide what systems you are controlling at what times and for what purpose by running a specific role/task for a given group.

You can also specify variables for groups. We set the CoreOS specific here.

```
do01 ansible_ssh_host=138.68.144.191

[coreos]
do01

[coreos:vars]
ansible_python_interpreter="PATH=/home/core/bin:$PATH python"
ansible_user=core


[digitalocean]
do01

[digitalocean:vars]
ansible_ssh_private_key_file=~/.docker/machine/machines/do-ansible-workshop/id_rsa
```

We'll create a playbook so we can declare our expected configuration for every host.

In this step our ```playbook.yml``` will only include another one running the role with downloaded previewsly on every coreos machine (just one so far).
By default.
```
- name: bootstrap coreos hosts
  hosts: coreos
  gather_facts: False
  roles:
    - coreos_bootstrap
```

Run ansible:

```
ansible-playbook -i inventory playbook.yml
```

### Add a new machine on a different cloud. Inventory groups.

```git tag step-3```

Run

```
ansible all -i inventory -m ping
```

You will see it fail for aws01 as the python interpreter is not there yet. So let's apply the playbook again.

```
ansible-playbook -i inventory playbook.yml
```

Now:

```
ansible all -i inventory -m ping
```

Image goes here

Nice!

### Satisfy your needs by overiding role variables.

```git tag step-4```

So far we have used Ansible to set up a python interpreter for the CoreOS machines so we can run Ansible effectively as many modules rely on python.
In this Step we'll setup a Weave network and Weave Scope between both clouds so docker containers can communicate with ease.

We add a new role dependency on the requirements.

```
- src: defunctzombie.coreos-bootstrap
  name: coreos_bootstrap

- src: https://github.com/Capgemini/weave-ansible
  name: weave
``` 

Run

```
ansible-galaxy install -r requirements.yml
```

We'll modify the inventory to create a group of hosts belonging to the weave network. By using the ["children"](http://docs.ansible.com/ansible/intro_inventory.html#groups-of-groups-and-group-variables) tag you can create a group of groups

```
[weave_servers:children]
digitalocean
aws
```

We'll override the weave role variables for satisfying our needs. Ansible allows to create ansible per host, per group, or site wide variables by setting group_vars/all
In ```group_vars/weave_server.yml```

```
weave_launch_peers: "
{% for host in groups[weave_server_group] %}
  {%- if loop.first %}{% break %}{% endif %}
  {% if host != inventory_hostname %}
    {{ hostvars[host].ansible_ssh_host }}
  {% endif %}
{% endfor %}"

weave_proxy_args: '--rewrite-inspect'
weave_router_args: ''
weave_version: 1.7.2
scope_enabled: true
proxy_env: 
  none: none
```

Add te weave role into our playbook:

```
---
- include: coreos-bootstrap.yml

- hosts: weave_servers
  roles:
    - weave
```

### Templates, conditionals and loops and accessing variables from other hosts.


### Debugging


