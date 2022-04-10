# -*- mode: ruby -*-
# vi: set ft=ruby :

# All Vagrant configuration is done below. The "2" in Vagrant.configure
# configures the configuration version (we support older styles for
# backwards compatibility). Please don't change it unless you know what
# you're doing.
Vagrant.configure("2") do |config|

  config.vm.define "wp" do |wp|
    wp.vm.box = "ubuntu/focal64"
    wp.vm.box_check_update = false
    wp.vm.hostname = "wp"
    wp.vm.network "private_network", ip: "192.168.30.5", nic_type: "virtio", virtualbox__intnet: "keepcoding"
    wp.vm.network "forwarded_port", guest: 80, host: 8081
    wp.vm.provider "virtualbox" do |vb|
      vb.name = "wp"
      vb.cpus='1'
      vb.memory = "1024"
      #mirar
      vb.default_nic_type = "virtio"
      file_to_disk = "wp.vmdk"
      #unless condicional->ejecuta si condi=false
      unless File.exist?(file_to_disk)
          vb.customize [ "createmedium", "disk", "--filename", "wp.vmdk", "--format", "vmdk", "--size", 1024 * 1 ]
      end
      #wsl2 error
      vb.customize [ "modifyvm", :id, "--uartmode1", "disconnected" ]
    end
  end  

  config.vm.define "el" do |el|
    el.vm.box = "ubuntu/focal64"
    el.vm.box_check_update = false
    el.vm.hostname = "wp"
    el.vm.network "private_network", ip: "192.168.30.6", nic_type: "virtio", virtualbox__intnet: "keepcoding"
    el.vm.network "forwarded_port", guest: 9200, host: 9200
    el.vm.network "forwarded_port", guest: 80, host: 8080
    el.vm.provider "virtualbox" do |vb|
      vb.name = "el"
      vb.cpus='1'
      vb.memory = "4096"
      vb.default_nic_type = "virtio"
      file_to_disk = "el.vmdk"
      unless File.exist?(file_to_disk)
          vb.customize [ "createmedium", "disk", "--filename", "el.vmdk", "--format", "vmdk", "--size", 1024 * 1 ]
      end
      vb.customize [ "storageattach", "wp" , "--storagectl", "SCSI", "--port", "2", "--device", "0", "--type", "hdd", "--medium", file_to_disk]
      #wsl2 error
      vb.customize [ "modifyvm", :id, "--uartmode1", "disconnected" ]
    end
  end 

end

