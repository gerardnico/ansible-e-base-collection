# Traefik

## About
This is an example of a traefik role that:
* customize the traefik included helm release in a `k3s` installation with [Traefik k3s](tasks/traefik_k3s.yml)
* configure Traefik with [Manifest](tasks/traefik_conf.yml)


## Dependencies

* Cert Manager
* Prometheus Operator

## Example with a deployment of the dashboard

The inventory should follow the [k3s ansible one](https://github.com/k3s-io/k3s-ansible#usage)


```yaml
- name: Setup K3S server
  hosts: servers
  become: true
  roles:
    - role: ans_e_k3s_traefik
      tags: traefik
      vars:
        ans_e_k3s_traefik_dashboard_hostname: 'traefik.i.example.com'
        ans_e_k3s_traefik_dashboard_basic_auth_users:
          # A list with one user by item 
          # where the value is the output of the command `htpasswd`.
          # -------------
          # Example:
          # ```bash
          # htpasswd -nb admin@traefik welcome
          # ```
          # -------------
          # This value should be encrypted with Ansible vault.
          # Example:
          # ```bash
          # # There is a dollar don't forget the single quote
          # ansible-vault encrypt_string 'admin@traefik:$apr1$DhxcDhWu$Obq7vz1P.HJYFudhxSFqA.'
          # ```
          # -------------
          # The below line represents the user `admin@traefik` with the password `welcome`
          - admin@traefik:$apr1$9qTj0zBl$ZmA009UNx3RMOBxALIKLB1
        # https://github.com/traefik/traefik-helm-chart/blob/master/traefik/values.yaml
        ans_e_k3s_traefik_helm_values:
          service:
            spec: {
              externalTrafficPolicy: Cluster # Allow Origin Remote IP to be forwarded
            }
          additionalArguments:
            - '--accesslog=true'
            - '--log.level=DEBUG'
          ports:
            websecure:
              # https://doc.traefik.io/traefik/v2.3/routing/entrypoints/#forwarded-headers
              forwardedHeaders:
                # https://www.cloudflare.com/en-gb/ips/
                trustedIPs:
                  - 103.21.244.0/22
                  - 103.22.200.0/22
                  - 103.31.4.0/22
                  - 104.16.0.0/13
                  - 104.24.0.0/14
                  - 108.162.192.0/18
                  - 131.0.72.0/22
                  - 141.101.64.0/18
                  - 162.158.0.0/15
                  - 172.64.0.0/13
                  - 173.245.48.0/20
                  - 188.114.96.0/20
                  - 190.93.240.0/20
                  - 197.234.240.0/22
                  - 198.41.128.0/17
```

## Features

### Dashboard Access

The dashboard will be accessible:
  * via the hostname (variable `ans_e_k3s_traefik_dashboard_hostname`)
  * secured by `basic auth` 
  * by the basic auth users (Variable `ans_e_k3s_traefik_dashboard_basic_auth_users`
    * The default User is: 
      * username: `admin@traefik` 
      * password: `welcome`

### Values Customization

Helm Values customization can be set via the variable `ans_e_k3s_traefik_helm_values`

The possible values are defined [here](https://github.com/traefik/traefik-helm-chart/blob/master/traefik/values.yaml)


## Debug

### Get Actual Cli/Static Configuration


```bash
kubectl exec svc/traefik -- ps aux | grep traefik | grep -o -- '--[^ ]*'
```


### Logs

```bash
kubectl logs -l app.kubernetes.io/name=traefik -n kube-system -f
```

