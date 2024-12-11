# Kind (Kube in docker)
# https://kind.sigs.k8s.io/docs/user/quick-start/#building-images
# https://github.com/kubernetes-sigs/kind/blob/74acdf744a3566279ddf7e4d7d2e2b364b1ec5bd/images/base/Dockerfile#L4
#
# The `base` image is a small-ish Docker image for running nested containers, systemd, and kubernetes components.
# https://kind.sigs.k8s.io/docs/design/base-image/
FROM kindest/base:v20241108-5c6d2daf


# Ansible
RUN export DEBIAN_FRONTEND=noninteractive && \
    apt update -y && \
    echo "# Python" && \
    apt install -y python3 && \
    echo "# apt module" && \
    apt install -y python3-apt && \
    echo "# needed for Helm Diff Plugin, we should check there" && \
    apt -y install python3-yaml


# (nslookup, netstat )
RUN echo "# nslookup" \
    && apt-get install -y dnsutils \
    && echo "# ip tools" \
    && apt-get install -y iputils-ping \
    && echo "# netstat" \
    && apt-get install -y net-tools
