# Molecule Image

## About

We have created this image so that they are the counterpart of the
cloud image (Hetzner, ...)

Raw image does not work because they don't have python.
Without the counterpart python module, ansible tasks will not really work.

## Molecule Dockerfile image in the wild

* https://github.com/nmusatti/molecule-docker-images
* https://hub.docker.com/r/ysebastia/molecule/tags

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

## Ref

* Systemd Container: https://ansible.readthedocs.io/projects/molecule/guides/systemd-container/

