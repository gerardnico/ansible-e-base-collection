# Longhorn


## About

Longhorn is an OS dependent application and therefore has an Ansible Role to [configure the OS](#configure-os)


This role can also:
* [install Longhorn via Helm](#install-longhorn-via-a-chart) 
* and configure:
  * an [ingress](#ingress) 
  * a [backup](#cronjob-backup)

## What is Longhorn ?

Longhorn creates a dedicated storage controller for each volume and synchronously replicates 
the volume across multiple replicas stored on multiple nodes.

## Prerequisites

* Helm should be on the target host. You can use the [ans_e_helm_role](../ans_e_helm) for that.

## Example / Usage

### Configure OS

The OS configuration runs each time. It:
* installs the dependent packages
* disables the rpc bind service for security reason
* and run the environments check script

```yaml
- name: Play
  hosts: all
  roles:
    - role: ans_e_longhorn
```

### Install Longhorn via a Chart

If you set the `ans_e_longhorn_version` variable value, this playbook will install longhorn via the Helm Chart

You can even pass the chart values via the `ans_e_longhorn_chart_values` variable.

The namespace is `longhorn-system`

Example:
```yaml
- name: Setup Longhorn
  hosts: all
  gather_facts: true
  become: true
  roles:
    - role: ans_e_longhorn
      vars:
        # The version is m
        ans_e_longhorn_version: "1.7.0"
        # The chart values
        # [Doc](https://longhorn.io/docs/1.6.2/advanced-resources/deploy/customizing-default-settings/#using-helm)
        # [Default values file](https://raw.githubusercontent.com/longhorn/charts/master/charts/longhorn/values.yaml)
        ans_e_longhorn_chart_values:
          defaultSettings:
            defaultReplicaCount: 1
            defaultDataLocality: strict-local
            # https://longhorn.io/docs/1.6.2/snapshots-and-backups/backup-and-restore/set-backup-target/
            # backupTarget: s3://longhorn-backup@us/
            # backupTargetCredentialSecret: '{{ ans_e_longhorn_s3_secret_name }}'
            #        createDefaultDiskLabeledNodes: true
            #        defaultDataPath: /var/lib/longhorn-example/
            #        replicaSoftAntiAffinity: false
            #        storageOverProvisioningPercentage: 600
            #        storageMinimalAvailablePercentage: 15
            #        upgradeChecker: false
            #        defaultLonghornStaticStorageClass: longhorn-static-example
            #        backupstorePollInterval: 500
            #        taintToleration: key1=value1:NoSchedule; key2:NoExecute
            #        systemManagedComponentsNodeSelector: "label-key1:label-value1"
            #        priorityClass: high-priority
            #        autoSalvage: false
            #        disableSchedulingOnCordonedNode: false
            #        replicaZoneSoftAntiAffinity: false
            #        replicaDiskSoftAntiAffinity: false
            #        volumeAttachmentRecoveryPolicy: never
            #        nodeDownPodDeletionPolicy: do-nothing
            #        guaranteedInstanceManagerCpu: 15
            #        orphanAutoDeletion: false
```

### Longhorn Configuration

This configuration is really specific and is much more for demonstration purpose.



### Ingress
The ingress depends on:
* Traefik
* Cert Manager

The variables are:
* `ans_e_longhorn_hostname`: the host name in your DNS
* `ans_e_cert_manager_issuer_name`: the cert manager issuer (generally `letsencrypt-prod` or `letsencrypt-staging`)
* `ans_e_longhorn_frontend_basic_auth_users`: the users in a list. See [how to add an ingress user](#how-to-add-an-ingress-user) for the values


```yaml
- name: Setup Longhorn
  hosts: all
  gather_facts: true
  become: true
  roles:
    - role: ans_e_longhorn
      vars:
        # Version and Chart values omitted
        # ...
        # Hostname
        ans_e_longhorn_hostname: "longhorn.i.eraldy.com"
        # List of users
        # Below is the user `admin@traefik` with the password `welcome`
        ans_e_longhorn_frontend_basic_auth_users:
          - admin@traefik:$apr1$9qTj0zBl$ZmA009UNx3RMOBxALIKLB1
        # The cert manager issuer
        ans_e_cert_manager_issuer_name: 'letsencrypt-prod'
        # Cron Job
        ans_e_recurring_job_cron: "0 1 * * *"
```

#### How to add an Ingress user

To add a user called `admin@traefik` with the password `welcome`
```bash
htpasswd -nb admin@traefik welcome
# ie `htpasswd -nb username password`
# example output: `admin@traefik:$apr1$9qTj0zBl$ZmA009UNx3RMOBxALIKLB1`
```
```yaml
ans_e_longhorn_frontend_basic_auth_users:
  - admin@traefik:$apr1$9qTj0zBl$ZmA009UNx3RMOBxALIKLB1
```
The password is bcrypt hashed, it should be safe to store it in the repo, but you can encrypt it further with `ansible vault`

### CronJob Backup

If the `ans_e_recurring_job_cron` is set, it will add a [Backup CronJob](https://longhorn.io/docs/1.6.2/snapshots-and-backups/scheduling-backups-and-snapshots/#using-the-manifest) 
with this [backup manifest](templates/longhorn-backup.yml)

The variables are all mandatory:
* `ans_e_recurring_job_cron`: the cron expression
* `ans_e_longhorn_s3_key_id`: the s3 access key
* `ans_e_longhorn_s3_key_secret`: the s3 access key secret
* `ans_e_longhorn_s3_endpoints`: the endpoints

```yaml
- name: Setup Longhorn
  hosts: all
  gather_facts: true
  become: true
  roles:
    - role: ans_e_longhorn
      vars:
        # Version and Chart values omitted
        # Ingress vars omitted
        # Cron Job expression for a daily backup
        ans_e_recurring_job_cron: "0 1 * * *"
        # s3 parameters
        ans_e_longhorn_s3_key_id:  !vault 
        ans_e_longhorn_s3_key_secret: !vault
        ans_e_longhorn_s3_endpoints: https://h0k0.ca.idrivee2-22.com
```
