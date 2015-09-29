# -*- mode: ruby -*-
# vi: set ft=ruby :

unless Vagrant.has_plugin?("vagrant-docker-compose")
  system("vagrant plugin install vagrant-docker-compose")
  puts "Dependencies installed, please try the command again."
  exit
end

Vagrant.configure("2") do |config|
  config.vm.box = "ubuntu/trusty64"

  config.vm.network "private_network", ip: "192.168.50.5"

  config.vm.network(:forwarded_port, guest: 27017, host: 27018)

  config.nfs.map_uid = Process.uid
  config.nfs.map_gid = Process.gid

  config.vm.synced_folder "./data", "/vagrant-mongo", type: 'nfs'
  config.bindfs.bind_folder "/vagrant-mongo", "/data/db", create_as_user: true, perms: "u=rwx:g=rwx:o=rwx",  :'chown-ignore' => true, :'chgrp-ignore' => true, :'chmod-ignore' => true, after: :provision, o: 'nonempty'

  config.vm.provision :docker
  config.vm.provision :docker_compose, yml: "/vagrant/docker-compose.yml", rebuild: true, project_name: "myproject", run: "always"
end