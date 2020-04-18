# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.require_version '>= 2.1.5'

Vagrant.configure("2") do |config|
    config.vagrant.plugins = ["vagrant-hostsupdater", "vagrant-vbguest", "vagrant-bindfs"]

    config.vm.define "acme"
    config.vm.hostname = "acme.local"

	config.vm.box = "debian/stretch64"
	config.vm.box_version = ">=9.12"

	config.vm.network "private_network", ip: "192.168.20.103"
	config.ssh.forward_agent = true

	config.vm.synced_folder "./", "/vagrant", type: "virtualbox"
    config.vm.synced_folder "./src", "/vagrant-nfs", type: "nfs", create: true
	config.bindfs.bind_folder "/vagrant-nfs", "/var/www/acme",
        :owner => "vagrant", :group => "vagrant",
        :'create-as-user' => true,
        :'chmod-allow-x' => true, :'chown-ignore' => true, :'chgrp-ignore' => true, :'chmod-ignore' => true,
        :perms => "u=rwX:g=rwX:o=rD"

    config.vm.provider "virtualbox" do |v|
        v.name = config.vm.hostname
        v.memory = 4096
        v.cpus = 2
    end

    config.vm.provision "ansible_local" do |ansible|
    	ansible.become = true
    	ansible.install = true
    	ansible.install_mode = "pip"
    	ansible.playbook = "ansible/playbook.yml"
    	ansible.galaxy_role_file = "ansible/roles.yml"
    	ansible.galaxy_command = "sudo ansible-galaxy install --role-file=%{role_file} --roles-path=%{roles_path}"
    end
end