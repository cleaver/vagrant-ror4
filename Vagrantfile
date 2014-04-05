# encoding: utf-8
# This file originally created at http://rove.io/3d50b88cf078c62c7d3d8a11d02d3fc9

# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|

  config.vm.box = "opscode-ubuntu-12.04_chef-11.4.0"
  config.vm.box_url = "https://opscode-vm-bento.s3.amazonaws.com/vagrant/opscode_ubuntu-12.04_chef-11.4.0.box"
  config.ssh.forward_agent = true
  config.vm.network "private_network", ip: "192.168.33.10"
  # config.vm.synced_folder "./data", "/vagrant_data",  :mount_options => ['dmode=777,fmode=666']
  config.vm.synced_folder "./data", "/vagrant_data", type: "nfs"
  config.vm.provider "virtualbox" do |vb|
    vb.customize ["modifyvm", :id, "--memory", "1024"]
  end

  config.vm.provision :chef_solo do |chef|
    chef.cookbooks_path = ["cookbooks"]
    chef.add_recipe :apt
    chef.add_recipe 'redis'
    # chef.add_recipe 'nginx'
    chef.add_recipe 'ruby_build'
    chef.add_recipe 'rbenv::user'
    chef.add_recipe 'git'
    chef.add_recipe 'postgresql::server'
    chef.add_recipe 'vim'
    chef.json = {
      :redis      => {
        :bind        => "127.0.0.1",
        :port        => "6379",
        :config_path => "/etc/redis/redis.conf",
        :daemonize   => "yes",
        :timeout     => "300",
        :loglevel    => "notice"
      },
      # :nginx      => {
      #   :dir                => "/etc/nginx",
      #   :log_dir            => "/var/log/nginx",
      #   :binary             => "/usr/sbin/nginx",
      #   :user               => "www-data",
      #   :init_style         => "runit",
      #   :pid                => "/var/run/nginx.pid",
      #   :worker_connections => "1024"
      # },
      :rbenv      => {
        :user_installs => [
          {
            :user   => "vagrant",
            :rubies => [
              # "2.0.0-p353",
              "2.1.1"
            ],
            :global => "2.1.1"
          }
        ]
      },
      :git        => {
        :prefix => "/usr/local"
      },
      :postgresql => {
        :config   => {
          :listen_addresses => "*",
          :port             => "5432"
        },
        :pg_hba   => [
          {
            :type   => "local",
            :db     => "postgres",
            :user   => "postgres",
            :addr   => nil,
            :method => "trust"
          },
          {
            :type   => "host",
            :db     => "all",
            :user   => "all",
            :addr   => "0.0.0.0/0",
            :method => "md5"
          },
          {
            :type   => "host",
            :db     => "all",
            :user   => "all",
            :addr   => "::1/0",
            :method => "md5"
          }
        ],
        :password => {
          :postgres => "password"
        }
      },
      :vim        => {
        :extra_packages => [
          "vim-rails"
        ]
      }
    }
  end
end
