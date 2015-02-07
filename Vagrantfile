# -*- mode: ruby -*-
# vi: set ft=ruby :

# Vagrantfile API/syntax version. Don't touch unless you know what you're doing!
VAGRANTFILE_API_VERSION = "2"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |vagrant|
  vagrant.vm.box = "ubuntu/trusty64"
  vagrant.vm.box_url = "https://cloud-images.ubuntu.com/vagrant/trusty/current/trusty-server-cloudimg-amd64-vagrant-disk1.box"

  # Machine Configuration
  # Docker Hosts
  (1..1).each do |i|
    vagrant.vm.define "dockerhost0#{i}" do |config|
      config.vm.hostname = "dockerhost0#{i}"
      config.vm.network "private_network", ip: "10.100.199.20#{i}"
      config.vm.provision :hosts
      config.vm.provision :ansible do |ansible|
        ansible.playbook = 'ansible/dockerhost.yml'
        ansible.groups   = {'dockerhosts' => ["dockerhost0#{i}"], 'local' => ['localhost']}
        ansible.raw_arguments = '--timeout=30'
        ansible.host_key_checking = false
      end
    end
  end

end
