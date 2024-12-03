# Molecule Image

## About

We have created this image so that they are the counterpart of the
cloud image (Hetzner, ...)


## Our Image

They are all systemd container.

* Raw image does not work because they don't have python.
* Without the counterpart python module, ansible tasks will not really work.

### Kind (Kubernetes in Docker)

We have created ours. See [](kind-debian/bookworm/Dockerfile)

### SystemdD self

Create from all SystemD image in the wild. See [Systemd Dockerfile](molecule-debian/12.8/Dockerfile)

## Image in the wild


### Molecule SystemD Container Doc


Ref:
* https://developers.redhat.com/blog/2014/05/05/running-systemd-within-docker-container
* Systemd Container: https://ansible.readthedocs.io/projects/molecule/guides/systemd-container/

```bash
docker run --privileged -ti --rm -v /sys/fs/cgroup/warewulf.scope:/sys/fs/cgroup:rw ghcr.io/gerardnico/molecule-debian:12.8 /lib/systemd/systemd
```

### The Dockerfile of the docker driver

https://ansible.readthedocs.io/projects/molecule/guides/custom-image/

[The Dockerfile of the docker driver](https://github.com/ansible-community/molecule-plugins/blob/main/src/molecule_plugins/docker/playbooks/Dockerfile.j2)

For apt-get:
```bash
export DEBIAN_FRONTEND=noninteractive && \
  apt-get update && \
  apt-get install -y python3 sudo bash ca-certificates iproute2 python3-apt aptitude rsync && \
  apt-get clean \
  && rm -rf /var/lib/apt/lists/*;
```

### Molecule Dockerfile image

* https://github.com/nmusatti/molecule-docker-images
* https://hub.docker.com/r/ysebastia/molecule/tags
