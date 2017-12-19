# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure(2) do |config|
    config.vm.box = "blinkreaction/boot2docker"

    # Network config & shared folders
    config.vm.network "private_network", ip: "192.168.33.95"
    config.vm.synced_folder "~/WebProjects/Magento2", "/var/www/magento2", type: "nfs"
    config.vm.synced_folder "~/.composer/", "/home/docker/.composer", type: "nfs"

    # VM definition
    config.vm.provider "virtualbox" do |vb|
        vb.name = "magento2.dev"
        vb.memory = 2048
        vb.cpus = 2
    end

    # Bring up containers
    config.vm.provision "shell", run: "always", inline: "cd /vagrant && docker-compose up -d 1>&2"

    # Redirect webserver port down 80, etc
    config.vm.provision "shell", run: "always", inline: "/usr/local/sbin/iptables -i eth1 -t nat -A PREROUTING -p tcp --dport 80 -j REDIRECT --to-port 7080 1>&2"
    config.vm.provision "shell", run: "always", inline: "/usr/local/sbin/iptables -i eth1 -t nat -A PREROUTING -p tcp --dport 81 -j REDIRECT --to-port 7081 1>&2"

    # Disable guest additions auto update as it won't work on boot2docker, and slows vm boot down boot
    if Vagrant.has_plugin?("vagrant-vbguest")
        config.vbguest.auto_update = false
    end
end
