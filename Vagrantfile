# -*- mode: ruby -*-
# vi: set ft=ruby :

VAGRANTFILE_API_VERSION = "2"

hosts = {
  "zk1" => {
    ip: "10.77.0.2",
    ram: "256",
    cpus: "1",
  },
  "master1" => {
    ip: "10.77.1.2",
    ram: "1024",
    cpus: "2"
  }
}

Vagrant.configure(VAGRANTFILE_API_VERSION) do |vagrant_config|
  hosts.each do |name, params|
    vagrant_config.vm.define name do |config|
      config.vm.box = "precise32"
      config.vm.box_url = "http://files.vagrantup.com/precise32.box"
      config.vm.network :private_network, ip: params[:ip]

      config.vm.provider :virtualbox do |vb|
        vb.customize ["modifyvm", :id, "--memory", params[:ram]]
        vb.customize ["modifyvm", :id, "--cpus", params[:cpus]]
        vb.customize ["modifyvm", :id, "--ioapic", "on"]
      end

      config.vm.provision "ansible" do |ansible|
        # ansible.verbose = "vvv"
        ansible.inventory_path = "provisionning/hosts"
        ansible.playbook = "provisionning/site.yml"
      end
    end
  end
end
