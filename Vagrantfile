# -*- mode: ruby -*-
# vi: set ft=ruby :
Vagrant.configure("2") do |config|
  config.vm.box = "generic/centos8"
  config.vm.synced_folder "files", "/vagrant_files"

    config.vm.provision "shell", inline: <<-SHELL
    mkdir -p ~root/.ssh
    cp ~vagrant/.ssh/auth* ~root/.ssh

    sudo cp /vagrant_files/watchlog /etc/sysconfig/watchlog   
    sudo cp /vagrant_files/watchlog.sh /opt/watchlog.sh
    sudo cp /vagrant_files/watchlog.log /var/log/watchlog.log 
    sudo cp /vagrant_files/watchlog.service /usr/lib/systemd/system/watchlog.service  
    sudo cp /vagrant_files/watchlog.timer /usr/lib/systemd/system/watchlog.timer 

    sudo systemctl daemon-reload
    sudo systemctl enable watchlog.timer
    sudo systemctl start watchlog.timer
  SHELL
  #config.vm.provision "file", source: "./watchlog", destination: "/etc/sysconfig/watchlog"

end


