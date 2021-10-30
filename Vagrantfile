# -*- mode: ruby -*-
# vi: set ft=ruby :

VAGRANTFILE_API_VERSION = "2"

Vagrant.configure("2") do |config|
  config.vm.box = "ubuntu/focal64"
  config.ssh.insert_key = false
  config.vm.provider "virtualbox"

  config.vm.provider :virtualbox do |v|
    v.memory = 8192
    v.cpus = 4
    v.linked_clone = true
  end

  # Define six VMs with static private IP addresses.
  boxes = [
    { :name => "k8s-0", :ip => "192.168.42.10" },
    { :name => "k8s-1", :ip => "192.168.42.11" },
    { :name => "k8s-2", :ip => "192.168.42.12" },
  ]

  # Provision each of the VMs.
  boxes.each do |opts|
    config.vm.define opts[:name] do |config|
      config.vm.hostname = opts[:name]
      config.vm.network "private_network", ip: opts[:ip]

      # Provision all the VMs using Ansible after last VM is up.
      if opts[:name] == "k8s-5"
        config.vm.provision "ansible" do |ansible|
          ansible.playbook = "main.yml"
          ansible.inventory_path = "inventory"
          ansible.limit = "all"
        end
      end
    end
  end

end
