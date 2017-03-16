iptables
=========

Configures iptables using ufw.

Requirements
------------

None.

Role Variables
--------------

`_iptables` configures the rules for interfaces, ports, and port ranges; see example Playbook for details.

If sysdig is part of the tools list, the corresponding Draios repository is automatically added.

Dependencies
------------

None.

Example Playbook
----------------

```
- hosts: localhost
  connection: local
  become: yes
  vars:
    RW_APT_CACHE_UPDATE: yes
    IPTABLES:
      default_config: no
      default_policy:
        incoming: deny
      enabled: No
      rules:
        - interface: eth0
          dest_ip: "{{ ansible_default_ipv4.address }}"
          ports:
            - { name: ftp, port: 21 }
            - { name: ftp_desktop_client, port: 2181 }
            - { name: ssh, port: 22, proto: tcp, direction=in, rule=allow, delete: no }
            - { name: http, port: 80 }
            - { name: https, port: 443 }
            - { name: bosun, port: 8070 }
            - { name: bosun_te, port: 8170 }
          port_ranges:
            - { name: ftp_data, port: "1421:1423" }
        - interface: "br+"
          src_ip: 172.16.0.0/12
          dest_ip: 172.17.42.1
          ports:
            - { name: smtp, port: 25 }
          port_ranges: []
  roles:
    - { role: ansible-role-iptables, tags: ['iptables'], _iptables: "{{ IPTABLES }}" }
```

License
-------

See LICENSE file.

Author Information
------------------

Initially created by Lukas Pustina [@drivebytesting](https://twitter.com/drivebytesting) as member of the [Rheinwerk](https://github.com/Rheinwerk) project.

