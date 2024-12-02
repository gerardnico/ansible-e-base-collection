
## Steps

### Run the playbook
```bash
ansible-playbook playbooks/kube-x.yml -i ../extensions/kube_x/kube-x-inventory.yml
```

### Kubeconfig

The kubeconfig is then available at: `~/.kube/config.new` (by default `kubeconfig` var)

On server: `/etc/rancher/k3s/k3s.yaml`


Play reference:
* [kubeconfig Var Ref](https://github.com/k3s-io/k3s-ansible/blob/master/roles/k3s_server/defaults/main.yml#L5C13-L5C31)
* [Copy Tasks Ref](https://github.com/k3s-io/k3s-ansible/blob/master/roles/k3s_server/tasks/main.yml#L145)

Update the config
```bash
export KUBECONFIG=~/.kube/config.new
KUBE_CONTEXT=k3s-ansible
kubectl config use-context $KUBE_CONTEXT
kubectl config set-cluster $KUBE_CONTEXT --server=https://localhost:6443
kubectl cluster-info
```

### Installation Check

```bash
k3s check-config
```

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

```bash
level=info msg="Waiting for control-plane node kube.example.com startup: nodes \"kube.example.com\" not found"
```