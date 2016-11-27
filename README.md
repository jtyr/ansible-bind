bind
====

Role which helps to install and configure Bind DNS server.

The configuration of the role is done in such way that it should not be necessary
to change the role for any kind of configuration. All can be done either by
changing role parameters or by declaring completely new configuration as a
variable. That makes this role absolutely universal. See the examples below for
more details.

Please report any issues or send PR.


Example
-------

```
---

- name: Default installation
  hosts: myhost1
  roles:
    - bind
```


Role variables
--------------

List of variables used by the role:

```
# Package to be installed (explicit version can be specified here)
bind_pkg: bind

# Name of the Bind service
bind_service: named

# Location of the sysconfig file
bind_sysconfig_file: /etc/sysconfig/named

# Content of the sysconfig file
bind_sysconfig: {}
# Example:
#bind_sysconfig:
#  rootdir: /var/named/chroot
#  options: whatever
#  keytab_file: /dir/file
#  disable_zone_checking: yes


# Location of the main Bind config file
bind_config_file: /etc/named.conf

# Final Bind config file
bind_config:
  - options:
    - listen-on port 53:
      - 127.0.0.1
    - listen-on-v6 port 53:
      - "::1"
    - directory "/var/named"
    - dump-file "/var/named/data/cache_dump.db"
    - statistics-file "/var/named/data/named_stats.txt"
    - memstatistics-file "/var/named/data/named_mem_stats.txt"
    - allow-query:
      - localhost
    - recursion yes
    - dnssec-enable yes
    - dnssec-validation yes
    - bindkeys-file "/etc/named.iscdlv.key"
  - logging:
    - channel default_debug:
      - file "data/named.run"
      - severity dynamic
  - zone "." IN:
    - type hint
    - file "named.ca"
  - include "/etc/named.rfc1912.zones"
  - include "/etc/named.root.key"
```


Dependencies
------------

- [`config_encoder_filters`](https://github.com/jtyr/ansible-config_encoder_filters)


License
-------

MIT


Author
------

Jiri Tyr
