FROM debian:12.8

RUN \
    apt update -y && \
    apt install -y apt-utils gcc git python3 python3-dev python3-pip python3-apt libssl-dev sudo && \
    ln -s /usr/bin/python3 /usr/bin/python

RUN \
   echo "Ip command to get the address" && \
   apt install -y iproute2


#&& \
#   echo "==> Make small" && \
#	rm -rf /var/lib/apt/lists/*

ENV SHELL=/bin/bash

