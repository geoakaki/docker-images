# CI job image for Ansible runs: ansible (community package), AWS SDK/CLI for dynamic inventories and
# AWS modules, netaddr for the ipaddr filters, and ssh/rsync for the connection and synchronize module.
FROM python:3.13-slim

ARG ANSIBLE_VERSION=14.5.0

RUN apt-get update \
 && apt-get install -y --no-install-recommends git openssh-client rsync sshpass \
 && rm -rf /var/lib/apt/lists/*

RUN pip install --no-cache-dir "ansible==${ANSIBLE_VERSION}" boto3 awscli netaddr

RUN ansible --version && aws --version

CMD ["bash"]
