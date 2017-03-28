# Ansible DigitalOcean/Vagrant LEMP stack provisioning

## Intro
This playbook can be used to quickly build or rebuild virtual servers located either on your local computer or on the remote DigitalOcean droplet. In case of DigitalOcean provisioning it creates a dynamic inventory, so there is no necessity to configure your Ansible connection.

This playbook:
* Creates default "deploy" user
* Installs NTP, Nginx, PHP, MySQL, Composer
* Configures base security measures including FireWall.
