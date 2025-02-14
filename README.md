# Homelab Configuration

A collection of Ansible playbooks for my homelab. Currently, this is a work in progress, as I've had my homelab for nearly 3 years now and everything has been done manually. I'm converting to Ansible to A) provide easier deployment in the future, and B) be able to spin everything back up in the event of total failure.

## What this repo _will_ contain

1. A collection of Ansible playbooks for spinning up Proxmox containers, configuring SSH, and other specific applications
2. Any additional configuration that may be needed for deployment

## What this repo _won't_ contain

1. Playbooks which automate any form of OS install (On VM or bare metal)
2. Generic playbooks which anyone can use. These playbooks are _highly_ customized for my use, however you may use them if you like the way I do things! Obviously nothing is stopping you since this is a public repo :)
3. GitHub Actions that automate any sort of deployment (for now). My goal is to have these playbooks automatically update my containers in the future, but tools like Flux or ArgoCD are specific to Kubernetes. GitHub Actions does have an Ansible action but I'm not exposing my containers to the internet for what I hope are extremely obvious reasons.

# Usage

Before using any of these commands, be sure to modify the `ansible.cfg` file to your needs.

## Install Promtail

You can define the loki variables directly in the command</br>
`ansible-playbook -e loki_ip_address=<loki_ip> -e loki_port=<loki_port> ./ansible/install_promtail.yml`

Or, you can define them in a YAML or JSON file and run the command this way</br>
`ansible-playbook -e @<your env file> ./ansible/install_promtail.yml`
If the variables aren't provided, they will default to `127.0.0.1` and `3100`, respectively.

## Install literally anything else
All other apps do not require any extra variables (yet). However, this will change. For now, to install other apps:</br>
`ansible-playbook ./ansible/install_<app>.yml`

# Prerequisites
This collection assumes `amd64` CPU architectures and Debian-based operating systems with the `apt` package manager installed. Do not proceed if this doesn't apply to you.

You must have an SSH key on each server that allows you to SSH without intervention (So, no passphrase protected keys). If this is a problem for you, you can either create a new temporary key or install everything manually. There are workarounds for using passphrase protected keys, but they aren't simple.

For certain apps, `apt` repos must be configured. The list of applications and necessary playbooks to run prior to installation are below.

| Application                       | Playbook                   |
|-----------------------------------|----------------------------|
| Filestat Exporter                 | N/A                        |
| Grafana                           | configure_grafana_repo.yml |
| Loki                              | configure_grafana_repo.yml |
| Node Exporter                     | N/A                        |
| Promtail (Deprecated<sup>1</sup>) | configure_grafana_repo.yml |
| Alloy                             | configure_grafana_repo.yml |

<sup>1 Promtail has been deprecated in favor of Alloy. However, Alloy is currently configured to *migrate* a Grafana Agent Flow config instead of create a new one. Do not use this yet</sup>