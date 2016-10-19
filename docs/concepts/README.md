# What is Ansible?

https://www.ansible.com/quick-start-video

Ansible is a language to describe infrastructure expectations.

It's human readable.

It has agent-less architecture, it just needs ssh + python interpreter.

It's an automation engine that runs playbooks on the machines specified in the inventory.

You can run it from the command line like:

```ansible-playbook -i inventory playbook.yml```

# Inventory

File where you declare the list of hosts that you want to match the expectations declared in a playbook.

You can specify meaningful groups of hosts in order to decide what systems you are controlling at what times and for what purpose.

You can specify group variables or host variables that will help to [control how ansible interacts with remote hosts](http://docs.ansible.com/ansible/intro_inventory.html#list-of-behavioral-inventory-parameters) and they will be available later in playbooks.

```
aws01 ansible_ssh_host=52.49.153.19

[coreos]
aws01
```

http://docs.ansible.com/ansible/intro_inventory.html

http://docs.ansible.com/ansible/intro_dynamic_inventory.html

http://docs.ansible.com/ansible/intro_patterns.html


# Playbook - Roles, tasks and modules

A playbook is a yml file where you describe the desired state of a host or a group of hosts declared in the inventory.

Ansible ships with a [list of modules](http://docs.ansible.com/ansible/list_of_all_modules.html).

E.g: [File module](http://docs.ansible.com/ansible/file_module.html)

You can create a task using a module that satisfy you necessity.

E.g: [Create a file in a given folder, with specific user and permissions.](http://docs.ansible.com/ansible/file_module.html#examples)

```- file: path=/etc/foo.conf owner=foo group=foo mode=0644```

You can encapsulate a group of meaningfully related tasks in a role.

E.g: [Create files and running services for configuring a Weave](https://github.com/Capgemini/weave-ansible)

You can apply roles to hosts in the playbook.

```
---
- hosts: weave_servers
  roles:
    - weave
```

Playbook -> role -> task -> module


# Variables

Ansible provides a mechanisim for overriding variables.

You [can go deeply](http://docs.ansible.com/ansible/playbooks_variables.html) on this but a useful guideline to beging with is:

cli extra-vars -> host_vars/hostname.yml -> group_vars/group_name.yml -> group_vars/all.yml -> role defaults



# Templates

[Templates](http://docs.ansible.com/ansible/template_module.html) are a powerful resource for generating files on the hosts

Templates are processed by the [Jinja2 templating language](http://jinja.pocoo.org/docs/) 

A template will have a common structure and it will be populated with specify variable values at runtime.

[weave.env.j2](https://github.com/enxebre/ansible-pragmatic-guide/blob/master/roles/weave/templates/weave.env.j2)

```
WEAVE_PEERS="{{ weave_launch_peers }}"
WEAVEPROXY_ARGS="{{ weave_proxy_args }}"
WEAVE_ROUTER_ARGS="{{ weave_router_args }}"
# Uncomment and make it more secure
# WEAVE_PASSWORD="aVeryLongString"
```

# Conditionals and loops

You can run roles or tasks depending on a conditional statement.

```
tasks:
  - name: "shut down Debian flavored systems"
    command: /sbin/shutdown -t now
    when: ansible_os_family == "Debian"
    # note that Ansible facts and vars like ansible_os_family can be used
    # directly in conditionals without double curly braces
```

http://docs.ansible.com/ansible/playbooks_conditionals.html#loops-and-conditionals


# Rolling upgrades

[You can run ansible in serial and have control on how many servers you want to run it at one time.](http://docs.ansible.com/ansible/guide_rolling_upgrade.html)


# Debugging

Gather useful variables about remote hosts that can be used in playbooks:

```ansible all -i inventory -m setup```

Check your machines are reachable. This returns pong on successful contact:

```ansible all -i inventory -m ping```

Run a bash command on the machines remotely:

```ansible all -i inventory -a ls```

See how your playbook will apply to the your hosts:

```ansible-playbook --list-host -i inventory playbook.yml```

Print all variables/facts known for a host by adding this into your playbook:

```
 - hosts: all
   tasks:
   - name: Display all variables/facts known for a host
     debug: var=hostvars
```

[You can also run Ansible in "check mode"](http://docs.ansible.com/ansible/playbooks_checkmode.html)
