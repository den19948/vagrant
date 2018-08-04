# -*- mode: ruby -*-
# vi: set ft=ruby :
 

Vagrant.configure(2) do |config|
config.vm.define "Apache2" do |web_config|
 web_config.vm.box = "ubuntu/trusty64"

   web_config.vm.network "private_network", ip: "192.168.33.10"

  web_config.vm.provision "shell", inline: <<-SHELL
    # sudo apt-get update
     sudo apt-get install -y apache2
   SHELL
end
config.vm.define :db do |db_config|
db_config.vm.box = "ubuntu/trusty64"
db_config.vm.network "private_network", ip: "192.168.33.21"

db_config.vm.provision "shell", inline: <<-SHELL
sudo apt-get install mysql
SHELL
  end
end
