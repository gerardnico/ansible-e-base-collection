# Molecule Testing for Kube-X


## Run

### Docker Scenario

```bash
# go to the project dir
cd extensions/kube_x
molecule converge -s docker
molecule destroy -s docker
```

## Results: Scenario

* The [docker scenario](molecule/docker/README.md) run successfully but there is a [network problem](molecule/docker/README.md#docker-bug) and there is no nodes in the cluster
* The vagrant does not work because we work with docker, and it does not found any vagrant.


## Molecule Project directory

This is the molecule project directory for the `kube_x` playbooks.
* [](../../playbooks/kube_x_site.yml)

Normally this is the `role` but in our case. There is no role.

> [WARNING] 
> Molecule can not be run from the collection project directory.
> Otherwise, it will reinstall the collection and messed up practically everything.

