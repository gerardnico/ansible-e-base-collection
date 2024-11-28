# OS (Common and Safety)

## About

This role will secure the server as describe in the [ref](#ref). 
The connection is only via a public key.

## Example / Usage

### Prerequisites

The `hostname` should be set in the [inventory file](../../../../ansible/inventory/inventory.yml) with the `FQDN` format.

Example:
```yaml
all:
  children:
    group:
      hosts:
        ansible_hostname:
          hostname: sub.example.com
```



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

