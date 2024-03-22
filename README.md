# Homelab Configuration

A collection of Ansible playbooks for my homelab. Currently this is a work in progress, as I've had my homelab for nearly 2 years now and everything has been done manually. I'm converting to Ansible to A) provide easier deployment in the future, and B) be able to spin everything back up in the event of total failure.

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

You can define the loki variables directly in the command
`ansible-playbook -e loki_ip_address=<loki_ip> -e loki_port=<loki_port> ./ansible/install_promtail.yml`

Or, you can define them in a YAML or JSON file and run the command this way:
`ansible-playbook -e @<your env file> ./ansible/install_promtail.yml`

If the variables aren't provided, they will default to `127.0.0.1` and `3100`, respectively.
