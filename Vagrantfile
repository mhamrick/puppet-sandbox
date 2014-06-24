# -*- mode: ruby -*-
# vi: set ft=ruby :

domain = 'example.com'

puppet_nodes = [
  {:hostname => 'puppet',  :ip => '172.16.32.10', :box => 'trusty64',:boxurl=> 'http://cloud-images.ubuntu.com/vagrant/trusty/current/trusty-server-cloudimg-amd64-vagrant-disk1.box',:fwdhost => 8140, :fwdguest => 8140, :ram => 512},
#  {:hostname => 'client1', :ip => '172.16.32.11', :box => 'trusty64',:boxurl=> 'http://cloud-images.ubuntu.com/vagrant/trusty/current/trusty-server-cloudimg-amd64-vagrant-disk1.box'},
  {:hostname => 'client2', :ip => '172.16.32.12', :box => 'centos65',:boxurl => 'http://puppet-vagrant-boxes.puppetlabs.com/centos-65-x64-virtualbox-puppet.box'},
#  {:hostname => 'client3', :ip => '172.16.32.13', :box => 'RedHat'},
#  {:hostname => 'client4', :ip => '172.16.32.14', :box => 'trustyi386'},
]

Vagrant.configure("2") do |config|
  puppet_nodes.each do |node|
    config.vm.define node[:hostname] do |node_config|
      node_config.vm.box = node[:box]
      node_config.vm.box_url = node[:boxurl]
      node_config.vm.hostname = node[:hostname] + '.' + domain
      node_config.vm.network :private_network, ip: node[:ip]

    config.vm.provider "virtualbox" do |v|
      v.gui = false
    end


      if node[:fwdhost]
        node_config.vm.network :forwarded_port, guest: node[:fwdguest], host: node[:fwdhost]
      end

      memory = node[:ram] ? node[:ram] : 256;
      node_config.vm.provider :virtualbox do |vb|
        vb.customize [
          'modifyvm', :id,
          '--name', node[:hostname],
          '--memory', memory.to_s
        ]
      end

      node_config.vm.provision :puppet do |puppet|
        puppet.manifests_path = 'provision/manifests'
        puppet.module_path = 'provision/modules'
      end
    end
  end
end
