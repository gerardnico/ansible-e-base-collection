# Molecule Image

## About

We have created this image so that they are the counterpart of the
cloud image (Hetzner, ...)

Raw image does not work because they don't have python.
Without the counterpart python module, ansible tasks will not really work.

## Other molecule image in the wild

* https://github.com/nmusatti/molecule-docker-images
* https://hub.docker.com/r/ysebastia/molecule/tags

## FAQ

### What is the default molecule command

The default command is `sleep 1d`
```bash
docker run -rm python:3.12.7-bookworm sleep 1d 
```

## Ref

* Systemd Container: https://ansible.readthedocs.io/projects/molecule/guides/systemd-container/
