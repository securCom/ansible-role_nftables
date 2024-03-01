# nftables Management

This role helps you manage packet filtering and related tasks with nftables.

As it's very hard to write a generic nftables template, this role just moves **user defined** nftables config snippets to the target device and runs them.  You still need to understand nftables syntax.

## Requirements

- ansible: 2.4

## Role Variables

### OS Based Variables

Some variables are based on OS. These variables are located in `vars/os-<OS>.yml` files.

### Generic Variables

- `nftables_dir`: nftables configuration directory, defaults to **/etc/nftables**
- `nftables_service_state`: if the rule set should be started
- `nftables_service_enabled`: if the rule set should be enabled at boot time

### Firewall Rules

- `nftables_rules_directory`: where to look for the firewall rules files, defaults to playbook templates directory
- `nftables_families`: which IP version(s) to generated rules for, defaults **IPv4** and **IPv6**
- `nftables_rules`: list of rules to apply. Defaults to allow only SSH and ICMP, see sample rules in **templates/rules** directory

This role uses the templating engine to generate rules. The hard work to write the rules is still your responsibility, but you have it fully **under control**.

## Example

### host/group variables
```
nftables_rules_directory: {{ playbook_dir }}/files/nftables

nftables_rules:
  - default_rules
  - connection_tracking
  - input_icmp
  - managment
```
In this case you should create following files:

- `{{ playbook_dir }}/files/nftables/rules/default_rules.conf.j2`
- `{{ playbook_dir }}/files/nftables/rules/connection_tracking.conf.j2`
- `...`

You should rewrite the `nftables_rules` in **group_var** or **host_vars** for each group or server as needed.


### playbook

```
- hosts: firewall1
  roles:
     - securcom.nftables
```

## Dependencies

None

## License

BSD

## Author Information

Peter Hudec (@hudecof)
