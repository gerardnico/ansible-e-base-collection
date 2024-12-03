# A Molecule with Docker Driver

## Steps
The `create` and `destroy` playbooks are provided by the docker driver.

They are [here](https://github.com/ansible-community/molecule-plugins/tree/main/src/molecule_plugins/docker/playbooks)


## Docker Bug

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

This is because `/var/lib/rancher` has to be a local filesystem such as ext4 or XFS. https://github.com/containerd/containerd/issues/5464
(ie can only write data to a Linux filesystem)
But is not
```bash
df -T /var/lib/rancher
```
```
Filesystem     Type     1K-blocks      Used Available Use% Mounted on
overlay        overlay 1055762868 119930392 882129004  12% /
```
Solution Mount it:
```bash
docker run -v /tmp/var/lib/rancher:/var/lib/rancher
```
