# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
  config.vm.box = "debian/bookworm64"

  config.vm.provider "virtualbox" do |vb|
    vb.customize ["modifyvm", :id, "--cpuexecutioncap", "50"]
    vb.linked_clone = true
    vb.memory = "2048"
    vb.cpus = "4"
  end

  config.vm.define "router" do |machine|
    machine.vm.hostname = "router"
    machine.vm.network "private_network", virtualbox__intnet: "LAN1", auto_config: false
    machine.vm.network "private_network", virtualbox__intnet: "LAN2", auto_config: false
  end
  (1..2).each do |i|
    config.vm.define "controller#{i}" do |machine|
      machine.vm.hostname = "controller#{i}"
      machine.vm.network "private_network", virtualbox__intnet: "LAN#{i}", auto_config: false
    end
    (1..2).each do |j|
      config.vm.define "slurm#{i}-#{j}" do |machine|
        machine.vm.hostname = "slurm#{i}-#{j}"
        machine.vm.network "private_network", virtualbox__intnet: "LAN#{i}", auto_config: false
      end
    end
  end

  config.vm.provision "ansible" do |ansible|
    ansible.playbook = "site.yml"
  end
end
