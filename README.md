# Openssh Server

Role to install and configure the openssh server.  

#### Variables

| Parameter | Choices / **Defaults** | Comments |
|-----------|------------------------|----------|
|SshServerPort|**22**| Port the ssh server is running on|
|SshProtocol|**2**| ssh protocol version|
|SshHostKeys| _See defaults/main file for default host keys| Host Keys for ssh server| 
|SshSyslogFacility|**AUTH**|What syslog facility to use|
|SshLogLevel|**INFO**|Log Level|
|SshLoginGraceTime|**120**|Time allowed for successful login|
|SshPermitRootLogin|**prohibit-password**|Allowing root to log in directly or not|
|SshStrictModes|Choices:<ul><li>no</li><li>**yes**</li></ul>|Ssh key,config files ownership, permission checks are done before sshd starts|
|SshPubkeyAuthentication|Choices:<ul><li>no</li><li>**yes**</li></ul>|Enable public key auth|
|SshIgnoreRhosts|Choices:<ul><li>no</li><li>**yes**</li></ul>|Ignors the existance of a .rhosts file|
|SshHostbasedAuthentication|Choices:<ul><li>**no**</li><li>yes</li></ul>|allows hosts to authenticate on behalf of all or some of that host's users|
|SshPermitEmptyPasswords|Choices:<ul><li>**no**</li><li>yes</li></ul>|Allow users to log in with empty password|
|SshPasswordAuthentication|Choices:<ul><li>no</li><li>**yes**</li></ul>|Enable/disable password authentication|
|SshChallengeResponseAuthentication|Choices:<ul><li>**no**</li><li>yes</li></ul>|Enable/Disable challenge Response Auth.  Used for things like google authenticator|
|SshAuthorizedKeysFile|**%h/.ssh/authorized_keys**|Location of the authorized_keys file for a user|
|SshX11Forwarding|Choices:<ul><li>no</li><li>**yes**</li></ul>|Enable/disable X11 forwarding|
|SshX11DisplayOffset|**10**|Specifies the first display number available for sshd's X11 forwarding|
|SshPrintMotd|Choices:<ul><li>**no**</li><li>yes</li></ul>|Enable/disable display of the MOTD|
|SshPrintLastLog|Choices:<ul><li>no</li><li>**yes**</li></ul>|Enable/disable printing the date/time of the last user interactive login|
|SshTCPKeepAlive|Choices:<ul><li>no</li><li>**yes**</li></ul>|Enable/disable TCPKeepAlive|
|SshAllowTcpForwarding|Choices:<ul><li>**no**</li><li>yes</li></ul>|Enable/disable AllowTcpForwarding|
|SshPermitTunnel|Choices:<ul><li>**no**</li><li>yes</li></ul>|Enable/disable PermitTunnelfor tunnel devices ( such as VPN)|
|SshUsePAM|Choices:<ul><li>no</li><li>**yes**</li></ul>|Enable/disable the Pluggable Authentication Module interface|
|SshPubKeys|Choices:<ul><li>**no**</li><li>yes</li></ul>|Enable/disable If you are going to use public key auth only|
|SshMatchRules||Match rules for additional configuration which can be found in /etc/ssh/sshd_config.d/09-MatchRule.conf|


#### Example Playbook Execution

**NOTE:** _The following example is the role being called without any extra rules_

```
- hosts: all
  become: true
  roles:
      - role: OpensshServer

```



**NOTE:** _The following example is the role being called from a playbook, and adding extra rules that are defined when the role is called at execution time_

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
**NOTE:** _The following example shows how to have multiple Match Rules as part of the playbook._


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
            - Match: "User testuser"
              Options: |
                X11Forwarding no
                AllowTcpForwarding no

```