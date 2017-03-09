# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure(2) do |config|
  config.vm.synced_folder ".", "/vagrant", type: "rsync",
    rsync__exclude: ".git/"
  config.vm.box = "ubuntu/trusty64"
  config.vm.box_check_update = true
  config.vm.network "public_network"

  %w(buildsrv).each do |node|
    config.vm.define node do |vmdef|
      vmdef.vm.box = "ubuntu/trusty64"
      vmdef.vm.hostname = node
      vmdef.vm.provider "virtualbox" do |vm|
        vm.name = node
       # vm.check_guest_tools = false
        vm.memory = 2096
        vm.cpus = 2
      end
      vmdef.vm.provision "ansible" do |ansible|
        ansible.playbook = "provisioning/site.yml"
      end
    end
  end
end
