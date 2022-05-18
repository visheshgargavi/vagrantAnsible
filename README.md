# vagrantAnsible

The goal is to create an Ansible script that provisions an Ubuntu server with the following components. Applications should be installed with rootless Podman containers running as unprivileged user apps (unless stated otherwise). The Ansible script should be delivered on GitHub and regular commits should be done so that it's visible that some progress is going on.

• Ansible script should not expect to be root on the destination but should use sudo instead delivery of the Ansible script should happen on a private repository on https://gitlab.com/ 

• you can use this project as a reference (it is using rootless Podman containers with Systemd, sets up Prometheus, Wireguard, a firewall for using privileged ports [in the ingress role] and various apps in the compose directory (i.e. grafana, healthchecks). ): https://github.com/brettinternet/homelab 

• use and commit a locally defined Vagrantfile configuration for easy testing and developing: https://www.vagrantup.com/docs/provisioning/ansible 

• all containers should log to a Loki instance which should be running on the same host as Podman container ( https://grafana.com/docs/loki/latest/installation/#install-loki ) - here's an example configuration with Vector: https://github.com/livingdocsIO/monitoring/blob/master/dockercompose.yaml 

• MOTD should be configurable • block all ports except SSH, email, HTTP, VPN, BigBlueButton ports, AdGuardHome ports ( 53/tcp , 67/udp , 853/tcp , 784/udp , 5443/tcp ) and GitLab (Git+SSH, HTTP[S] and Docker registry) ports via firewall Additionally, write a flask app in Python to trigger ansible run using API

## Install WSL
```hcl
Search “Turn Windows feature on or off”
Turn on the “Windows Subsystem for Linux”
Then restart the system
Open Microsoft store and download linux (here I downloaded Ubuntu linux)
Open ubuntu linux and set username and password
```

## Install Vagrant and Ansible

To install vagrant:
```hcl
Go to the official website of vagrant and download it for windows
Install vagrant
Open cmd and run bash, you will be redirected to the ubuntu terminal

Now run:
vim .bashrc

export VAGRANT_WSL_WINDOWS_ACCESS_USER_HOME_PATH="/mnt/c/Users/user"
export VAGRANT_WSL_ENABLE_WINDOW_ACCESS="1"
export PATH="$PATH:/mnt/c/Program Files/Oracle/VirtualBox"

source .bashrc
cd /mnt/Users/user/Desktop
mkdir vagrant
cd vagrant
vagrant init ubuntu/trusty64
apt-get install ansible -y
```

## To run and provision a vagrant script
```hcl
vagrant up
vagrant provision
```

## To create, run ansible role and syntax-check
```hcl
ansible -m ping vagrant
ansible-galaxy init role_name
ansible-playbook --syntax-check playbook_name.yml
ansible-playbook playbook_name.yml
```

## Install ansible packages
```hcl
ansible-galaxy collection install containers.podman
ansible-galaxy collection install community.general
ansible-galaxy collection install ansible.posix
```
