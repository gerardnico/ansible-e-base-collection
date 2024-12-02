# Install Helm

## About

This role install `Helm` on target.

This is needed if you use Ansible to deploy `Helm`.

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