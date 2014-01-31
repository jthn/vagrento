# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant::Config.run do |config|
  # All Vagrant configuration is done here. The most common configuration
  # options are documented and commented below. For a complete reference,
  # please see the online documentation at vagrantup.com.

  # Every Vagrant virtual environment requires a box to build off of.
  config.vm.box = 'precise64'

  # The url from where the 'config.vm.box' box will be fetched if it
  # doesn't already exist on the user's system.
  config.vm.box_url = 'http://files.vagrantup.com/precise64.box'

  # These ports are used to avoid conflicts with local services.
  config.vm.forward_port 80, 4080
  config.vm.forward_port 3306, 23306

  config.vm.network :hostonly, "192.168.50.11"

  # This is to ensure Magento can write it's generated data to our media and var directories.
  config.vm.share_folder("v-root", "/vagrant", ".", :extra => 'dmode=777,fmode=777')

  config.vm.customize [
    "setextradata", :id,
    "VBoxInternal2/SharedFoldersEnableSymlinksCreate/v-root", "1",
  ]

  config.vm.customize [
    "modifyvm", :id,
    "--memory", "1024",
  ]

  config.vm.provision :chef_solo do |chef|
    chef.roles_path = 'vagrant-lamp/chef/roles'
    chef.cookbooks_path = 'vagrant-lamp/chef/cookbooks'
    chef.add_role 'magento_dev'
    chef.json = { 'vagrant_magento' => {
      'phpinfo_enabled' => true,
      'mage_check_enabled' => true,
      'sample_data' => { 'install' => true },
      'config' => { 'install' => true }
      }
    }
    # Chef Logging
    #chef.log_level = :debug
  end
end
