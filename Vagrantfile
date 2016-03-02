# -*- mode: ruby -*-
# vi: set ft=ruby :
# vi: set tabstop=2 :
# vi: set shiftwidth=2 :

Vagrant.configure("2") do |config|

  vms = [
    [ "debian-jessie", "debian/jessie64" ]
  ]

  second_drive = '.vagrant/disk_data.vdi'

  config.vm.provider "virtualbox" do |v|
    v.cpus = 1
    v.memory = 256
    v.customize ['createhd', '--filename', second_drive, '--size', 1024]
    v.customize ['storageattach', :id, '--storagectl', 'SATA Controller', '--port', 1, '--device', 0, '--type', 'hdd', '--medium', second_drive]
  end

  vms.each do |vm|
    config.vm.define vm[0] do |m|
      m.vm.hostname = vm[0]
      m.vm.box = vm[1]
      m.vm.network "private_network", type: "dhcp"
      m.vm.provision "ansible" do |ansible|
        ansible.playbook = "tests/test.yml"
        ansible.groups = { "test" => [ vm[0] ] }
        ansible.verbose = 'vv'
        ansible.sudo = true
      end
    end
  end
end
