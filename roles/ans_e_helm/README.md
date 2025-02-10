# Install Helm

## About

This role install:
* `Helm` 
* and the plugin [helm-diff](https://github.com/databus23/helm-diff)

This is needed if you use Ansible to deploy `Helm` as the command is run on the target.

## vars

* `ans_e_helm_version`: the [helm release version](https://github.com/helm/helm/releases)
* `ans_e_helm_plugins`: a list of plugin with properties of [helm_plugin_module](https://docs.ansible.com/ansible/latest/collections/kubernetes/core/helm_plugin_module.html) 
  * Only `plugin_path`, `plugin_version` and `state`




## Example

```yaml
- name: Play
  hosts: all
  roles:
    - role: ans_e_helm
      vars:
        ans_e_helm_version: 3.16.3
        ans_e_helm_plugins:
          - plugin_path: https://github.com/databus23/helm-diff
            plugin_version: 3.9.12
            state: present
```

## Helm-diff plugin

To avoid the below warning, you should install [helm-diff](https://github.com/databus23/helm-diff)
```
# [WARNING]: The default idempotency check can fail to report changes in certain
# cases. Install helm diff >= 3.4.1 for better results.
```

The `yaml` python module is mandatory.

With `ans_e_os_packages`
```bash
ans_e_os_packages:
  - python3-apt
```
