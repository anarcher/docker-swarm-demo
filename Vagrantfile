# -*- mode: ruby -*-
# vi: set ft=ruby :

# Vagrantfile API/syntax version. Don't touch unless you know what you're doing!
VAGRANTFILE_API_VERSION = "2"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |vagrant|
  vagrant.vm.box = "ubuntu/trusty64"
  vagrant.vm.box_url = "https://cloud-images.ubuntu.com/vagrant/trusty/current/trusty-server-cloudimg-amd64-vagrant-disk1.box"

  # Swarm Configuration
  # Docker Swarm
  vagrant.vm.define "dockerswarm01" do |config|
    config.vm.hostname = "dockerswarm01"
    config.vm.network "private_network", ip: "10.100.199.200"
    config.vm.network "forwarded_port", guest: 2376, host: 2376
    config.vm.network "forwarded_port", guest: 2375, host: 2375
    config.vm.provision :hosts
    config.vm.provision :ansible do |ansible|
      ansible.playbook = 'ansible/dockerswarm.yml'
      ansible.groups   = {'dockerswarms' => ["dockerswarm01"], 'local' => ['localhost']}
      ansible.raw_arguments = '--timeout=30'
      ansible.host_key_checking = false
    end
  end

  # Machine Configuration
  # Docker Hosts
  (1..1).each do |i|
    vagrant.vm.define "dockerhost0#{i}" do |config|
      config.vm.hostname = "dockerhost0#{i}"
      config.vm.network "private_network", ip: "10.100.199.20#{i}"
      config.vm.provision :hosts do |p|
          p.autoconfigure = true
          p.add_host '10.100.199.200' ,['dockerswarm01']
      end
      config.vm.provision :ansible do |ansible|
        ansible.playbook = 'ansible/dockerhost.yml'
        ansible.groups   = {'dockerhosts' => ["dockerhost0#{i}"], 'local' => ['localhost']}
        ansible.raw_arguments = '--timeout=30'
        ansible.host_key_checking = false
      end
    end
  end

end
