# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure(2) do |config|

  config.vm.box = "geerlingguy/centos7"
  config.vm.hostname = 'rundeck.taybiz.local'

  # if !Vagrant.has_plugin?("vagrant-vbguest")
  #   system('vagrant plugin install vagrant-vbguest')
  #   raise('vagrant-vbguest installed. Run command again.')
  # end

  # if !Vagrant.has_plugin?("vagrant-proxyconf")
  #   system('vagrant plugin install vagrant-proxyconf')
  #   raise("vagrant-proxyconf installed. Run command again.")
  # else
  #   config.proxy.http     = "http://192.168.1.250:3128"
  #   config.proxy.https    = "http://192.168.1.250:3128"
  #   config.proxy.no_proxy = "localhost,127.0.0.1"
  # end

  config.vm.provider "virtualbox" do |vb|
     #vb.gui = true
     vb.cpus = "2"
     vb.customize ["modifyvm", :id, "--name", "rundeck.taybiz.local"]
     vb.customize ["modifyvm", :id, "--memory", "1024"]
     #vb.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]
     vb.customize ["modifyvm", :id, "--ioapic", "on"]
  end

  config.vm.synced_folder ".", "/vagrant", :mount_options => ["fmode=666"]

  config.vm.network "private_network", ip: "192.168.33.10"

  config.vm.provision "shell", inline: <<-SHELL
    yum -y install epel-release
    yum -y install ansible git

    #Galaxy dependencies
    ansible-galaxy install -r /vagrant/requirements.yml --ignore-errors

    #run the playbook
    ansible-playbook /vagrant/playbook.yml -i /vagrant/hosts
  SHELL

end
