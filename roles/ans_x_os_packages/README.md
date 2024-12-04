# Os Package installation

## About
A role to install OS packages.

## Vars


## Package Manager supported

* `yum` and `apt`


## Example / Usage

Example of inventory file:
```yaml
all:
  children:
    group:
      vars:
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