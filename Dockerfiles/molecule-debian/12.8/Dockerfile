# Kind (Kube in docker)
# https://kind.sigs.k8s.io/docs/user/quick-start/#building-images
# https://github.com/kubernetes-sigs/kind/blob/74acdf744a3566279ddf7e4d7d2e2b364b1ec5bd/images/base/Dockerfile#L4
FROM debian:12.8

# Copy of https://github.com/ansible-community/molecule-plugins/blob/main/src/molecule_plugins/docker/playbooks/Dockerfile.j2
RUN export DEBIAN_FRONTEND=noninteractive && \
    apt update -y && \
    apt install -y python3 sudo bash ca-certificates iproute2 python3-apt aptitude rsync


# SystemD
# https://github.com/geerlingguy/docker-debian11-ansible/blob/master/Dockerfile
# https://github.com/Rosa-Luxemburgstiftung-Berlin/ansible-role-unbound/blob/main/molecule/default/Dockerfile-debian12.j2
# https://developers.redhat.com/blog/2014/05/05/running-systemd-within-docker-container

RUN apt -y install systemd; \
(cd /lib/systemd/system/sysinit.target.wants/; for i in *; do [ $i == systemd-tmpfiles-setup.service ] || rm -f $i; done); \
rm -f /lib/systemd/system/multi-user.target.wants/*; \
rm -f /etc/systemd/system/*.wants/*; \
rm -f /lib/systemd/system/local-fs.target.wants/*; \
rm -f /lib/systemd/system/sockets.target.wants/*udev*; \
rm -f /lib/systemd/system/sockets.target.wants/*initctl*; \
rm -f /lib/systemd/system/basic.target.wants/*; \
rm -f /lib/systemd/system/anaconda.target.wants/*;


RUN apt -y install curl # needed to download k3s
RUN apt -y install procps # for sysctl, required by K3s


RUN apt -y install python3-yaml # needed for Helm Diff Plugin, we should check there

# https://docs.k3s.io/advanced#running-k3s-in-docker
# Waiting for control-plane node kube.example.com startup: nodes \"kube.example.com\" not found
RUN apt-get install -y kmod iptables


# The symlink /usr/sbin/init does not exist
CMD ["/lib/systemd/systemd"]


# Clean
#&& \
#   echo "==> Make small" && \
#   apt-get clean && \
#	rm -rf /var/lib/apt/lists/*

ENV SHELL=/bin/bash

