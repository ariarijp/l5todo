# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure(2) do |config|
  config.vm.box = "ubuntu/trusty64"
  config.vm.network "private_network", ip: "192.168.33.10"

  config.vm.synced_folder "./todo/bootstrap/cache", "/vagrant/todo/bootstrap/cache",
    owner: "vagrant",
    group: "www-data",
    mount_options: ["dmode=777,fmode=666"]

  config.vm.synced_folder "./todo/storage", "/vagrant/todo/storage",
    owner: "vagrant", 
    group: "www-data",
    mount_options: ["dmode=777,fmode=666"]

  config.vm.provision "shell", privileged: false, inline: <<-SHELL
    sudo sed -i s/archive.ubuntu.com/ftp.jaist.ac.jp/ /etc/apt/sources.list
    LC_ALL=en_US.UTF-8 sudo add-apt-repository -y ppa:ondrej/php
    sudo apt-get update
    sudo apt-get -y install apache2 php7.0 php7.0-sqlite3 php7.0-curl git curl language-pack-ja
    sudo chown vagrant.vagrant /var/www/html
    sudo a2enmod rewrite
    sudo cp /vagrant/default.conf /etc/apache2/sites-enabled/000-default.conf
    sudo service apache2 restart
    curl -sS https://getcomposer.org/installer | php
    sudo mv composer.phar /usr/local/bin/composer
    composer global require "laravel/installer"
    echo 'export PATH=~/.composer/vendor/bin:$PATH' >> .bashrc
    cd /vagrant/todo; composer install
  SHELL
end
