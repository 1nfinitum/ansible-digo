# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
  config.vm.box = "geerlingguy/ubuntu1604"
  config.ssh.insert_key = false

  config.vm.hostname = "digo"
  config.vm.network "private_network", ip: "192.168.77.77"

  config.vm.provider :virtualbox do |v|
      v.name = "digo"
      v.memory = 512
      v.cpus = 1
      v.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]
      v.customize ["modifyvm", :id, "--ioapic", "on"]
  end

  # Set the name of the VM. See: http://stackoverflow.com/a/17864388/100134
  config.vm.define :digo do |digo|
  end

  config.vm.provision "ansible" do |ansible|
    ansible.playbook = "vagrant.yml"
    ansible.inventory_path = "inventory"
    ansible.sudo = true
  end
end
