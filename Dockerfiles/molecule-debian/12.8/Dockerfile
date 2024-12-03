FROM debian:12.8
# https://docs.k3s.io/advanced#running-k3s-in-docker
# Kind (Kube in docker)
# https://kind.sigs.k8s.io/docs/user/quick-start/#building-images
# https://github.com/kubernetes-sigs/kind/blob/74acdf744a3566279ddf7e4d7d2e2b364b1ec5bd/images/base/Dockerfile#L4
# SystemD
# https://github.com/geerlingguy/docker-debian11-ansible/blob/master/Dockerfile
# https://github.com/Rosa-Luxemburgstiftung-Berlin/ansible-role-unbound/blob/main/molecule/default/Dockerfile-debian12.j2
# https://developers.redhat.com/blog/2014/05/05/running-systemd-within-docker-container
# Error:
# https://github.com/k3s-io/k3s/issues/5989
# "Waiting to retrieve agent configuration; server is not ready: \"overlayfs\" snapshotter cannot be enabled for \"/var/lib/rancher/k3s/agent/containerd\",
# try using \"fuse-overlayfs\" or \"native\": failed to mount overlay: invalid argument"
# Error: # Waiting for control-plane node kube.example.com startup: nodes \"kube.example.com\" not found

# Copy of https://github.com/ansible-community/molecule-plugins/blob/main/src/molecule_plugins/docker/playbooks/Dockerfile.j2
RUN export DEBIAN_FRONTEND=noninteractive && \
    apt update -y && \
    apt install -y \
      aptitude \
      bash \
      ca-certificates \
      conntrack \
      ethtool \
      fuse-overlayfs \
      iproute2 \
      iptables \
      kmod \
      libseccomp2 \
      mount \
      nftables \
      nfs-common \
      open-iscsi \
      pigz && \
    echo "# procps (for sysctl, required by K3s)" && \
    apt install -y procps \
      sudo \
      systemd \
      util-linux

# Tools
RUN echo "Curl needed to download k3s" && \
    apt -y install curl && \
    echo "Rsync" && \
    apt -y install rsync jq

# Ansible
RUN export DEBIAN_FRONTEND=noninteractive && \
    echo "# Python" && \
    apt install -y python3 && \
    echo "# apt module" && \
    apt install -y python3-apt && \
    echo "# needed for Helm Diff Plugin, we should check there" && \
    apt -y install python3-yaml


RUN echo "Installing Packages ..." \
    && find /lib/systemd/system/sysinit.target.wants/ -name "systemd-tmpfiles-setup.service" -delete \
    && rm -f /lib/systemd/system/multi-user.target.wants/* \
    && rm -f /etc/systemd/system/*.wants/* \
    && rm -f /lib/systemd/system/local-fs.target.wants/* \
    && rm -f /lib/systemd/system/sockets.target.wants/*udev* \
    && rm -f /lib/systemd/system/sockets.target.wants/*initctl* \
    && rm -f /lib/systemd/system/basic.target.wants/* \
    && echo "ReadKMsg=no" >> /etc/systemd/journald.conf


# (nslookup, dig )
RUN apt-get install -y dnsutils iputils-ping

# The symlink /usr/sbin/init does not exist
CMD ["/lib/systemd/systemd"]


# Clean
#&& \
#   echo "==> Make small" && \
#   apt-get clean && \
#	rm -rf /var/lib/apt/lists/*

ENV SHELL=/bin/bash

