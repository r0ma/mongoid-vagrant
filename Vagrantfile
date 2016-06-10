# encoding: utf-8
# This file originally created at http://rove.io/482027957fc9b410846d981bbac4401c

# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|

  config.vm.box = "opscode-ubuntu-12.04_chef-11.4.0"
  config.vm.box_url = "https://opscode-vm-bento.s3.amazonaws.com/vagrant/opscode_ubuntu-12.04_chef-11.4.0.box"
  config.vm.network :private_network, ip: "192.168.56.101"

  config.ssh.forward_agent = true
  config.vagrant.host = :detect


  config.vm.provider :virtualbox do |virtualbox|
    virtualbox.customize ["modifyvm", :id, "--rtcuseutc", "on"] #fix grub hang
    virtualbox.customize ["modifyvm", :id, "--name", "karpatybox12"]
    virtualbox.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]
    virtualbox.customize ["modifyvm", :id, "--memory", "512"]
    virtualbox.customize ["setextradata", :id, "--VBoxInternal2/SharedFoldersEnableSymlinksCreate/v-root", "1"]
  end

  config.vm.provision :chef_solo do |chef|
    chef.cookbooks_path = ["cookbooks"]
    chef.add_recipe :apt
    chef.add_recipe 'git'
    chef.add_recipe 'mongodb::default'

    chef.json = {
      :git     => {
        :prefix => "/usr/local"
      },
      :mongodb => {
        :dbpath  => "/var/lib/mongodb",
        :logpath => "/var/log/mongodb",
        :port    => "27017",
        :default_init_name => 'mongod',
        :install_method => '10gen',
        :config  => {
            :bind_ip => "192.168.56.101"
        },
        :package_version => '2.6.3'
      }
    }
  end
end
