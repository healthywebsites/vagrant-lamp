require 'socket'

def my_first_private_ipv4
  Socket.ip_address_list.detect{|intf| intf.ipv4_private?}
end

def my_first_public_ipv4
  Socket.ip_address_list.detect{|intf| intf.ipv4? and !intf.ipv4_loopback? and !intf.ipv4_multicast? and !intf.ipv4_private?}
end

Vagrant::Config.run do |config|
  # All Vagrant configuration is done here. For a detailed explanation
  # and listing of configuration options, please view the documentation
  # online.

  # Every Vagrant virtual environment requires a box to build off of.
  config.vm.box = "precise64"
  config.vm.box_url = "http://files.vagrantup.com/precise64.box"

  ip = my_first_public_ipv4.ip_address unless my_first_public_ipv4.nil?

  config.vm.provision :chef_solo do |chef|
    chef.cookbooks_path = "dev/cookbooks"
    chef.add_recipe("vagrant_main")
    chef.json.merge!({
    :mysql => {
      :server_root_password => "root"
    },
    :php => {
      :virtualbox_host_ip => "192.168.0.4"
    }
  })
  end

  config.vm.forward_port(80, 8080)
  config.vm.forward_port(3306, 3306)
end
