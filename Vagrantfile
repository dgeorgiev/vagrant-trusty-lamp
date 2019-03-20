# -*- mode: ruby -*-
# vi: set ft=ruby :

require 'yaml'

current_dir    = File.dirname(File.expand_path(__FILE__))
configs        = YAML.load_file("#{current_dir}/etc/config.yaml")

Vagrant.configure("2") do |config|

  config.vm.box = "ubuntu/trusty64"
  config.vm.hostname = "codealist-lamp"

  config.vm.provision :shell, :path => "bootstrap.sh"
  config.vm.provision :shell, :path => "install/composer.sh"
  
  if configs['configs']['services']['phpmyadmin'] == true
    config.vm.provision :shell, :path => "install/phpmyadmin.sh"
  end
  if configs['configs']['services']['xdebug'] == true
    config.vm.provision :shell, :path => "install/xdebug.sh"
  end
  if configs['configs']['services']['nodejs'] == true
    config.vm.provision :shell, :path => "install/nodejs.sh"
  end
  if configs['configs']['services']['redis'] == true
    config.vm.provision :shell, :path => "install/redis.sh"
  end
  if configs['configs']['services']['memcached'] == true
    config.vm.provision :shell, :path => "install/memcached.sh"
  end
  if configs['configs']['services']['mailutils'] == true
    config.vm.provision :shell, :path => "install/mailutils.sh"
  end
  if configs['configs']['services']['elasticsearch'] == true
    config.vm.provision :shell, :path => "install/elasticsearch.sh"
  end

  config.vm.provision "shell", inline: "sudo mysql -uroot -e \"ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY '${configs[configs][general][mysql_root_pwd]}';\" 2> /dev/null"

  config.vm.network "private_network", ip: configs['configs']['general']['public_ip']

  config.vm.synced_folder "./etc", "/etc/vagrant", type: "nfs"
  config.vm.synced_folder "./sites", "/var/www", type: "nfs"
  config.vm.synced_folder "./scripts", "/scripts", type: "nfs"

  config.vm.provision "file", source: "~/.ssh/id_rsa.pub", destination: "~/.ssh/me.pub"
end
