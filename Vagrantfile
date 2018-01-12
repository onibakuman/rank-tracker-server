# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|

  nodes       = (ENV['NODES'] || 4).to_i
  node_size   = (ENV['SIZE']  || 512).to_i
  bucket_size = (node_size * 0.7).to_i # 80% is recommended by couchbase, 70% to be safe.

  config.vm.box     = 'precise64'
  config.vm.box_url = 'http://files.vagrantup.com/precise64.box'

  config.vm.provider :virtualbox do |vb|
    vb.customize ['modifyvm', :id, '--memory', node_size]
  end

  (nodes - 1).times do |i|
    ni = i + 1
    config.vm.define "node#{ni}" do |couch|
      couch.vm.provision :shell, path: 'bootstrap-couchbase', args: bucket_size.to_s
      couch.vm.network   :private_network, ip: "192.168.23.10#{ni}"
    end
  end

  config.vm.define :master do |couch|
    couch.vm.provision :shell, path: 'bootstrap-cluster', args: "#{nodes - 1} #{bucket_size}"
    couch.vm.network   :private_network, ip: '192.168.23.100'
    couch.vm.network   :forwarded_port, guest: 8091,  host: 8091
    couch.vm.network   :forwarded_port, guest: 8092,  host: 8092
    couch.vm.network   :forwarded_port, guest: 11211, host: 11211
    couch.vm.network   :forwarded_port, guest: 11210, host: 11210
  end

end
