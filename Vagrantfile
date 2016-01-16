# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure(2) do |config|
  config.vm.box = "ubuntu/trusty64"
  config.vm.network "private_network", ip: "192.168.33.10"

  # day 1
  config.vm.provision "shell", inline: <<-SHELL
    sudo sed -i s/archive.ubuntu.com/ftp.jaist.ac.jp/ /etc/apt/sources.list
    LC_ALL=en_US.UTF-8 sudo add-apt-repository -y ppa:ondrej/php
    sudo apt-get update
    sudo apt-get -y install apache2 php7.0 php7.0-sqlite3 git curl language-pack-ja
    sudo chown vagrant.vagrant /var/www/html
  SHELL
  config.vm.provision "shell", privileged: false, inline: <<-SHELL
    echo '<?php phpinfo();?>' > /var/www/html/phpinfo.php
  SHELL

  # day 2
  config.vm.provision "shell", privileged: false, inline: <<-SHELL
    curl -sS https://getcomposer.org/installer | php
    sudo mv composer.phar /usr/local/bin/composer
    composer global require "laravel/installer"
    echo 'export PATH=~/.composer/vendor/bin:$PATH' >> .bashrc
    sudo chown vagrant.vagrant /var/www
  SHELL
  config.vm.provision "shell", privileged: false, env: {PATH: '~/.composer/vendor/bin:$PATH'}, inline: <<-SHELL
    cd /var/www && laravel new todo
    chmod -R 777 /var/www/todo/storage
    chmod -R 777 /var/www/todo/bootstrap/cache
    rm -rf /var/www/html
    ln -s /var/www/todo/public /var/www/html
  SHELL

  # day 3
  config.vm.provision "shell", privileged: false, inline: <<-SHELL
    sudo apt-get -y install php7.0-curl
    sudo a2enmod rewrite
    sudo cp /vagrant/default.conf /etc/apache2/sites-enabled/000-default.conf
    sudo service apache2 restart
    cd /var/www/todo && composer require "laravel/socialite:2.0.*"
  SHELL
end
