# -*- mode: ruby -*-
# vi: set ft=ruby :

# All Vagrant configuration is done below. The "2" in Vagrant.configure
# configures the configuration version (we support older styles for
# backwards compatibility). Please don't change it unless you know what
# you're doing.
Vagrant.configure(2) do |config|
  # The most common configuration options are documented and commented below.
  # For a complete reference, please see the online documentation at
  # https://docs.vagrantup.com.

  # Every Vagrant development environment requires a box. You can search for
  # boxes at https://atlas.hashicorp.com/search.
  config.vm.box = "ubuntu/trusty64"
  config.vm.box_version = "20170818.0.0"

  # Disable automatic box update checking. If you disable this, then
  # boxes will only be checked for updates when the user runs
  # `vagrant box outdated`. This is not recommended.
  # config.vm.box_check_update = false

  # Create a forwarded port mapping which allows access to a specific port
  # within the machine from a port on the host machine. In the example below,
  # accessing "localhost:8080" will access port 80 on the guest machine.
  # config.vm.network "forwarded_port", guest: 80, host: 8080
  config.vm.network "forwarded_port", guest: 80, host: 8000
  config.vm.network "forwarded_port", guest: 8080, host: 8080
  config.vm.network "forwarded_port", guest: 5432, host: 1234

  # Create a private network, which allows host-only access to the machine
  # using a specific IP.
  # config.vm.network "private_network", ip: "192.168.33.10"

  # Create a public network, which generally matched to bridged network.
  # Bridged networks make the machine appear as another physical device on
  # your network.
  # config.vm.network "public_network"

  # Share an additional folder to the guest VM. The first argument is
  # the path on the host to the actual folder. The second argument is
  # the path on the guest to mount the folder. And the optional third
  # argument is a set of non-required options.
  config.vm.synced_folder "~/wfp/sparc2.git", "/home/vagrant/sparc2.git"
  config.vm.synced_folder "~/wfp/geodash-base.git", "/home/vagrant/geodash-base.git"
  #config.vm.synced_folder "~/workspaces/public/geodash.js.git", "/home/vagrant/geodash.js.git"
  config.vm.synced_folder "~/wfp/sparc2-core.js.git", "/home/vagrant/sparc2-core.js.git"
  config.vm.synced_folder "~/wfp/sparc2-plugin-calendar.git", "/home/vagrant/sparc2-plugin-calendar.git"
  config.vm.synced_folder "~/wfp/sparc2-plugin-sidebar.git", "/home/vagrant/sparc2-plugin-sidebar.git"
  #config.vm.synced_folder "~/workspaces/public/sparc2-plugin-welcome.git", "/home/vagrant/sparc2-plugin-welcome.git"
  config.vm.synced_folder "~/wfp/geodash-plugin-map-map.git", "/home/vagrant/geodash-plugin-map-map.git"
  config.vm.synced_folder "~/wfp/geodash-plugin-navbars.git", "/home/vagrant/geodash-plugin-navbars.git"

  # Provider-specific configuration so you can fine-tune various
  # backing providers for Vagrant. These expose provider-specific options.
  # Example for VirtualBox:
  #
  config.vm.provider "virtualbox" do |vb|
  #   # Display the VirtualBox GUI when booting the machine
      vb.gui = true
  #
  #   # Customize the amount of memory on the VM:
      vb.cpus = 4
      #vb.memory = 4096
      vb.memory = 8192
  end
  #
  # View the documentation for the provider you are using for more
  # information on available options.

  # Define a Vagrant Push strategy for pushing to Atlas. Other push strategies
  # such as FTP and Heroku are also available. See the documentation at
  # https://docs.vagrantup.com/v2/push/atlas.html for more information.
  # config.push.define "atlas" do |push|
  #   push.app = "YOUR_ATLAS_USERNAME/YOUR_APPLICATION_NAME"
  # end

  # Enable provisioning with a shell script. Additional provisioners such as
  # Puppet, Chef, Ansible, Salt, and Docker are also available. Please see the
  # documentation for more information about their specific syntax and use.
  # config.vm.provision "shell", inline: <<-SHELL
  #   sudo apt-get update
  #   sudo apt-get install -y apache2
  # SHELL

  config.vm.provision "ansible" do |ansible|
    ansible.playbook = "vagrant.yml"
    ansible.host_key_checking = false
    ansible.verbose = "v"
    ansible.raw_arguments = [
      "--extra-vars=@extra_vars/vagrant.yml",
    ]
  end

end
