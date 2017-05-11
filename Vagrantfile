# -*- mode: ruby -*-
# vi: set ft=ruby :

# Require YAML module
require 'yaml'
 
# Read YAML file with box details
PWD = ENV['PWD']
group_vars = YAML.load_file(PWD + '/inventories/vagrant/group_vars/all.yml')
host_vars = YAML.load_file(PWD + '/inventories/vagrant/host_vars/all.yml')


Vagrant.configure("2") do |config|
  config.vm.box = "parallels/ubuntu-14.04"

  # config.vm.synced_folder "host_path", "guest_path", id: "id"
  # config.vm.network "forwarded_port",  host: "host_port", guest: "guest_port", id: "id_port"
  config.vm.synced_folder ".", "/vagrant", id: "vagrant_root", disabled: true
  # config.vm.synced_folder "./src", "/var/www", id: "source_root"

  config.vm.provider :virtualbox do |vm|
    vm.customize ["modifyvm", :id, "--memory", 2048 ]
    vm.customize ["modifyvm", :id, "--name", "vagrant" ]
    vm.customize ["modifyvm", :id, "--cableconnected1", "on"]
    # Connection cable on network interface for private network
    #vm.customize ["modifyvm", :id, "--cableconnected2", "on"]
  end

  config.vm.provider :parallels do |vm|
    vm.name = "vagrant"
    vm.memory = 2048
    vm.check_guest_tools = false
  end

  config.vm.provider "vmware_fusion" do |v|
    v.vmx["memsize"] = "2048"
  end

  config.vm.provision :local, type: :ansible, run: :always do |ansible|
    ansible.playbook = './site.yml'
    # ansible.tags = docker
    ansible.verbose = '-vv'
    ansible.extra_vars = group_vars 
    ansible.groups = {
      "vagrant" => ["default"],
      "all_groups:children" => ["vagrant"]
    }
    # ansible.host_vars = host_vars
  end
end
