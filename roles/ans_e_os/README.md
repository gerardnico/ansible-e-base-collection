# OS 

## About

This role execute common init/bootstrap tasks:
* the full qualified hostname (hostname and ip name in /etc/hosts)
* add an optional admin user (in the wheel group)
* add optional packages
* disable selinux by default

## Vars

* `ans_e_os_hostname`: the full qualified os name
* `ans_e_os_admin_username`: `Optional` an admin user (sudo wheel user)
* `ans_e_os_admin_public_key`: `Optional` the admin user public key
* `ans_e_os_packages`: `Optional` a list of packages to install
* `ans_e_os_packages_epel_repo_enabled`: `Optional` a boolean to add or no [Extra Packages for Enterprise Linux (epel)](https://docs.fedoraproject.org/en-US/epel/) (yum base only)
* `ans_e_selinux_disabled`: `Optional` default to true

## Example / Usage

Example of inventory file:
```yaml
all:
  children:
    group:
      vars:
        # Username of the admin user (sudo wheel user)
        ans_e_os_admin_username: admin
        # the public key of the admin user (mandatory with an admin user)
        ans_e_os_admin_public_key: ssh-rsa AAAAB3NzaC1yxxxx
        # Do not disable selinux (default true)
        ans_e_selinux_disabled: false
        # Packages
        ans_e_os_packages:
          - curl # needed to download k3s
          - git
          - jq
          - wget
          - lsof # lsof (open file)
          - zip
          - unzip
          - sshpass
          - nmap # Added nmap to tbe able to scan the open port at the command line
          - sysstat # (for process monitoring and stat, pidstat)
          - bzip2 # to unzip bz2)
          - telnet
          - sudo
          - whois
          - netcat-openbsd
          - bind9-utils # dig
        # Install epel
        ans_e_os_packages_epel_repo_enabled: true
      hosts:
        my_ansible_server_name:
          # Hostname in a FQDN form (ie name.apex.tld)
          ans_e_os_hostname: kube-server-01.example.com
```


## Support
### No sudo

Note that without sudo, you can still execute administrative task with `pkexec` if you are in the wheel group.

Example for a bad:
```bash
pkexec rm /etc/sudoers.d/tower
```
```bash
pkexec visudo -f /etc/sudoers.d/appctl
```

