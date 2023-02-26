# -*- mode: ruby -*-
# vi: set ft=ruby :


Vagrant.configure("2") do |config|
  
  config.ssh.insert_key = false
    
  config.vm.synced_folder ".", "/vagrant", disabled: true

  # Master node server configuration
  config.vm.define "master" do |master|
    master.vm.box = "geerlingguy/ubuntu1604"
    master.vm.hostname = "master"
    master.vm.network :private_network, ip: "192.168.50.10"
    master.vm.provider "virtualbox" do |vb|
      vb.name = "master"
      vb.memory = 2048
      vb.cpus = 2
    end
  end
  
  # DB Server configuration
  config.vm.define "db" do |db|
    db.vm.box = "geerlingguy/centos7"
    db.vm.hostname = "db"
    db.vm.network :private_network, ip: "192.168.50.12"
    db.vm.provider "virtualbox" do |vb|
      vb.name = "db"
      vb.memory = 1024
      vb.cpus = 2
    end
  end

  # Development Server configuration
  config.vm.define "dev" do |dev|
    dev.vm.box = "geerlingguy/centos7"
    dev.vm.hostname = "dev"
    dev.vm.network :private_network, ip: "192.168.50.13"
    dev.vm.provider "virtualbox" do |vb|
      vb.name = "dev"
      vb.memory = 1024
      vb.cpus = 2
    end
  end

  # Staging Server Configuration 
  config.vm.define "stage" do |stage|
    stage.vm.box = "geerlingguy/centos7"
    stage.vm.hostname = "stage"
    stage.vm.network :private_network, ip: "192.168.50.15"
    stage.vm.provider "virtualbox" do |vb|
      vb.name = "stage"
      vb.memory = 1024
      vb.cpus = 2
    end
  end

  # Production Server configuration
  config.vm.define "prod" do |prod|
    prod.vm.box = "twistedvines/openshift-4.3.0"
    prod.vm.box_version = "1.0.0"
    prod.vm.hostname = "prod"
    prod.vm.network :private_network, ip: "192.168.50.16"
    prod.vm.provider "virtualbox" do |vb|
      vb.name = "prod"
      vb.memory = 1024
      vb.cpus = 2
    end
  end

end
