[![Build Status](https://travis-ci.org/aiidalab/ansible-playbook-aiidalab-server.svg?branch=master)](https://travis-ci.org/aiidalab/ansible-playbook-aiidalab-server)
[![Release](https://img.shields.io/github/tag/aiidalab/ansible-playbook-aiidalab-server.svg)](https://github.com/aiidalab/ansible-playbook-aiidalab-server/releases)

# Ansible playbook for AiiDA Lab Server

This is an [ansible](https://www.ansible.com/overview/how-ansible-works) playbook for setting up a multi-user [AiiDA lab](https://aiidalab.materialscloud.org)
either on a physical server or on a virtual machine (e.g. on Amazon Web Services or OpenStack).

### Prerequisites

### Server

- A server running Ubuntu 18.04 with open ports 22 (ssh), 80 (http) and 443 (https).
- SSH access to the server via passwordless private SSH key.
  The user must have `sudo` rights.

For instructions on how to set up cloud services, see [here](https://tljh.jupyter.org/en/latest/index.html).

### Your machine

- [bash](https://www.gnu.org/software/bash/)
- [python](https://www.python.org/)
- [git](https://git-scm.com/)

Instructions for the terminal:
```
git clone https://github.com/aiidalab/ansible-playbook-aiidalab-server.git
cd ansible-playbook-aiidalab
pip install -r requirements.txt
ansible-galaxy install -r requirements.yml
```

### Installation

1. Edit the [`hosts`](hosts) file to provide the IP address and SSH key for your server
1. Execute `ansible-playbook playbook.yml --ask-become-pass`

Once finished, open the IP address of your server in a web browser.

**Warning:** To keep things simple, this uses the [`FirstUseAuthenticator`](https://github.com/jupyterhub/firstuseauthenticator), which simply creates new JupyterHub users when they enter their username and password.
In order to disable the creation of new users, set `aiidalab_server_create_users` to `false` in the [`playbook.yml`](playbook.yml) and run the ansible playbook again.

## Modifying the user environment

By default, the playbook will simply pull the docker image for the AiiDA lab accounts from the [DockerHub registry](https://hub.docker.com/r/aiidalab/aiidalab-docker-stack).

If you would like to modify the user environment (e.g. to install additional packages), uncomment the `aiidalab_server_build_locally: true` line in the playbook and run the ansible playbook again.

This will cause the `marvel-nccr.aiidalab_server` role to clone the git repository containing the `Dockerfile` and build the docker image locally from it. You can then update the image by modifying the `Dockerfile` on the server:

```
ssh aiidalab  # ssh into your aiida lab server
cd ~/aiidalab-docker-stack
vim Dockerfile  # edit Dockerfile
docker build . -t aiidalab-docker-stack:latest
```

Once the new image is built, it will be used for new containers (i.e. if you already have a container running, use the JupyterHub control panel to stop your server and start it again).


## Connecting to authentication services

The `FirstUseAuthenticator` can be useful to get started but for production you may want to connect to an OAuth2 server for authentication.
See the [OAuthenticator](https://github.com/jupyterhub/oauthenticator) for instructions on how to connect to external authentication services.

### Cheatsheet

- Run only `aiidalab_server` role: `ansible-playbook playbook.yml --ask-become-pass --tags aiidalab_server --skip-tags dependencies`

On the server: 
- List running containers: `docker ps`
- List resource usage: `docker stats`
- View log of a container: `docker container logs  <container_id>`
- View JupyterHub log: `tail -f /var/log/syslog | grep jupyterhub`
- Restart JupyterHub: `sudo service jupyterhub restart`
- Restart Apache: `sudo apachectl graceful`

## Acknowledgements

This work is supported by the [MARVEL National Centre for Competency in Research](http://nccr-marvel.ch)
and the [MaX European centre of excellence](http://www.max-centre.eu/)
