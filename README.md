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

Certain settings in Ansible are adjustable via a configuration file. See https://github.com/ansible/ansible/blob/devel/examples/ansible.cfg for very complete template.
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

```

### Add a new machine on a different cloud. Inventory groups.

### Satisfy your needs by overiding role variables.

### Templates, conditionals and loops and accessing variables from other hosts.