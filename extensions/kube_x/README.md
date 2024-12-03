# Molecule Testing for Kube-X


## Results: Scenario

* The [docker scenario](molecule/default/README.md) run successfully
* The vagrant does not work because we work with docker, and it does not found any vagrant.


## Run

* Go to the project dir
```bash
cd extensions/kube_x
```
* Create the docker kube network if not present
```bash
docker network create kube
```
* Optionally, change the docker image used in the env in the [.env](.env.yml)
* Run (Always reset, otherwise, it's too much pain because of the cache)
```bash
# -e .env.yml should be taken into account because it's in the project directory
molecule reset && molecule converge
```
* Destroy
```bash
molecule destroy -s docker
```

## Test/Verify

After a `molecule converge`, this should work:
```bash
KUBECONFIG=~/.kube/config.new kubectl config use-context k3s-ansible
KUBECONFIG=~/.kube/config.new kubectl config set-cluster k3s-ansible --server=https://localhost:6443
KUBECONFIG=~/.kube/config.new kubectl cluster-info
KUBECONFIG=~/.kube/config.new kubectl get nodes # we should see a node
```
```
NAME               STATUS   ROLES                  AGE     VERSION
kube.example.com   Ready    control-plane,master   8m24s   v1.31.2+k3s1
```

## Molecule Project directory

This is the molecule project directory for the `kube_x` playbooks.
* [](../../playbooks/kube_x_site.yml)

Normally this is the `role` but in our case. There is no role.

> [WARNING] 
> Molecule can not be run from the collection project directory.
> Otherwise, it will reinstall the collection and messed up practically everything.

