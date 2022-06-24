Ansible Role: Guacamole Server (gaucd)
=========

This Ansible role will configure Apache Guacamole Server (guacd) on Debian 11. 

The Guacamole Server is a C application that speaks the guacamole protocol. It listens on (default) port 4822 and the Guacamole Client connects to it to tell it to connect to the Connections configured using RDP/SSH/Telnet etc. In essence guacd is an RDP/SSH/Telnet Client, remotely controlled by the guacamole client (a Tomcat Java application).

A quick flow of how a typical "exploded" Guacamole deployment works:

User(Browser) -> haproxy:443 -> Tomcat(Gauacamole war):8080 -> guacd:4822 -> RDP/SSH/Telnet connections :3389/:22/:23

Each of these are seperate machines, with a firewall in the way, allowing only the ports listed above. Guacamole client will also need :3306 access to mysql for example too. Gaucd doesn't need mysql access, it gets told what to do by guacamole client only.

If you use the sol1-guacamole-mysql role (almost certainly need to!), that role will create a default user for this application of 'guacadmin/guacadmin', so immediately login and change the password for this account and store it in your favourite break-glass password manager. You may need this in case your other authentication methods don't work first time.

Requirements
------------

Works with Ansible 2.4.

Requires `become` or running as the `root` user. You can use `--ask-become-pass` when running. (often we use this with -kKb)

Role Variables
--------------
The following variables are set in `defaults/main`:

| Variable                 | Description                  |
|--------------------------|------------------------------|
|guacamole_version         | Guacamole version to install |
|guacamole_db_user         | Guacamole MariaDB username   |
|guacamole_db_password     | Guacamole MariaDB password   |
|guacamole_db_name         | Guacamole MariaDB database   |
|mysql_java_client_version | MySQL Java Client version    |
|guacamole_apt_install     | Apt packages to install      |

Example Playbook
----------------

```
- hosts: guacamole-host
  become: yes
  roles:
    - alexfeig.guacamole
```

 Information
------------------

This role was created by [Alex Feigenson](https://github.com/alexfeig) and updated by [Sol1](https://sol1.com.au).

To Do List
------------------

