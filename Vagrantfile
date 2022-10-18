# -*- mode: ruby -*-
# vi: set ft=ruby :
# virsh pool-create-as --name data --type dir --target /mnt/data

Vagrant.configure("2") do |config|
  config.vm.box = "generic/ubuntu2004"
  config.vm.define "ubuntu_192.168.33.111"
  config.vm.network "private_network", ip: "192.168.33.111"
  config.vm.hostname = "ubuntu20.04"

  config.vm.provider "libvirt" do |kvm|
    kvm.memory = 4096 
    kvm.cpus = 2
    kvm.storage_pool_name = "data"
    kvm.machine_type = "q35"
  end

  config.vm.provision "shell", inline: <<-SHELL
   sudo apt update
   sudo apt install software-properties-common
   sudo apt-add-repository --yes --update ppa:ansible/ansible
   sudo apt install -y ansible
   git clone https://github.com/developer-onizuka/ansible
   ansible-playbook ansible/google-chrome.yaml
   SHELL
end
