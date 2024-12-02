# Install Helm

## About

This role install:
* `Helm` 
* and the plugin [helm-diff](https://github.com/databus23/helm-diff)

This is needed if you use Ansible to deploy `Helm` as the command is run on the target.

## Example

* The `ans_x_helm_version`: https://github.com/helm/helm/releases . Example: `3.15.2`
* Facts should be gathered in the playbook
```yaml
- name: Play
  hosts: all
  roles:
    - role: ans_x_helm
      vars:
        ans_x_helm_version: 3.15.2
```

## Prerequisites

The `yaml` python module for the [Helm Diff Plugin](https://github.com/databus23/helm-diff)

```bash
apt install -y python3-apt
```
