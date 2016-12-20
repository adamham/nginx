# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure('2') do |config|

  config.vm.define 'dev' do |machine|
    machine.vm.box = "ubuntu/trusty64"
    #machine.vm.box = "debian/jessie64"
    #machine.vm.box = "centos/7"

    machine.vm.network :private_network, ip: '10.11.12.13'
    machine.vm.hostname = 'dev.local'
    machine.vm.provision 'ansible' do |ansible|
      ansible.playbook = 'setup.yml'
      ansible.sudo = true
      ansible.inventory_path = 'vagrant-inventory'
      ansible.host_key_checking = false
    end

  end

end
