# -*- mode: ruby -*-
# vi: set ft=ruby :

BOX_NAME = "debian-wheezy-64"
BOX_URI = "http://basebox.libera.cc/debian-wheezy-64.box"
VBOX_VERSION = "4.3.8"
VAGRANTFILE_API_VERSION = "2"
DOMAIN = "cluster.local"
HOSTS = [
  { :host => "node1-debian", :ip => "192.168.42.11"},
  { :host => "node2-debian", :ip => "192.168.42.12"},
  { :host => "node3-debian", :ip => "192.168.42.13"}
]

LAST_HOSTNAME=HOSTS[-1][:host]

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  config.landrush.enable
  config.landrush.tld = DOMAIN

  HOSTS.each do | h |

    config.vm.define h[:host] do |machine|

      machine.vm.box = BOX_NAME
      machine.vm.box_url = BOX_URI

      machine.vm.hostname = "%s.%s" % [h[:host], config.landrush.tld]
      machine.vm.network :private_network, ip: h[:ip]

      machine.vm.provider "virtualbox" do |vb|
        vb.name = machine.vm.hostname
        vb.memory = 2048
        vb.cpus = 1
        vb.customize ['modifyvm', :id, '--ioapic', 'on', '--cpuexecutioncap', '50']
      end

      if h[:host] == LAST_HOSTNAME
        machine.vm.provision :ansible do |ansible|
          ansible.playbook = "site.yml"
          ansible.sudo = true
          ansible.inventory_path = "./devel/inventory"
          ansible.limit = 'all'
        end
      end

    end

  end

end
