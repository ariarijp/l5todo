# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure(2) do |config|
  config.vm.box = "ubuntu/trusty64"
  config.vm.network "private_network", ip: "192.168.33.10"
  config.vm.provision "shell", inline: <<-SHELL
    sudo sed -i s/archive.ubuntu.com/ftp.jaist.ac.jp/ /etc/apt/sources.list
    LC_ALL=en_US.UTF-8 sudo add-apt-repository -y ppa:ondrej/php
    sudo apt-get update
    sudo apt-get -y install apache2 php7.0 php7.0-sqlite3 git curl language-pack-ja
    sudo chown vagrant.vagrant /var/www/html
    echo '<?php phpinfo();?>' > /var/www/html/phpinfo.php
  SHELL
end
