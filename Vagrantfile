# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
  config.vm.box = "debian/stretch64"
  config.vm.network "public_network", bridge: "en0: Wi-Fi (AirPort)"
  config.vm.synced_folder ".", "/vagrant", disabled: true

  config.persistent_storage.enabled = true
  config.persistent_storage.location = "./data.vdi"
  config.persistent_storage.size = 50000
  config.persistent_storage.filesystem = "ext4"
  config.persistent_storage.mountpoint = "/var/lib/tftpboot"
  config.persistent_storage.use_lvm = false

  config.vm.provision "ansible" do |ansible|
    ansible.playbook = "playbook.yml"
  end
end
