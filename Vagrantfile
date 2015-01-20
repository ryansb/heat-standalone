# -*- mode: ruby -*-
# vi: set ft=ruby :

# Vagrantfile API/syntax version. Don't touch unless you know what you're doing!
VAGRANTFILE_API_VERSION = "2"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  #config.vm.box = "chef/centos-6.5"

  # Share an additional folder to the guest VM. The first argument is
  # the host path. The second argument is the guest path
  # config.vm.synced_folder "../data", "/vagrant_data"

  config.vm.define "stack" do |stack|
    stack.vm.box = "chef/fedora-20"
    stack.vm.hostname = "stack"
    stack.vm.network "private_network", ip: "192.168.33.3", netmask: "255.255.255.0"
    stack.vm.network "private_network", ip: "10.4.128.3", netmask: "255.255.255.0"
    stack.vm.provider "virtualbox" do |vb|
      vb.memory = 10000
      vb.customize ["modifyvm", :id, "--macaddress1", "080027e94f3d"]
      vb.customize ["modifyvm", :id, "--macaddress2", "080027e94f3f"]
    end
  end
  config.vm.define "heat" do |heat|
    heat.vm.box = "chef/fedora-20"
    heat.vm.hostname = "heat"
    heat.vm.network "private_network", ip: "192.168.33.4", netmask: "255.255.255.0"
    heat.vm.network "private_network", ip: "10.4.128.4", netmask: "255.255.255.0"
    heat.vm.provider "virtualbox" do |vb|
      vb.memory = 5000
      vb.customize ["modifyvm", :id, "--macaddress1", "080027e94f4d"]
      vb.customize ["modifyvm", :id, "--macaddress2", "080027e94f4f"]
    end
  end

  config.vm.provider "virtualbox" do |vb|
    # boot with headless mode
    vb.gui = false
    vb.cpus = 4
  end
  config.vm.provision "ansible" do |ansible|
    ansible.playbook = "devstack_playbook.yml"
    ansible.skip_tags = "post"
    #ansible.tags = "post"
    ansible.groups = {
      "compute" => ["stack"],
      "orchestration" => ["heat"],
      "devstack" => ["stack", "heat"]
    }
  end
end
