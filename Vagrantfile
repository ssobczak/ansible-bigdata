# -*- mode: ruby -*-
# vi: set ft=ruby :

VAGRANTFILE_API_VERSION = "2"

hosts = {
  "zk1" => {ip: "10.77.0.2", ram: "256", cpus: "1"},
  "master1" => {ip: "10.77.1.2", ram: "1024", cpus: "2", port: {guest: 5050, host: 5050}},
  "master2" => {ip: "10.77.1.3", ram: "512", cpus: "1", port: {guest: 5050, host: 5051}}
}

Vagrant.configure(VAGRANTFILE_API_VERSION) do |vagrant_config|
  hosts.each do |name, params|
    vagrant_config.vm.define name do |config|
      config.vm.box = "13.04_amd64"
      config.vm.box_url = "http://goo.gl/ceHWg"

      config.vm.network :private_network, ip: params[:ip]
      config.vm.network :forwarded_port, guest: params[:port][:guest], host: params[:port][:host] if params[:port]

      config.vm.provider :virtualbox do |vb|
        vb.customize ["modifyvm", :id, "--memory", params[:ram]]
        vb.customize ["modifyvm", :id, "--cpus", params[:cpus]]
        vb.customize ["modifyvm", :id, "--ioapic", "on"]
      end

      config.vm.provision "ansible" do |ansible|
        # ansible.verbose = "vvvv"
        ansible.inventory_path = "provisionning/hosts"
        ansible.playbook = "provisionning/site.yml"
      end
    end
  end
end
