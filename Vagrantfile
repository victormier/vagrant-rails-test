# -*- mode: ruby -*-
# vi: set ft=ruby :

# Based on https://gorails.com/guides/using-vagrant-for-rails-development
# run this before provisioning:
# > vagrant plugin install vagrant-vbguest
# > vagrant plugin install vagrant-librarian-chef-nochef

VAGRANTFILE_API_VERSION = "2"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  # Use Ubuntu 14.04 Trusty Tahr 64-bit as our operating system
  config.vm.box = "ubuntu/trusty64"

  # Configurate the virtual machine to use 2GB of RAM
  config.vm.provider :virtualbox do |vb|
    vb.customize ["modifyvm", :id, "--memory", "2048"]
  end

  # Set up private network (specifically for nfs on virtualbox)
  config.vm.network "private_network", type: "dhcp"

  # Set synced directory using nfs which is more performant than the default solution
  config.vm.synced_folder ".", "/vagrant", type: "nfs"

  # Forward the Rails server default port to the host
  config.vm.network :forwarded_port, guest: 3000, host: 4000

  # Fix no-tty error
  config.vm.provision "fix-no-tty", type: "shell" do |s|
    s.privileged = false
    s.inline = "sudo sed -i '/tty/!s/mesg n/tty -s \\&\\& mesg n/' /root/.profile"
  end

  # Use Chef Solo to provision our virtual machine
  config.vm.provision :chef_solo do |chef|
    chef.cookbooks_path = ["cookbooks", "site-cookbooks"]

    chef.add_recipe "apt"
    chef.add_recipe "nodejs"
    chef.add_recipe "ruby_build"
    chef.add_recipe "rbenv::user"
    chef.add_recipe "rbenv::vagrant"
    chef.add_recipe "vim"
    chef.add_recipe "postgresql::server"
    chef.add_recipe "postgresql::client"

    # Install Ruby 2.2.1 and Bundler
    # Set an empty root password for MySQL to make things simple
    chef.json = {
      rbenv: {
        user_installs: [{
          user: 'vagrant',
          rubies: ["2.2.1"],
          global: "2.2.1",
          gems: {
            "2.2.1" => [
              { name: "bundler" }
            ]
          }
        }]
      },
      postgresql: {
        password: {
          postgres: ''
        }
      }
    }
  end
end
