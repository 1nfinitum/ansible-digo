# Ansible DigitalOcean/Vagrant LEMP stack provisioning

## Intro
This playbook can be used to quickly build/rebuild/destroy virtual servers located either on your local computer or on the remote DigitalOcean droplet and based on CentOS 6 or Ubuntu 16.04 LTS, depending on your favors. In case of DigitalOcean provisioning it creates a dynamic inventory, so there is no necessity to configure your Ansible connection.

This playbook:
* Creates default "deploy" user
* Installs NTP, Nginx, PHP, MySQL, Composer
* Configures base security measures (disables remote `root` login and others)
* Connects DigitalOcean droplet to domain name

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
Default distribution used in `Vagrantfile` is `geerlingguy/centos6` (CentOS 6). If you want to run with Ubuntu 16.04 LTS - just change the distribution in line `config.vm.box = "geerlingguy/centos6"` to `geerlingguy/ubuntu1604` and then accomplish the following steps. As it playbook is supposed to work with most Debian/RedHat distributions - feel free to try some other OS with it. 

To provision your local environment you can simply type `vagrant up` (in the directory containing `Vagrantfile`) and wait until Vagrant will create a new VM, install the base box, and configure it.

Once the Vagrant VM is up and runing (after `vagrant up` is complete and you're back at the command prompt), you can log into via SSH with `vagrant ssh` or with `ssh -i ~/.vagrant.d/insecure_private_key deploy@192.168.33.34`. 

### DigitalOcean
You should know your `API token` from your DigitalOcean [account](https://cloud.digitalocean.com/droplets) and add it to `vars.yml` file in `vars` directory, or add it to `DO_API_TOKEN` environment variable.

To provision your droplet just `cd` into the provisioning directory and run `ansible-playbook do-provision.yml`, then you will asked if you want your machine to be `present` (created) or `absent` (deleted) and just wait till everything is done with ansible.

To log into your server via SSH run `ssh -i /path/to/your/public_ssh_key deploy@%created_droplet_ip%`.

If you have domain directed on DigitalOcean's rDNS you can also connect it to your droplet, all you need is to add `domain_name` variable to `vars/vars.yml` file.
## Variables

There are default variables in `vars/defaults.yml` file which can be overwritten in `vars/vars.yml` file.

### Required variables
- `api_token` - it's needed for DigitalOcean provision. `DO_API_TOKEN` environment variable can be also used instead.
- `private_ssh_key_root` - path to your private ssh key (absolute or relative to `*_provision.yml` file).
- `public_ssh_key_root` - path to your public ssh key (absolute or relative to `*_provision.yml` file).

### Other variables
```
distro: ""
```
You can choose what distribution to use while provisioning. The default is `centos-6-x32` meaning that is CentOS 6 x32 distribution according to the image list retrieves from [DigitalOcean API](https://developers.digitalocean.com/documentation/v2/#list-all-images). This playbook was also tested with `ubuntu-16-04-x64` (Ubuntu 16.04 LTS x64) distribution. It also supposes to work with most Debian/RedHat based OS, just have a try.
```
domain_name: ""
```
You can connect your own domain name to the DigitalOcean droplet if you will just declare this variable with your own domain name. Don't forget that you have previously to [direct](https://www.digitalocean.com/community/tutorials/how-to-point-to-digitalocean-nameservers-from-common-domain-registrars) your domain provider's DNS on DigitalOcean's DNS.
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
```
mysql_user_pass: ""
```
Pass that will be used for connections to MariaDB database, default value is `deploy`.
```
security_ssh_port: ""
```
Here you can choose your prefered port for ssh connections, the default one is `22`. If you are going to change it - also change `ansible_ssh_port` variable in `inventory_for_vagrant` file.
```
firewall_allowed_tcp_ports: []
```
Here you can write down list of ports will be exposed for connection, the default ports are `25, 80, 3306` and port mentioned in `security_ssh_port` used for SSH connections.
```
mysql_packages: []
```
List of packages which be installed for mysql, default packages are the ones that install MariaDB.
```
php_packages: []
```
List of PHP packages that will be installed.
```
php_webserver_daemon: nginx
php_enable_php_fpm: true
```
Variables for configuration of Nginx and PHP communication.

Other variables you can find in docs for the next roles from [Geerlingguy's github](https://github.com/geerlingguy):
```
geerlingguy.security
geerlingguy.firewall
geerlingguy.ntp
geerlingguy.mysql
geerlingguy.nginx
geerlingguy.php
geerlingguy.composer
```
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
