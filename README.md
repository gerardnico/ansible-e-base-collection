# AnsE - Ansible Easy Collections

## About

This the Ansible Collections of the `Ansible Easy Framework`

## Content

### Roles

See [Roles](roles)

### Playbook

See [Playbooks](playbooks)

## Installation

* Installation at the command line:
```bash
# Head
ansible-galaxy collection install git+https://github.com/gerardnico/ansible-e-base-collection.git
# Tag/Branch/Commit
ansible-galaxy collection install git+https://github.com/gerardnico/ansible-e-base-collection.git,tagname
```
* Installation with a [requirement file](https://docs.ansible.com/ansible/latest/collections_guide/collections_installing.html#install-multiple-collections-with-a-requirements-file)
```yaml
collections:
  - name: https://github.com/gerardnico/ansible-e-base-collection.git
    type: git
    version: 2a5a89a  # specify branch/tag/commit
```
```bash
ansible-galaxy install -r requirements.yml
```

