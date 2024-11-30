
FROM debian:12.8

# Copy of https://github.com/ansible-community/molecule-plugins/blob/main/src/molecule_plugins/docker/playbooks/Dockerfile.j2
RUN export DEBIAN_FRONTEND=noninteractive && \
    apt update -y && \
    apt install -y python3 sudo bash ca-certificates iproute2 python3-apt aptitude rsync


#&& \
#   echo "==> Make small" && \
#   apt-get clean && \
#	rm -rf /var/lib/apt/lists/*

ENV SHELL=/bin/bash

