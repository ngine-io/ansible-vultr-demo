# Ansible Vultr Demo

In this demo, we are going to show how to use the ngine_io.vultr_cloud role to setup a typical 2-tier Cloud environment, including:

- VPC networks
- SSH public keys
- Block Storage
- Firewall groups and rules
- Server instances
- (Optinally with different accounts/api keys)

## Setup

```
python3.11 -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt

# refresh venv
deactivate
source .venv/bin/activate
```

## Install

Clone this repo:

```
git clone git@github.com:ngine-io/ansible-vultr-demo.git
cd ansible-vultr-demo
```

Install latest vultr collection and vultr role:

```
ansible-galaxy collection install -r requirements.yml
ansible-galaxy role install -r requirements.yml
```

## Vultr API Keys

In case you want to have different api keys (accounts) per tier, place your api keys into `./secrets/<tier>/vultr_api_key` and uncomment the variable `vultr__api_key` in `./inventory/group_vars/all/vultr.yml`.

In case you only have one or want to use an environment variable, you can alternatively use

```
export VULTR_API_KEY=xyz
```

## Configuration

We defined 2 inventory files which represent our tiers: production and staging.

Each with (almost) identical groups but different amount of hosts. On the top of each inventory, we define a group named like the corresponding tier.This allows to define and configure resource which are different per tier: such as region, VPC name, network and so on.

## Deployment

The playbook `playbooks/cloud.yml` looks rather simple and with the following command executed, we deploy our infra, such as ssh keys, networks, firewall rules, block storage and instances for our staging tier:

To also run all playbooks, we just run the `site.yml`:

```
ansible-playbook playbooks/site.yml -i inventory/staging.yml
```

After that, we can switch to production:

```
ansible-playbook playbooks/site.yml -i inventory/production.yml
```

## License

MIT

## Author Information

René Moser (@resmo)
