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
        name: ans_x.ans_x_base.ans_x_prometheus
      vars:
        ans_x_prometheus_user_name: 'prometheus'
        ans_x_prometheus_version: 2.48.0-rc.0
        # See defaults/main.yml for all variables        
```
```bash
ansible-playbook ans_x_playbook_prometheus.yml -i inventory.yml --vault-id passphrase.sh
```
