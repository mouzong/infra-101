# -*- mode: ruby -*-
# vi: set ft=ruby :


Vagrant.configure("2") do |config|
  

  # master01 Server configuration
  config.vm.define "master01" do |master|
    master.vm.box = "ubuntu/focal64"
    master.vm.hostname = "master01"
    master.vm.network :private_network, ip: "192.168.50.11"
    master.vm.provider "virtualbox" do |vb|
      vb.name = "master01"
      vb.memory = 4096
      vb.cpus = 2
    end
  end
  
  # master02 Server configuration
  config.vm.define "master02" do |master|
    master.vm.box = "ubuntu/focal64"
    master.vm.hostname = "master02"
    master.vm.network :private_network, ip: "192.168.50.12"
    master.vm.provider "virtualbox" do |vb|
      vb.name = "master02"
      vb.memory = 4096
      vb.cpus = 2
    end
  end
  
  # master03 Server configuration
  config.vm.define "master03" do |master|
    master.vm.box = "ubuntu/focal64"
    master.vm.hostname = "master03"
    master.vm.network :private_network, ip: "192.168.50.13"
    master.vm.provider "virtualbox" do |vb|
      vb.name = "master03"
      vb.memory = 4096
      vb.cpus = 2
    end
  end

  # node01 Server configuration
  config.vm.define "node01" do |node|
    node.vm.box = "ubuntu/focal64"
    node.vm.hostname = "node01"
    node.vm.network :private_network, ip: "192.168.50.14"
    node.vm.provider "virtualbox" do |vb|
      vb.name = "node01"
      vb.memory = 2048
      vb.cpus = 2
    end
  end

  # node02 Server Configuration 
  config.vm.define "node02" do |node|
    node.vm.box = "ubuntu/focal64"
    node.vm.hostname = "node02"
    node.vm.network :private_network, ip: "192.168.50.15"
    node.vm.provider "virtualbox" do |vb|
      vb.name = "node02"
      vb.memory = 2048
      vb.cpus = 2
    end
  end

  # node03 Server configuration
  config.vm.define "node03" do |node|
    node.vm.box = "ubuntu/focal64"
    node.vm.hostname = "node03"
    node.vm.network :private_network, ip: "192.168.50.16"
    node.vm.provider "virtualbox" do |vb|
      vb.name = "node03"
      vb.memory = 2048
      vb.cpus = 2
    end
  end

end
