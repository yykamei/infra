# frozen_string_literal: true
# -*- coding: utf-8 -*-
# vi: set ft=ruby :

Vagrant.configure('2') do |config|
  nodes = [
    {
      name: 'node1',
      hostname: 'node1.example.com',
      ip: '192.168.4.11',
      memory: 1024
    },
    {
      name: 'node2',
      hostname: 'node2.example.com',
      ip: '192.168.4.12',
      memory: 1024
    }
  ]
  hosts = ['cat <<EOF > /etc/hosts', '127.0.0.1 localhost']
  hosts += nodes.map(&-> (node) { "#{node[:ip]} #{node[:hostname]}" })
  hosts += ['EOF']
  write_hosts = hosts.join("\n")
  nodes.each do |node|
    config.vm.define node[:name] do |node_conf|
      node_conf.vm.box = 'centos/7'
      node_conf.vm.box_check_update = false
      node_conf.vm.hostname = node[:hostname]
      node_conf.vm.provision 'shell', inline: write_hosts
      node_conf.vm.network 'private_network', ip: node[:ip]
      node_conf.vm.provider 'virtualbox' do |vb|
        vb.customize [
          'modifyvm', :id,
          '--memory', node[:memory],
          '--paravirtprovider', 'kvm'
        ]
      end
    end
  end
end
