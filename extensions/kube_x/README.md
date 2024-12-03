# Molecule Testing for Kube-X

## Docker Bug, may be Vagrant ?

The installation runs without problem. We can connect to the API 
but there is no node.

We got network problem.
```bash
journalctl -u k3s -f
```
```
"Waiting to retrieve agent configuration; server is not ready: \"overlayfs\" snapshotter cannot be enabled for \"/var/lib/rancher/k3s/agent/containerd\",
try using \"fuse-overlayfs\" or \"native\": failed to mount overlay: invalid argument"
Error: # Waiting for control-plane node kube.example.com startup: nodes \"kube.example.com\" not found
```

`k3s-ansible` uses full fledge virtual image with [Vagrant](https://github.com/k3s-io/k3s-ansible/blob/master/Vagrantfile)
and `vagrant` is a [driver for molecule](https://github.com/ansible-community/molecule-plugins/tree/main/src/molecule_plugins/vagrant)

## Molecule Project directory

This is the molecule project directory for the `kube_x` playbooks.
* [](../../playbooks/kube_x_site.yml)

Normally this is the `role` but in our case. There is no role.

> [WARNING] 
> Molecule can not be run from the collection project directory.
> Otherwise, it will reinstall the collection and messed up practically everything.

