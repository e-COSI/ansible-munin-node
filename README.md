# Ansible Role: munin-node

This role applied munin-node installation on Debian Jessie.

## Requirements

This role requires Ansible 2.0 or higher. Requirements are listed in the metadata file.

## Role Variables

| Variable | Required | Default | Comments |
|----------|----------|---------|----------|
| `munin_node_bind_host` | No | `"*"` | Host to which munin-node will bind. |
| `munin_node_bind_port` | No | `"4949"` | Port to which munin-node will bind. |
| `munin_node_host_name` | No | `''` | Set this explicitly if the munin master doesn't report the correct hostname when telnetting in to munin-node. |
| `munin_reset_plugins` | No | `false` | Whether to delete all previous plugins before adding new symlinks. |
| `munin_plugin_src_path` | No | `/usr/share/munin/plugins` | Source of munin plugins. |
| `munin_plugin_dest_path` | No | `/etc/munin/plugins` | Destinations of munin plugins. |

A list of IP addresses formatted in CIDR notations (even if it's full address, you should add /32 at the end). Hosts with these IP addresses will be allowed to connect to the server and get detailed system stats via munin-node:
```
munin_node_cidr_allowed_ips:
  - "127.0.0.1/32"
```
A list of IP addresses formatted as a python-style regular expression. Must use single quotes to allow the proper regex escaping to pass through to the configuration file. Hosts with these IP addresses will be allowed to connect to the server and get detailed system stats via munin-node:
```
munin_node_allowed_ips: []
```
List of enabled plugins, you should include 'name' field. If the name of the resulting plugin does not match the name of the munin plugin from which it is generated (as is the case, say, with the if_ plugin), you need to add a 'plugin' field to the list item, like so:
```
munin_node_enabled_plugins:
  - name: uptime
  - name: if_eth0
    plugin: if_
```
If you need to add plugin configuration for plugins you've added via munin_node_plugins, you can do so with a simple hashmap that has the plugin name (which will be the [plugin] section in the resulting configuration file), and a list of variable names and values. For example:
```
munin_node_config: {
  "ps_test": {
    "env.regex": "bash",
    "env.name": "bash"
  }
}
```

Example Playbook
----------------
```
- hosts: localhost
  become: yes
  roles:
    - { role: tenequm.munin-node }
```
License
-------
MIT

Author Information
------------------
This role was created in 2017 by [Mykhaylo Kolesnik](http://github.com/tenequm).
