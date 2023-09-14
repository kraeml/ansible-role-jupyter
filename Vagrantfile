# -*- mode: ruby -*-
# vi: set ft=ruby :

# ansible_local requires version >= 1.8.4 to work stably
Vagrant.require_version '>= 1.8.4'

# All Vagrant configuration is done below. The "2" in Vagrant.configure
# configures the configuration version (we support older styles for
# backwards compatibility). Please don't change it unless you know what
# you're doing.
Vagrant.configure("2") do |config|
  # Taken from https://github.com/gantsign/development-environment/blob/master/Vagrantfile
  required_plugins = %w[vagrant-readenv]
  plugins_to_install = required_plugins.reject { |plugin| Vagrant.has_plugin? plugin }
  unless plugins_to_install.empty?
    puts "Installing plugins: #{plugins_to_install.join(' ')}"
    if system "vagrant plugin install #{plugins_to_install.join(' ')}"
      exec "vagrant #{ARGV.join(' ')}"
    else
      abort 'Installation of one or more plugins has failed. Aborting.'
    end
  end
  config.env.enable # Enable vagrant-env(.env)
  # Setup ENV with default values
  RDF_HOSTNAME = ENV["RDF_HOSTNAME"] || "ansible-jupyter"
  GUEST_IP = ENV["GUEST_IP"] || "192.168.33.20"
  # The most common configuration options are documented and commented below.
  # For a complete reference, please see the online documentation at
  # https://docs.vagrantup.com.

  # Every Vagrant development environment requires a box. You can search for
  # boxes at https://vagrantcloud.com/search.
  config.vm.box = "rdf/ubuntu2004-de"

  # Disable automatic box update checking. If you disable this, then
  # boxes will only be checked for updates when the user runs
  # `vagrant box outdated`. This is not recommended.
  # config.vm.box_check_update = false

  # Create a forwarded port mapping which allows access to a specific port
  # within the machine from a port on the host machine. In the example below,
  # accessing "localhost:8080" will access port 80 on the guest machine.
  # NOTE: This will enable public access to the opened port
  # config.vm.network "forwarded_port", guest: 80, host: 8080

  # Create a forwarded port mapping which allows access to a specific port
  # within the machine from a port on the host machine and only allow access
  # via 127.0.0.1 to disable public access
  # config.vm.network "forwarded_port", guest: 80, host: 8080, host_ip: "127.0.0.1"

  # Create a private network, which allows host-only access to the machine
  # using a specific IP.
  config.vm.network "private_network", ip: GUEST_IP
  config.vm.hostname = RDF_HOSTNAME

  # Create a public network, which generally matched to bridged network.
  # Bridged networks make the machine appear as another physical device on
  # your network.
  # config.vm.network "public_network"

  # Share an additional folder to the guest VM. The first argument is
  # the path on the host to the actual folder. The second argument is
  # the path on the guest to mount the folder. And the optional third
  # argument is a set of non-required options.
  # config.vm.synced_folder "../data", "/vagrant_data"
  config.vm.synced_folder "./tests", "/home/vagrant/jupyter-rolle"
  config.vm.synced_folder "./", "/home/vagrant/jupyter-rolle/roles/jupyter"

  # config.ssh.forward_agent = true
  # config.ssh.forward_x11 = true


  # Provider-specific configuration so you can fine-tune various
  # backing providers for Vagrant. These expose provider-specific options.
  # Example for VirtualBox:
  #
  config.vm.provider "virtualbox" do |vb|
    # Display the VirtualBox GUI when booting the machine
    vb.gui = false

    # Customize the amount of memory on the VM:
    vb.memory = "2048"

    # Customize CPU settings
    vb.cpus = 2

    # Customize graphics settings
    # vb.customize ['modifyvm', :id, '--vram', '32']
    # vb.customize ['modifyvm', :id, '--accelerate3d', 'off']
    # vb.customize ['modifyvm', :id, '--graphicscontroller', 'vmsvga']

    # Enable sound
    #vb.customize ['modifyvm', :id, '--audio', 'default_vb_audio', '--audiocontroller', 'default_vb_audiocontroler']

    # vb.customize ["modifyvm", :id, "--natdnshostresolver1", "off"]
    vb.customize ["modifyvm", :id, "--ioapic", "on"]
    # Enable host desktop integration
    vb.customize ["modifyvm", :id, "--clipboard", "bidirectional"]
    vb.customize ["modifyvm", :id, "--draganddrop", "bidirectional"]
    vb.customize ["modifyvm", :id, "--usbehci", "on"]
  end
  #
  # View the documentation for the provider you are using for more
  # information on available options.

  # Enable provisioning with a shell script. Additional provisioners such as
  # Ansible, Chef, Docker, Puppet and Salt are also available. Please see the
  # documentation for more information about their specific syntax and use.
  config.vm.provision "shell", inline: <<-SHELL
    # apt-get update
    cd jupyter-rolle/
    ansible-galaxy role install --force --roles-path ./roles --role-file requirements.yml
  SHELL
end
