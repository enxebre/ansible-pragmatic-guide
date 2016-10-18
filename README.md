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

Before reinventing the wheel you can try yo reuse.
Anisble galaxy is a website for sharing and downloading Ansible roles and a command line tool for managing and creating roles. You can download roles from Ansible galaxy or form your specific git repository.
Anisble allows you to define your dependencies with standalone roles in a yaml file. See ```requirements.yml```

```
- src: defunctzombie.coreos-bootstrap
  name: coreos_bootstrap
```

Certain settings in Ansible are adjustable via a configuration file. See https://github.com/ansible/ansible/blob/devel/examples/ansible.cfg for very complete template.
We'll set here the target folder for our community roles

Just run ```ansible-galaxy install -r requirements.yml```

### Boostrap ansible dependencies on a single CoreOS machine. The Inventory and the Playbook.

### Add a new machine on a different cloud. Inventory groups.

### Satisfy your needs by overiding role variables.

### Templates, conditionals and loops and accessing variables from other hosts.