# Ansible DigitalOcean/Vagrant LEMP stack provisioning

## Intro
This playbook can be used to quickly build/rebuild/destroy virtual servers located on the remote DigitalOcean droplet and based on Ubuntu 16.04 LTS. In case of DigitalOcean provisioning it creates a dynamic inventory, so there is no necessity to configure your Ansible connection.

## Requirements
To use this playbook, you will need to have done the following:
1. Install [Ansible](http://docs.ansible.com/intro_installation.html), Ansible 2.2+ is required.
2. Install [dopy](https://github.com/Wiredcraft/dopy).
3. Open a shell prompt (Terminal app on Mac) and cd into the cloned folder.
4. Run the following command to install the necessary Ansible roles for this profile: `$ ansible-galaxy install -r requirements.yml`
5. [Create](https://www.digitalocean.com/community/tutorials/how-to-set-up-ssh-keys--2) your ssh key pair and fill their paths into `public_ssh_key_root` and `private_ssh_key_root` variables (they can be both absolute or relative to the `do_provision.yml` file location).

## Usage

You should know your `API token` from your DigitalOcean [account](https://cloud.digitalocean.com/droplets) and add it to `vars.yml` file in `vars` directory, or add it to `DO_API_TOKEN` environment variable.

To provision your droplet just `cd` into your cloned directory and run `ansible-playbook do-provision.yml`, then you will be asked if you want your machine to be `present` (created) or `absent` (deleted) and just wait till everything is done with ansible.

To log into your server via SSH run `ssh -i /path/to/your/public_ssh_key %your_user_name%@%created_droplet_ip%`.

If you have domain directed on DigitalOcean's rDNS you can also connect it to your droplet, all you need is to add `domain_name` variable to `vars/vars.yml` file.
## Variables

There are default variables in `vars/defaults.yml` file which can be overwritten in `vars/vars.yml` file.

### Required variables
- `api_token` - it's needed for DigitalOcean provision. `DO_API_TOKEN` environment variable can be also used instead.
- `private_ssh_key_root` - path to your private ssh key (absolute or relative to `*_provision.yml` file).
- `public_ssh_key_root` - path to your public ssh key (absolute or relative to `*_provision.yml` file).

### Other variables
```
mysql_databases:
  - name: your_database_name
```
To create database you should add this variable to `vars/vars.yml` file with your preferred name.
```
domain_name: ""
```
You can connect your own domain name to the DigitalOcean droplet if you will just declare this variable with your own domain name. Don't forget that you have previously to [direct](https://www.digitalocean.com/community/tutorials/how-to-point-to-digitalocean-nameservers-from-common-domain-registrars) your domain provider's DNS on DigitalOcean's DNS.
```
droplet_name: ""
```
Name of the droplet, which will be shown in your web-interface.
```
user_name: ""
```
Name of user, which would be the main operating one, included in `sudoers` file and the only one possible to connect to remote server , while `root` login will be turned off.
```
user_pass: ""
```
The default password for `jz` is also `jz` if you would like to change it, you should put it in this document previously hashed it with SHA-512, this [instruction](http://docs.ansible.com/ansible/faq.html#how-do-i-generate-crypted-passwords-for-the-user-module) will help you to deal with it.
```
mysql_user_pass: ""
```
Pass that will be used for connections to MySQL database, default value is `jz`.
```
security_ssh_port: ""
```
Here you can choose your prefered port for ssh connections, the default one is `22`. If you are going to change it - also change `ansible_ssh_port` variable in `inventory_for_vagrant` file.

Other variables you can find in docs for the next roles from [1nfinitum's github](https://github.com/1nfinitum):

## Example vars.yml
```
---
public_ssh_key_root: "~/.ssh/id_rsa.pub"
private_ssh_key_root: "~/.ssh/id_rsa"
```
## License
MIT
## Author Information
This role was created in 2017 by [Mykhaylo Kolesnik](http://github.com/1nfinitum).
