# Hermes Local Deployment Instructions

## Tools used:

1. `Vagrant` (and VirtualBox) to setup a VM.
2. `Ansible` to automate the deployment process to the VM.
3.  `Docker` & `docker-compose` for a containerized deployment.

## Create the VM
 
To create the VM, just execute :
```
vagrant up
```

Please **note** that you will be prompted to choose an interface for the network to bridge to. When choosing an interface, it is usually the one that is being used to connect to the internet.

If you wish, you may update your `~/.ssh/config` file so that ssh  is available without `vagrant ssh`:

```
Host VagrantStation
 	HostName 192.168.1.69
 	User vagrant
 	IdentityFile ~/.ssh/id_rsa
```

## Deploying the app (with ansible playbooks)

Just execute:

```
# Install required ansible role from Ansible Galaxy
ansible-galaxy install geerlingguy.docker

# Run the ansible playbook
ansible-playbook -i hosts playbook.yml
```
This ansible playbook will:

- Install `docker` & `docker-compose` on the VM
- Copy the required files (`docker-compose.yaml` and NGINX's `default.conf`) to the VM
- Run `docker-compose up -d` on the VM to deploy the app.

## Access the app

First, add this line to your `/etc/hosts` file:

```
192.168.1.69	internal.hermes.local external.hermes.local sso.hermes.local mailhog.hermes.local
```

You may then access these apps locally:

- Internal Front-End -> http://internal.hermes.local
- External Front-End -> http://internal.hermes.local
- Keycloak -> http://sso.hermes.local
- Mailhog (Mock Mail Server) -> http://mailhog.hermes.local

## Delete the VM

To delete the VM along with all its' files execute : 

```
vagrant destroy
```