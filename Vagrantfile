# -*- mode: ruby -*-
# vi: set ft=ruby :

# All Vagrant configuration is done below. The "2" in Vagrant.configure
# configures the configuration version (we support older styles for
# backwards compatibility). Please don't change it unless you know what
# you're doing.
BOX_IMAGE = "peru/ubuntu-20.04-server-amd64"
NODE_COUNT = 2

Vagrant.configure("2") do |config|
  config.vm.provider "libvirt"

  config.vm.provision "shell", path: "./install-scripts/init_config.sh"  

  config.vm.define "master" do |subconfig|

    subconfig.vm.provider :libvirt do |libvirt|
      libvirt.cpus = 2
      libvirt.memory = 2024
    end

    subconfig.vm.box = BOX_IMAGE
    subconfig.vm.hostname = "controlplane"
    subconfig.vm.network :private_network, ip: "10.0.0.10"

  end

  (1..NODE_COUNT).each do |i|
    config.vm.define "worker#{i}" do |subconfig|

      subconfig.vm.provider :libvirt do |libvirt|
        libvirt.cpus = 2
        libvirt.memory = 1024
      end

      subconfig.vm.box = BOX_IMAGE
      subconfig.vm.hostname = "worker#{i}"
      subconfig.vm.network :private_network, ip: "10.0.0.#{i + 10}"
    end
  end
end