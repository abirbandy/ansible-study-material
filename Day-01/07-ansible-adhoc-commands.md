# Ansible Adhoc Commands

## Why to use adhoc commands?
Why need a complex Ansible playbook when one easy command can achieve it, or for some quick tasks?

[https://docs.ansible.com/ansible/latest/command_guide/intro_adhoc.html](https://docs.ansible.com/ansible/latest/command_guide/intro_adhoc.html)

Ad hoc tasks can be used to reboot servers, copy files, manage packages and users, and much more. You can use any Ansible module in an ad hoc task. Ad hoc tasks, like playbooks, use a declarative model, calculating and executing the actions required to reach a specified final state. They achieve a form of idempotence by checking the current state before they begin and doing nothing unless the current state is different from the specified final state.

The default module for the `ansible` command-line utility is the `ansible.builtin.command` module.

## Rebooting servers

```
$ ansible localhost -a "/sbin/reboot"
```

## Managing files
```
$ ansible atlanta -m ansible.builtin.copy -a "src=/etc/hosts dest=/tmp/hosts"
$ ansible webservers -m ansible.builtin.file -a "dest=/srv/foo/a.txt mode=600"
$ ansible webservers -m ansible.builtin.file -a "dest=/srv/foo/b.txt mode=600 owner=mdehaan group=mdehaan"
```

The file module can also create directories, similar to mkdir -p:
```
$ ansible webservers -m ansible.builtin.file -a "dest=/path/to/c mode=755 owner=mdehaan group=mdehaan state=directory"
$ ansible webservers -m ansible.builtin.file -a "dest=/path/to/c state=absent"
```

## Managing packages
You might also use an ad hoc task to install, update, or remove packages on managed nodes using a package management module such as yum. 
Package management modules support common functions to install, remove, and generally manage packages.
Some specific functions for a package manager might not be present in the Ansible module since they are not part of general package management.

To ensure a package is installed without updating it:
```
$ ansible webservers -m ansible.builtin.yum -a "name=acme state=present"
```
To ensure a specific version of a package is installed:
```
$ ansible webservers -m ansible.builtin.yum -a "name=acme-1.5 state=present"
```
To ensure a package is at the latest version:
```
$ ansible webservers -m ansible.builtin.yum -a "name=acme state=latest"
```
To ensure a package is not installed:
```
$ ansible webservers -m ansible.builtin.yum -a "name=acme state=absent"
```
## Managing users and groups
You can create, manage, and remove user accounts on your managed nodes with ad hoc tasks:
```
$ ansible all -m ansible.builtin.user -a "name=foo password=<encrypted password here>"
$ ansible all -m ansible.builtin.user -a "name=foo state=absent"
```
## Managing services
Ensure a service is started on all webservers:
```
$ ansible webservers -m ansible.builtin.service -a "name=httpd state=started"
```
Alternatively, restart a service on all webservers:
```
$ ansible webservers -m ansible.builtin.service -a "name=httpd state=restarted"
```
Ensure a service is stopped:
```
$ ansible webservers -m ansible.builtin.service -a "name=httpd state=stopped"
```
## Gathering facts
Facts represent discovered variables about a system. You can use facts to implement conditional execution of tasks but also just to get ad hoc information about your systems. To see all facts:
```
$ ansible all -m ansible.builtin.setup
```
You can also filter this output to display only certain facts, see the ansible.builtin.setup module documentation for details.

## Check mode
In check mode, Ansible does not make any changes to remote systems. Ansible prints the commands only. It does not run the commands.
```
$  ansible all -m copy -a "content=foo dest=/root/bar.txt" -C
```
Enabling check mode (-C or --check) in the above command means Ansible does not actually create or update the /root/bar.txt file on any remote systems
