# Ansible DigitalOcean/Vagrant LEMP stack provisioning

## Intro
This playbook can be used to quickly build or rebuild virtual servers located either on your local computer or on the remote DigitalOcean droplet. In case of DigitalOcean provisioning it creates a dynamic inventory, so there is no necessity to configure your Ansible connection.

This playbook:
* Creates default "deploy" user
* Installs NTP, Nginx, PHP, MySQL, Composer
* Configures base security measures including FireWall.

## Requirements
To use this playbook, you will need to have done the following:

1. Install [Ansible](http://docs.ansible.com/intro_installation.html)
2. Open a shell prompt (Terminal app on Mac) and cd into the folder containing Vagrantfile.
3. Run the following command to install the necessary Ansible roles for this profile: `$ ansible-galaxy install -r requirements.yml`

If you will be using Vagrant local provisioning you should also do the following:
1. Download and Install [VirtualBox](https://www.virtualbox.org/wiki/Downloads)
2. Download and Install [Vagrant](https://www.vagrantup.com/downloads.html)
