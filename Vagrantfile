# -*- mode: ruby -*-
# vi: set ft=ruby :
VAGRANTFILE_API_VERSION = "2"
VAGRANT_VM_PROVIDER = "virtualbox"
# All Vagrant configuration is done below. The "2" in Vagrant.configure
# configures the configuration version (we support older styles for
# backwards compatibility). Please don't change it unless you know what
# you're doing.
Vagrant.configure(VAGRANTFILE_API_VERSION) do | config |
  VM_COUNT = 2
  # The most common configuration options are documented and commented below.
  # For a complete reference, please see the online documentation at
  # https://docs.vagrantup.com.

  # Every Vagrant development environment requires a box. You can search for
  # boxes at https://vagrantcloud.com/search.
  config.vm.provider :virtualbox do | vb |
    vb.customize ["modifyvm", :id, "__memory", "1024"]
    vb.customize ["modifyvm", :id, "__natnet1", "10.3/16"]
    vb.gui = true
  end

  (1..VM_COUNT).each do | machine_id |
    config.vm.define "expert-devops#{machine_id}" do | server |
      server.vm.hostname = "expert-devops#{machine_id}"
      server.vm.box = "ubuntu/bionic64"
      server.vm.network :private_network, ip: "192.168.56.10#{machine_id}"
      server.vm.provider :virtualbox do | vb |
        vb.name = "expert-devops#{machine_id}"
      end

      if machine_id == VM_COUNT
        config.vm.provision "ansible" do | ansible |
          ansible.playbook = 'playbook.yml'
          ansible.become = true
          ansible.limit = 'all'
          ansible.groups = {
            "group1" => ["expert-devops[1:2]"]
          }
        end
      end
    end

    config.vm.provision "shell" do | s |
      s.inline = "apt install -y ansible"
    end
    config.vm.box_check_update = false
  end
end
