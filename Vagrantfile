# -*- mode: ruby -*-
# vi: set ft=ruby :

cookbook_testers = {
  :debian606    => {
    :hostname   => "debian605",
    :box_url    => "~/virtualbox/debian606.box",
    :ipaddress  => "192.168.56.6",
    :neo4j_port => 6006,
    :mongo_port => 7006,
    :http_port  => 8006,
    :https_port => 9006,
    :elast_port => 9206
  }
}

Vagrant::Config.run do |config|
  cookbook_testers.each_pair do |name, options|
    config.vm.define name do |config|
      #config.vm.boot_mode = :gui
      config.vm.box_url = options[:box_url]
      config.vm.customize ["modifyvm", :id, "--memory", 2048, "--cpus", "2"]
      config.vm.share_folder("v-root", "/vagrant", ".", :disabled => true)
      config.vm.box = options[:hostname]
      config.vm.boot_mode = :headless
      config.vm.host_name = options[:hostname]
      #config.vm.network :bridged
      config.vm.network :hostonly, options[:ipaddress]
      config.vm.forward_port 28017, options[:mongo_port]
      config.vm.forward_port 80, options[:http_port]
      config.vm.forward_port 443, options[:https_port]
      config.vm.forward_port 7474, options[:neo4j_port]
      config.vm.forward_port 9200, options[:elast_port]
      config.vm.provision :chef_solo do |chef|
        chef.provisioning_path = "/tmp/vagrant-chef"
        chef.cookbooks_path = "~/Development/cookbooks"
        chef.roles_path = "~/Development/vagrant/roles"
        chef.data_bags_path = "~/Development/vagrant/data_bags"
	chef.run_list = [
	   "role[base]"
	]
        #chef.add_recipe("users::sysadmins")
        #chef.add_recipe("toolstack")
        chef.json = {
          #'GRAYLOG2' => {
          #  'MONGO_HOST' => "0.0.0.0"
          #}
          #'elasticsearch' => {
          #  'server_version' => '0.19.3',
          #  'jetty' => {
          #    'admin' => 'password',
          #    'user'  => 'password'
          #},
          #'java' => {
          #  'install_flavor' => 'oracle'
          #}
        }
        #chef.log_level = :debug
      end
    end
  end
end
