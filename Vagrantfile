# -*- mode: ruby -*-
# vi: set ft=ruby :


Vagrant.configure("2") do |config|

  # config.hostmanager.enabled = true 
  # config.hostmanager.manage_host = true

  config.vm.box_check_update = false
  config.vm.box = "bento/ubuntu-24.04"
  config.vm.synced_folder ".", "/vagrant", disabled: true

  
  # ha-proxy Server configuration
  config.vm.define "grafana" do |ha|
    ha.vm.hostname = "grafana"
    ha.vm.network :private_network, ip: "192.168.56.9"
    ha.vm.provider "virtualbox" do |vb|
      vb.name = "grafana"
      vb.memory = 2048
      vb.cpus = 2
    end
  end   
  
  # # ha-proxy Server configuration
  # config.vm.define "prometheus" do |ha|
  #   ha.vm.hostname = "prometheus"
  #   ha.vm.network :private_network, ip: "192.168.50.10"
  #   ha.vm.provider "virtualbox" do |vb|
  #     vb.name = "prometheus"
  #     vb.memory = 1024
  #     vb.cpus = 2
  #   end
  # end  
  
  # # ha-proxy Server configuration
  # config.vm.define "gitea" do |ha|
  #   ha.vm.hostname = "gitea"
  #   ha.vm.network :private_network, ip: "192.168.50.11"
  #   ha.vm.provider "virtualbox" do |vb|
  #     vb.name = "gitea"
  #     vb.memory = 1024
  #     vb.cpus = 2
  #   end
  # end   
  
  # # ha-proxy Server configuration
  # config.vm.define "proxy" do |ha|
  #   ha.vm.hostname = "proxy"
  #   ha.vm.network :private_network, ip: "192.168.50.12"
  #   ha.vm.provider "virtualbox" do |vb|
  #     vb.name = "proxy"
  #     vb.memory = 2048
  #     vb.cpus = 2
  #   end
  # end  
    
  # # ha-proxy Server configuration
  # config.vm.define "dns" do |ha|
  #   ha.vm.hostname = "dns"
  #   ha.vm.network :private_network, ip: "192.168.50.13"
  #   ha.vm.provider "virtualbox" do |vb|
  #     vb.name = "dns"
  #     vb.memory = 2048
  #     vb.cpus = 2
  #   end
  # end

  # jenkins Server configuration
  config.vm.define "jenkins" do |jenkins|
    jenkins.vm.hostname = "jenkins"
    jenkins.vm.network :private_network, ip: "192.168.56.14"
    jenkins.vm.provider "virtualbox" do |vb|
      vb.name = "jenkins"
      vb.memory = 2048
      vb.cpus = 2
    end
  end
  
  # # nexus Server configuration
  # config.vm.define "nexus" do |nexus|
  #   nexus.vm.hostname = "nexus"
  #   nexus.vm.network :private_network, ip: "192.168.50.15"
  #   nexus.vm.provider "virtualbox" do |vb|
  #     vb.name = "nexus"
  #     vb.memory = 2048
  #     vb.cpus = 2
  #   end
  # end
  
  # # master03 Server configuration
  # config.vm.define "sonar" do |sonar|
  #   sonar.vm.hostname = "sonar"
  #   sonar.vm.network :private_network, ip: "192.168.50.16"
  #   sonar.vm.provider "virtualbox" do |vb|
  #     vb.name = "sonar"
  #     vb.memory = 2048
  #     vb.cpus = 2
  #   end
  # end

  # master01 Server configuration
  config.vm.define "k8s-master" do |master|
    master.vm.hostname = "k8s-master"
    master.vm.network :private_network, ip: "192.168.56.17"
    master.vm.provider "virtualbox" do |vb|
      vb.name = "k8s-master"
      vb.memory = 8192
      vb.cpus = 4
    end
  end

  # node01 Server configuration
  config.vm.define "k8s-node01" do |node|
    node.vm.hostname = "k8s-node01"
    node.vm.network :private_network, ip: "192.168.56.18"
    node.vm.provider "virtualbox" do |vb|
      vb.name = "k8s-node01"
      vb.memory = 4096
      vb.cpus = 2
    end
  end

  # node02 Server Configuration 
  config.vm.define "k8s-node02" do |node|
    node.vm.hostname = "k8s-node02"
    node.vm.network :private_network, ip: "192.168.56.19"
    node.vm.provider "virtualbox" do |vb|
      vb.name = "k8s-node02"
      vb.memory = 4096
      vb.cpus = 2
    end
  end

end
