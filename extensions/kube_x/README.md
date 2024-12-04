# Molecule Testing for Kube-X


## About

This is the project directory of a molecule [docker scenario](molecule/default/README.md).

This is the molecule project directory for the `kube_x` playbooks.
* [](../../playbooks/kube_x_site.yml)

Note: Normally the project directory is the `role` but in our playbook case. There is no role.

## Run

* Go to this project directory
```bash
cd extensions/kube_x
```

> [WARNING]
> Molecule can not be run from the collection project directory.
> Otherwise, it will reinstall the collection and messed up practically everything.

* Create the docker kube network if not present (don't know why, but then we get a `nslookup hostname` working)
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

Check [](../../playbooks/kube_x_site.md#installation-check)






