# OS 

## About

This role execute common init/bootstrap tasks:
* the hostname and ip
* add an optional admin user (in the wheel group)
* set additional packages
* disable selinux



## Example / Usage

Example of inventory file:
```yaml
all:
  children:
    group:
      hosts:
        my_ansible_server_name:
          # Hostname in a FQDN form (ie name.apex.tld)
          ans_x_os_hostname: kube-server-01.example.com
          # Username of the admin user (sudo wheel user)
          ans_x_os_admin_username: admin 
          # the public key of the admin user (mandatory with an admin user)
          ans_x_os_admin_public_key: ssh-rsa AAAAB3NzaC1yxxxx
          # List of packages for the os package manager
          ans_x_os_packages:
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
```

## Package Manager supported

* `yum` and `apt`

## Support
### No sudo

Note that without sudo, you can still execute administrative task with `pkexec` if you are in the wheel group.

Example for a bad:
```bash
pkexec rm /etc/sudoers.d/tower
```
```
pkexec visudo -f /etc/sudoers.d/appctl
```

