# Ansible playbook for AiiDA Lab Server

This is an ansible playbook for setting up a multi-user [AiiDA lab](https://aiidalab.materialscloud.org) 
either on a physical server or on a virtual machine (e.g. on Amazon Web Services or OpenStack).

### Prerequisites

- A server running Ubuntu 18.04 with open ports 22 (ssh), 80 (http) and 443 (https)
- passwordless ssh access to the server via ssh key
- [python](https://www.python.org/)

```
git clone git@github.com:aiidalab/ansible-playbook-aiidalab.git
cd ansible-playbook-aiidalab
pip install -r requirements.txt
ansible-galaxy install -r requirements.yml
```

### Set up Virtual Machine

1. select aws/os host in `./hosts` file
1. Open corresponding `./host_vars/*.yml` file and adapt path to SSH private key
1. run `ansible-playbook playbook.yml`

Once finished, navigate to the IP address of your server.

## Acknowledgements

This work is supported by the [MARVEL National Centre for Competency in Research](http://nccr-marvel.ch) 
and the [MaX European centre of excellence](http://www.max-centre.eu/)
