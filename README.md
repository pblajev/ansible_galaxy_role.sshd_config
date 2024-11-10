Role ansible_galaxy_role.sshd_config
=========

The role will look for directory /etc/ssh/sshd_config.d/.<br>
* If the directory exists then the role will edit the file homeco-sshd.conf in that directory.<br>The file will be created if it doesn't exists.<br>
* If the directory doesn't exists then the role will edit /etc/ssh/sshd_config file.<br>

SSHD will be restarted if there is a change in the configuration files.

The sshd settings that the role will apply are defined in the role:<br>
```text
PasswordAuthentication no
PermitRootLogin no
```

These settings can be overridden or amended by calling the role with a custom list of settings.<br>
For example:<br>

List `sshd_settings_default` defined in the role:
```text
sshd_settings_default:
  - setting: PasswordAuthentication
    value: "no"
  - setting: PermitRootLogin
    value: "no"
```
List `sshd_settings` provided when the role was called:
```text
    - role: ansible_galaxy_role.sshd_config
      vars:
        sshd_settings:
          - setting: "X11Forwarding"
            value: "no"
          - setting: "PasswordAuthentication"
            value: "yes"
```
The two lists will be merged and the final list will be:
```text
settings_list
  - setting: PasswordAuthentication
    value: "yes"
  - setting: PermitRootLogin
    value: "no"
  - setting: "X11Forwarding"
    value: "no"
```
---
Requirements
------------

Tested on Ubuntu, Debian, RedHat, Amazon Linux <br><br>

---
Role Variables
--------------

| Parameter       | Choices | Description                           |
|-----------------|---|---------------------------------------|
| `sshd_settings` | [] <-- | **Optional**<br>List of sshd settings |

---
## `sshd_settings` details

| Variable     | Required | Description                               |
|--------------|----------|-------------------------------------------|
| `setting`    | yes      | SSHD Setting                              
| `value`      | yes      | SSHD setting value. Usually `yes` or `no` |

###### Example `sshd_settings`

```yaml
sshd_settings:
  - setting: PermitRootLogin
    value: "no"
  - setting: X11Forwarding
    value: "no"
```
***NOTE:*** The values `yes` and `no`, always must be in quotes.
<br><br>

---
Example Playbook
----------------

```yaml
- name: Apply the sshd settings defined in the role
  hosts: all
  roles:
    - role: ansible_galaxy_role.sshd_config
```

```yaml
- name: Define custom sshd settings or override the ones in the role
  hosts: all
  roles:
    - role: ansible_galaxy_role.sshd_config
      vars:
        sshd_settings:
          - setting: "X11Forwarding"
            value: "no"
          - setting: "PasswordAuthentication"
            value: "yes"

```