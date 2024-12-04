## Kube X Site

## About

Provision a cluster 

## Steps

### Conf

#### Version

The `k3s_version` is the [version of k3](https://github.com/k3s-io/k3s/releases)

Example:
* The K3s release `v1.18.6+k3s1` maps to the `v1.18.6` Kubernetes release.
* The K3s release `v1.18.6+k3s2` maps to the `v1.18.6` Kubernetes release but with a fix

[Ref](https://github.com/k3s-io/k3s?tab=readme-ov-file#release-cadence)


#### Token

The `token` is used to encrypt the data (ie you change the token, the service error will be `bootstrap data already found and encrypted with different token`)

The token should be a random string of reasonable length.

Example: Generation
```bash
openssl rand -base64 64 | tr -d '\n'
```

### Run the playbook

With the [kube_x_inventory.yml](../extensions/kube_x/kube_x_inventory.yml)
```bash
ansible-playbook playbooks/kube_x_site.yml -i ../extensions/kube_x/kube_x_inventory.yml
```

### After
#### Kubeconfig

The kubeconfig is then available at:
* on local: `~/.kube/config.new` (by default `kubeconfig` var)
* on server: `/etc/rancher/k3s/k3s.yaml`

Reference on the creation of `~/.kube/config.new`:
* [README kubeconfig](https://github.com/k3s-io/k3s-ansible?tab=readme-ov-file#kubeconfig)
* [Play kubeconfig Var Ref](https://github.com/k3s-io/k3s-ansible/blob/master/roles/k3s_server/defaults/main.yml#L5C13-L5C31)
* [Play Copy Tasks Ref](https://github.com/k3s-io/k3s-ansible/blob/master/roles/k3s_server/tasks/main.yml#L145)

#### Token

Token (generated or not) on the server is located at: `/var/lib/rancher/k3s/server/token`



## Installation Check

### Connection and Nodes

After a `molecule converge`, this should work on the docker host:
```bash
export KUBECONFIG=~/.kube/config.new
kubectl config use-context k3s-ansible
# if on localhost
kubectl config set-cluster k3s-ansible --server=https://localhost:6443
kubectl cluster-info # we should connect to the API
kubectl get nodes # we should see a node
```
```
NAME               STATUS   ROLES                  AGE     VERSION
kube.example.com   Ready    control-plane,master   8m24s   v1.31.2+k3s1
```



### Config Check

```bash
k3s check-config
```

### Journal Ctl Check

```bash
journalctl -u k3s
# real time
journalctl -u k3s -f
# Last 100 lines
journalctl -u k3s -n 100

```
```
Dec 02 20:59:32 kube.example.com k3s[2593]: time="2024-12-02T20:59:32Z" level=info msg="Kube API server is now running"
Dec 02 20:59:32 kube.example.com k3s[2593]: time="2024-12-02T20:59:32Z" level=info msg="ETCD server is now running"
Dec 02 20:59:32 kube.example.com k3s[2593]: time="2024-12-02T20:59:32Z" level=info msg="k3s is up and running"
```


## Dependency
* https://github.com/k3s-io/k3s-ansible