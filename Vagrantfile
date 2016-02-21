# -*- mode: ruby -*-
# vi: set ft=ruby :

# Vagrantfile API/syntax version. Don't touch unless you know what you're doing!
VAGRANTFILE_API_VERSION = "2"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|

  BOX = "ubuntu/wily64"

  # create management node
  config.vm.define :mgmt do |mgmt_config|
    mgmt_config.vm.box = "#{BOX}"

    # time in seconds that Vagrant will wait for the machine to boot
    mgmt_config.vm.boot_timeout = 300
    # disable automatic box update checking
    mgmt_config.vm.box_check_update = false

    # NFS requires a host-only network to be created
    mgmt_config.vm.network :private_network, ip: "10.0.15.10"

    mgmt_config.vm.provider "virtualbox" do |vb|
      # the hostname the machine should have
      vb.name = "mgmt"

      vb.customize ["modifyvm", :id, "--hwvirtex", "on"]
      vb.customize ["modifyvm", :id, "--cpus", "1"]
      vb.customize ["modifyvm", :id, "--memory", "256"]
      vb.customize ["modifyvm", :id, "--audio", "none"]
      vb.customize ["modifyvm", :id, "--nictype1", "virtio"]
      vb.customize ["modifyvm", :id, "--nictype2", "virtio"]
    end
  end

  # create load balancer node
  config.vm.define :lb do |lb_config|
    lb_config.vm.box = "#{BOX}"

    # time in seconds that Vagrant will wait for the machine to boot
    lb_config.vm.boot_timeout = 300
    # disable automatic box update checking
    lb_config.vm.box_check_update = false

    # NFS requires a host-only network to be created
    lb_config.vm.network :private_network, ip: "10.0.15.11"
    lb_config.vm.network "forwarded_port", guest: 80, host: 8080

    lb_config.vm.provider "virtualbox" do |vb|
      # the hostname the machine should have
      vb.name = "lb"

      vb.customize ["modifyvm", :id, "--hwvirtex", "on"]
      vb.customize ["modifyvm", :id, "--cpus", "1"]
      vb.customize ["modifyvm", :id, "--memory", "256"]
      vb.customize ["modifyvm", :id, "--audio", "none"]
      vb.customize ["modifyvm", :id, "--nictype1", "virtio"]
      vb.customize ["modifyvm", :id, "--nictype2", "virtio"]
    end
  end

  # create some web servers
  (1..4).each do |i|
    config.vm.define "web#{i}" do |node|
      node.vm.box = "#{BOX}"

      # time in seconds that Vagrant will wait for the machine to boot
      node.vm.boot_timeout = 300
      # disable automatic box update checking
      node.vm.box_check_update = false

      # NFS requires a host-only network to be created
      node.vm.network :private_network, ip: "10.0.15.2#{i}"
      node.vm.network "forwarded_port", guest: 80, host: "808#{i}"

      node.vm.provider "virtualbox" do |vb|
        # the hostname the machine should have
        vb.name = "web#{i}"

        vb.customize ["modifyvm", :id, "--hwvirtex", "on"]
        vb.customize ["modifyvm", :id, "--cpus", "1"]
        vb.customize ["modifyvm", :id, "--memory", "256"]
        vb.customize ["modifyvm", :id, "--audio", "none"]
        vb.customize ["modifyvm", :id, "--nictype1", "virtio"]
        vb.customize ["modifyvm", :id, "--nictype2", "virtio"]
      end
    end
  end
end
