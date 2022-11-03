# nftables managment

This role manages the nftables.

As it's very hard to write generic nftables template, this role just moves **user defined** nftables config snippets to the server and run them, Ypu still need to understand nftables syntax.

## Requirements

- ansible: 2.4

## Role Variables

### OS based variables

Some variables are based on OS. These variables are locaten in `vars/os-<OS>.yml` files.

### Generic Variables

- `nftables_dir`: nftables configuration directory, defaults to **/etc/nftables**
- `nftables_service_state`: if the ferm should be started
- `nftables_service_enabled`: if the ferm should be enabled in boot sequence

### Firewal rules

- `nftables_rules_directory`: where should I look for the firewall rules files, default to playbook templates directory
- `nftables_families`: to which ip version generate the rules, defaults **IPv4** and **IPv6**
- `nftables_rules`: list of rules to apply. defualt allow only SSH and ICMP, see sample rules in **templates/rules** directory


This role is using  of the templating engine to generate rules. The hard work to write the rules is still on you, but you have it fully **under control**.

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
In this case you should create following files

- `{{ playbook_dir }}/files/nftables/rules/default_rules.conf.j2`
- `{{ playbook_dir }}/files/nftables/rules/connection_tracking.conf.j2`
- `...`

You should rewrite the `nftables_rules` in **group_var** or **host_vars** for each group or server as needed.


### playbook

```
- hosts: ferm
  roles:
     - securcom.nftables
```

## Dependencies

None

## License

BSD

## Author Information

Peter Hudec (@hudecof)
