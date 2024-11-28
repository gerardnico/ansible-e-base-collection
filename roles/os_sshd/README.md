# Hardened SSHD

## About 
Hardened SSHD as specified by [Hetzner](https://community.hetzner.com/tutorials/securing-ssh)

## Features

* No [root login](https://man.openbsd.org/sshd_config#PermitRootLogin)
* No [password authentication](https://man.openbsd.org/sshd_config#PasswordAuthentication)
* No Kbd Interactive Authentication
* No Challenge Response Authentication

## Example

```yml
sshd_no_root_login: true
sshd_allowed_users:
  - admin
  - foo@home.example.com
sshd_deny_users:
  - fedora
```

## Conf

* `sshd_permit_root_login` : permit or not root login ([PermitRootLogin](https://man.openbsd.org/sshd_config#PermitRootLogin) - Default to true)
* `sshd_allowed_users` : A list of users to allows ([AllowUsers](https://man.openbsd.org/sshd_config#AllowUsers) - mandatory if `sshd_no_root_login` is true)
* `sshd_deny_users`: A list of users to deny ([DenyUsers](https://man.openbsd.org/sshd_config#DenyUsers)) in case a non-root user is provided by the VPS provider


## Note: Firewall SSHD by Country

[How to allow only SSH on a country](https://datacadamia.com/firewalld_country)

