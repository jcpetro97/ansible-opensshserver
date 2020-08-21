# Openssh Server

Role to install and configure the openssh server.  

#### Variables
Most of the relevant variables can be seen in the Defaults/main.yml file

SshMatchRules:  This is the variable you can set to include rules to the sshd_config.  The variable can be set in the host_vars directory, group_vars or in the playbook.  These extra rules will be added to the **END** of the /etc/ssh/sshd_config file

I have also configured the sshd_config template, so that it will process extra rules from host_vars separately from group_vars, defaults or from defining the rules directly at execution time.

#### Example Playbook Execution

**NOTE:** _The following example is the role being called without any extra rules_

```
- hosts: all
  become: true
  roles:
      - role: OpensshServer

```



**NOTE:** _The following example is the role being called from a playbook, and adding extra rules that are defined when te role is called at execution time_

```
- hosts: all
  become: true
  roles:
      - role: OpensshServer
        vars:
          - SshMatchRules:
            - Match: "User from_playbook"
              Options: |
                 X11Forwarding no
                 AllowTcpForwarding no
                 ForceCommand cvs server

```

**group_vars/\<group name or all> or host_var/\<hostname from inventory file>/var**

_This is how the group_vars file or host_vars should be formatted to be included in the role execution.  If you want this rule applied to all servers, you can put it in the **group_vars/all** file.  If you want it applied to specific hosts, then it should go in the **host_vars/\<hostname>/vars** file_

**group_vars/\<group name or all>**
```
SshMatchRules:
    - Match: "User testuser"
      Options: |
        X11Forwarding no
        AllowTcpForwarding no

```

**host_vars/\<hostname>/vars**
```
HostSshMatchRules:
    - Match: "User testuser"
      Options: |
        X11Forwarding no
        AllowTcpForwarding no

```