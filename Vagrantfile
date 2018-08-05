# -*- mode: ruby -*-
# vi: set ft=ruby :
 

Vagrant.configure(2) do |config|
config.vm.define "Apache2" do |web_config|
 web_config.vm.box = "ubuntu/trusty64"

   web_config.vm.network "private_network", ip: "192.168.33.10"

  web_config.vm.provision "shell", inline: <<-SHELL
	sudo apt-get update
	sudo apt-get install -y apache2 libapache2-mod-fastcgi apache2-mpm-worker
	sudo service apache2 restart
	sudo add-apt-repository ppa:ondrej/php
	sudo apt-get update
	sudo apt-get install -y python-software-properties software-properties-common
	sudo apt-get install -y php php-mysql
	sudo apt-get install -y mysql-client
	mkdir wp
	cd wp/
	wget https://ru.wordpress.org/wordpress-4.9.8-ru_RU.tar.gz
	tar -xzf wordpress-4.9.8-ru_RU.tar.gz -C /var/www/html/
	sudo rm /var/www/html/index.html
	sudo cp -R /var/www/html/wordpress/* /var/www/html/
	sudo cp /var/www/html/wp-config-sample.php /var/www/html/wp-config.php
	sudo sed -i 's/'localhost'/'192.168.33.21'/g' /var/www/html/wp-config.php
	sudo sed -i 's/'username_here'/'wpuser'/g' /var/www/html/wp-config.php
	sudo sed -i 's/'password_here'/'q1w2e3r4'/g' /var/www/html/wp-config.php
	sudo sed -i 's/'database_name_here'/'wpdb'/g' /var/www/html/wp-config.php
	sudo service apache2 restart 
   SHELL
end
config.vm.define :db do |db_config|
db_config.vm.box = "ubuntu/trusty64"
db_config.vm.network "private_network", ip: "192.168.33.21"

db_config.vm.provision "shell", inline: <<-SHELL
sudo apt-get update
# Setting MySQL (username: root) ~ (password: password)
sudo debconf-set-selections <<< 'mysql-server-5.5 mysql-server/root_password password password'
sudo debconf-set-selections <<< 'mysql-server-5.5 mysql-server/root_password_again password password'

# Installing packages
apt-get install -y mysql-server mysql-client

# Setup database
#mysql -uroot -ppassword -e "CREATE DATABASE IF NOT EXISTS $APP_DATABASE_NAME;"
mysql -uroot -ppassword -e "GRANT ALL PRIVILEGES ON *.* TO 'root'@'%' IDENTIFIED BY 'password';"
mysql -uroot -ppassword -e "GRANT ALL PRIVILEGES ON *.* TO 'root'@'localhost' IDENTIFIED BY 'password';"
mysql -uroot -ppassword -e "CREATE DATABASE wpdb;"
mysql -uroot -ppassword -e "GRANT ALL PRIVILEGES ON wpdb.* TO wpuser IDENTIFIED BY 'q1w2e3r4';"
sudo sed -i 's/bind-address/#bind-address/g' /etc/mysql/my.cnf 

sudo service mysql restart
SHELL
  end
end
