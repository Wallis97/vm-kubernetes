# -*- mode: ruby -*-
# vi: set ft=ruby :

BOX_IMAGE = "peru/ubuntu-20.04-server-amd64"
NODE_COUNT = 2

Vagrant.configure("2") do |config|


  # config.vm.provider "libvirt"

  config.vm.define "controlplane" do |subconfig|

    subconfig.vm.provider :libvirt do |libvirt|
      libvirt.cpus = 2
      libvirt.memory = 2024
    end

    # subconfig.vm.provision "shell", path: "./install-scripts/init_config.sh"  
    # subconfig.vm.provision "shell", path: "./install-scripts/controplane_config.sh"
    
    subconfig.vm.box = BOX_IMAGE
    subconfig.vm.hostname = "controlplane"
    subconfig.vm.network :private_network, ip: "10.0.0.10"

    # subconfig.vm.provision :ansible do |ansible|
    #   ansible.playbook = "./ansible/playbooks/controlplane-setup.yml"
    #   ansible.groups = {
    #     "cp" => ["controlplane"],
    #     "workers" => ["worker[1:#{NODE_COUNT}]"],
    #     "nodes:children" => ["cp", "workers"]
    #   }
    # end
  end

  (1..NODE_COUNT).each do |i|
    config.vm.define "worker#{i}" do |subconfig|

      subconfig.vm.provider :libvirt do |libvirt|
        libvirt.cpus = 2
        libvirt.memory = 1024
      end

      # subconfig.vm.provision "shell", path: "./install-scripts/init_config.sh"  
      
      subconfig.vm.box = BOX_IMAGE
      subconfig.vm.hostname = "worker#{i}"
      subconfig.vm.network :private_network, ip: "10.0.0.#{i + 10}"

      # subconfig.vm.provision :ansible do |ansible|
      #   ansible.playbook = "./ansible/playbooks/worker-setup.yml"
      #   # ansible.groups = {
      #   #   "workers" => ["worker[1:#{NODE_COUNT}]"],
      #   # }
      # end
    end

    config.vm.provision :ansible do |ansible|
      ansible.playbook = "./ansible/playbooks/cluster-setup.yml"
      ansible.inventory_path = ".vagrant/provisioners/ansible/inventory/vagrant_ansible_inventory"
      ansible.compatibility_mode = '2.0'
      ansible.limit = 'all'
      ansible.groups = {
        "cp" => ["controlplane"],
        "workers" => ["worker[1:#{NODE_COUNT}]"],
        "nodes:children" => ["cp", "workers"]
      }
    end
  end
end