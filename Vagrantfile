Vagrant.configure("2") do |config|
  config.vm.box = "bento/ubuntu-24.04"
  config.vm.box_version = "202510.26.0"
  config.vm.synced_folder ".", "/vagrant", disabled: true

  config.vm.define "vm1" do |vm1|
    vm1.vm.hostname = "vm1"
    vm1.vm.network "private_network", ip: "192.168.56.11"
    vm1.vm.network "forwarded_port", guest: 8080, host: 8081, host_ip: "127.0.0.1"

    vm1.vm.provider "virtualbox" do |vb|
      vb.name = "SJSU-VM1"
      vb.memory = 1024
      vb.cpus = 1
    end
  end

  config.vm.define "vm2" do |vm2|
    vm2.vm.hostname = "vm2"
    vm2.vm.network "private_network", ip: "192.168.56.12"
    vm2.vm.network "forwarded_port", guest: 8080, host: 8082, host_ip: "127.0.0.1"

    vm2.vm.provider "virtualbox" do |vb|
      vb.name = "SJSU-VM2"
      vb.memory = 1024
      vb.cpus = 1
    end
  end
end
