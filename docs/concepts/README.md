# What is Ansible?

https://www.ansible.com/quick-start-video

Ansible is a language to describe infrastructure expectations.

It's Human readable

Agent-less architecture just ssh + python interpreter

Automation engine that runs playbooks.


# Inventory

File where you declare the list of hosts that will match the expectations declared in a playbook.

You can specify meaninful groups of hosts in order to decide what systems you are controlling at what times and for what purpose.

You can specify group variables or host variables that will be available later in playbooks and [control how ansible interacts with remote hosts](http://docs.ansible.com/ansible/intro_inventory.html#list-of-behavioral-inventory-parameters.

http://docs.ansible.com/ansible/intro_inventory.html

http://docs.ansible.com/ansible/intro_dynamic_inventory.html

http://docs.ansible.com/ansible/intro_patterns.html


# Playbook - Roles, tasks and modules

A playbook is a yml file where you describe the desired state of a host or a group of hosts declared in the inventory.

Ansible ships with a [list of modules](http://docs.ansible.com/ansible/list_of_all_modules.html)

You can create a task using the module that satisfy you necessity.

You can encapsulate a group of meaningfully related tasks in a role.

You can apply roles to hosts in the playbook.

Playbook -> role -> task -> module


# Variables

Ansible provides a mechanisim for overriding variables.

You [can go deeply](http://docs.ansible.com/ansible/playbooks_variables.html) on this but a useful guideline to begging with is:

cli extra-vars -> host_vars/hostname.yml -> group_vars/group_name.yml -> group_vars/all.yml -> role defaults



# Templates

[Templates](http://docs.ansible.com/ansible/template_module.html) are a powerful resource for generating files or scripts on the hosts

A template will have a common structure and it will be populated with specify values at runtime.


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

```ansible all -i inventory -m setup```

```ansible all -i inventory -m ping```

```ansible all -i inventory -a ls```

```ansible-playbook --list-host -i inventory playbook.yml```

In your playbook:

```
 - hosts: all
   tasks:
   - name: Display all variables/facts known for a host
     debug: var=hostvars
```