# Molecule Image

## About

We have created this image so that they are the counterpart of the
cloud image (Hetzner, ...)

Raw image does not work because they don't have python.
Without the counterpart python module, ansible tasks will not really work.

## Molecule Dockerfile image in the wild

* https://github.com/nmusatti/molecule-docker-images
* https://hub.docker.com/r/ysebastia/molecule/tags

### SystemD Container

```bash
docker run \
  -v /sys/fs/cgroup:/sys/fs/cgroup:ro \
  -it \
  --rm \
  --privileged \
  nmusatti/ubuntu2204-pys-systemd \ 
  /bin/bash
```
* https://developers.redhat.com/blog/2014/05/05/running-systemd-within-docker-container
* Systemd Container: https://ansible.readthedocs.io/projects/molecule/guides/systemd-container/
* https://github.com/moby/moby/issues/42275

```bash
docker run --privileged -ti --rm -v /sys/fs/cgroup/warewulf.scope:/sys/fs/cgroup:rw ghcr.io/gerardnico/molecule-debian:12.8 /lib/systemd/systemd
docker run --rm -it -v /sys/fs/cgroup/warewulf.scope:/sys/fs/cgroup:rw --tmpfs /run --tmpfs /run/lock warewulf-1:latest /sbin/init
```
### Docker Driver

[The Dockerfile of the docker driver](https://github.com/ansible-community/molecule-plugins/blob/main/src/molecule_plugins/docker/playbooks/Dockerfile.j2)

For apt-get:
```bash
export DEBIAN_FRONTEND=noninteractive && \
  apt-get update && \
  apt-get install -y python3 sudo bash ca-certificates iproute2 python3-apt aptitude rsync && \
  apt-get clean \
  && rm -rf /var/lib/apt/lists/*;
```

## FAQ

### What is the default molecule command

The default command is `sleep 1d`
```bash
docker run -rm python:3.12.7-bookworm sleep 1d 
```



