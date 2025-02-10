# Prometheus

## About

This role will install:
* [prometheus](tasks/prometheus_install.yml)
* and [Node exporter](tasks/prometheus_node_exporter.yml)
on all nodes (ie single node context)

## Run Steps

* [installation](../../README.md#installation)
* `Playbook`
```yaml
- name: Prometheus
  hosts: all
  tasks:

    - name: Executing Prometheus Role
      ansible.builtin.include_role:
        name: ans_e.ans_e_base.ans_e_prometheus
      vars:
        ans_e_prometheus_user_name: 'prometheus'
        ans_e_prometheus_version: 2.48.0-rc.0
        # See defaults/main.yml for all variables        
```
```bash
ansible-playbook ans_e_playbook_prometheus.yml -i inventory.yml --vault-id passphrase.sh
```
