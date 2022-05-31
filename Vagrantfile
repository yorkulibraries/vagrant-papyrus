rails_env = ENV['RAILS_ENV'] || 'development'

Vagrant.configure("2") do |config|
  config.vm.hostname = "vagrant"
  config.vm.box = "bento/ubuntu-20.04"
  config.vm.define "vagrant-papyrus"
  config.vm.provider "virtualbox" do |vb|
    # Customize the amount of memory on the VM:
    vb.memory = "1024"
    vb.name = "vagrant-papyrus"
  end
  config.vm.network :private_network, ip: "192.168.168.168"
  config.vm.provision "ansible" do |ansible| 
    ansible.playbook="vagrant_provision.yml" 
    ansible.extra_vars = {
      rails_env: rails_env
    } 
  end 

  config.vm.synced_folder "papyrus", "/vagrant/papyrus", mount_options: ["dmode=775,fmode=664"]
end
