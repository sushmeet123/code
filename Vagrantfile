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

  config.vm.define "csy2027-v3-32bit" do |csy2027|
  end

  # Give the box a name to make it easily identifiable inside virtualbox
  config.vm.provider :virtualbox do |vb|
      vb.name = "csy2027-vagrant-v3-32bit"
  end
  config.vm.box = "csy2027-v3-32bit"



  # First looks on the V: drive on the local machine for university computers
  # For laptops and any computer without the box image downloads it from the university server
  config.vm.box_url = ["file:///V:/CSY2028/csy2028-v3.box", "http://www.eng.nene.ac.uk/~tom/csy2028/csy2028-v3-32bit.box"]

  # Create a forwarded port mapping which allows access to a specific port
  # within the machine from a port on the host machine. In the example below,
  # accessing "localhost:8080" will access port 80 on the guest machine.
  config.vm.network "forwarded_port", guest: 80, host: 8080, auto_correct: true
  config.vm.network "forwarded_port", guest: 80, host: 80, auto_correct: true
  config.vm.network "forwarded_port", guest: 3306, host: 3307, auto_correct: true

  # Create a private network, which allows host-only access to the machine
  # using a specific IP.
  config.vm.network "private_network", ip: "192.168.56.2"
  config.vm.hostname = "csy2027"

  # Create a public network, which generally matched to bridged network.
  # Bridged networks make the machine appear as another physical device on
  # your network.
  #config.vm.network "public_network"

  # Share an additional folder to the guest VM. The first argument is
  # the path on the host to the actual folder. The second argument is
  # the path on the guest to mount the folder. And the optional third
  # argument is a set of non-required options.
  config.vm.synced_folder "./public_html", "/srv/http", create: true, owner: "vagrant", group: "users", mount_options: ["dmode=777,fmode=777"]
  config.vm.synced_folder "./mysql", "/var/lib/mysql", create: true, owner: "mysql", group: "root"


  # Enable provisioning with a shell script. Additional provisioners such as
  # Puppet, Chef, Ansible, Salt, and Docker are also available. Please see the
  # documentation for more information about their specific syntax and use.

  # Disable mysql
  # Copy the mysql data/config from /var/lib/mysql-default to /var/lib/mysql which is a synced folder
  # Then copy the default web pages into the public_html directory  
  # And finally resart mysql
  config.vm.provision "shell", run: "always", inline: <<-SHELL
    sudo systemctl stop mysqld
    sudo systemctl disable mysqld
    yes n | cp -i /var/www/index.php /srv/http/index.php > /dev/null 2>&1
    yes n | cp -i /var/www/bg.png /srv/http/bg.png > /dev/null 2>&1
    yes n | cp -i /var/www/demo.css /srv/http/demo.css > /dev/null 2>&1
    yes n | cp -irp /var/lib/mysql-default/* /var/lib/mysql/ > /dev/null 2>&1
    sudo echo '' > /var/lib/mysql/mysql-bin.index
    sudo systemctl enable mysqld
    sudo systemctl start mysqld 
  SHELL
end
