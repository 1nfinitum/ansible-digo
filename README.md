# Ansible DigitalOcean/Vagrant LEMP stack provisioning

## Intro
This playbook can be used to quickly build/rebuild virtual servers located either on your local computer or on the remote DigitalOcean droplet. In case of DigitalOcean provisioning it creates a dynamic inventory, so there is no necessity to configure your Ansible connection.

This playbook:
* Creates default "deploy" user
* Installs NTP, Nginx, PHP, MySQL, Composer
* Configures base security measures (disables remote `root` login and others)

## Requirements
To use this playbook, you will need to have done the following:
1. Install [Ansible](http://docs.ansible.com/intro_installation.html), Ansible 2.2+ is required.
2. Open a shell prompt (Terminal app on Mac) and cd into the folder containing `Vagrantfile`.
3. Run the following command to install the necessary Ansible roles for this profile: `$ ansible-galaxy install -r requirements.yml`
4. [Create](https://www.digitalocean.com/community/tutorials/how-to-set-up-ssh-keys--2) your ssh key pair and fill their paths into `public_ssh_key_root` and `private_ssh_key_root` variables (they can be both absolute or relative to the `*_provision.yml` file location).

If you will be using Vagrant local provisioning you should also do the following:
1. Download and Install [VirtualBox](https://www.virtualbox.org/wiki/Downloads)
2. Download and Install [Vagrant](https://www.vagrantup.com/downloads.html)


## Usage

### Vagrant
To provision your local environment you can simply type `vagrant up` (in the directory containing `Vagrantfile`) and wait until Vagrant will create a new VM, install the base box, and configure it.

Once the Vagrant VM is up and runing (after `vagrant up` is complete and you're back at the command prompt), you can log into via SSH with `vagrant ssh` or with `ssh -i ~/.vagrant.d/insecure_private_key deploy@192.168.33.34`. 

### DigitalOcean
You should know your `API token` from your DigitalOcean [account](https://cloud.digitalocean.com/droplets) and add it to `vars.yml` file in `vars` directory, or add it to `DO_API_TOKEN` environment variable.

To provision your droplet just `cd` into the provisioning directory and run `ansible-playbook do-provision.yml`, then you will asked if you want your machine to be `present` (created) or `absent` (deleted) and just wait till everything is done with ansible.

To log into your server via SSH run `ssh -i /path/to/your/public_ssh_key deploy@%created_droplet_ip%`.

## Variables

There are default variables in `vars/defaults.yml` file which can be overwritten in `vars/vars.yml` file.

### Required variables
- `api_token` - it's needed for DigitalOcean provision. `DO_API_TOKEN` environment variable can be also used instead.
- `private_ssh_key_root` - path to your private ssh key (absolute or relative to `*_provision.yml` file).
- `public_ssh_key_root` - path to your public ssh key (absolute or relative to `*_provision.yml` file).

### Other variables
```
droplet_name: ""
```
Name of the droplet, which will be shown in your web-interface.
```
deploy_user: ""
```
Name of user, which would be the main operating one, included in `sudoers` file and the only one possible to connect to remote server as remote `root` login will be turned off.
```
user_pass: ""
```
The default password for `deploy` is also `deploy` if you would like to change it, you should put it in this document previously hashed it with SHA-512, this [instruction](http://docs.ansible.com/ansible/faq.html#how-do-i-generate-crypted-passwords-for-the-user-module) will help you to deal with it.
