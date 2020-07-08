Vagrant.configure("2") do |config|
  
    config.vm.hostname    = "ubuntu-lab"
    config.vm.box         = "bento/ubuntu-20.04"
    config.vm.box_version = "202005.21.0"

    config.vm.network "private_network", ip: "192.168.50.100",
        virtualbox__intnet: true

    config.vm.network "forwarded_port", guest: 22, host: 2222
    config.vm.network "forwarded_port", guest: 8080, host: 8080

    config.vm.provider "virtualbox" do |vm_item_vb|

        vm_item_vb.cpus          = "2"
        vm_item_vb.disksize.size = "40GB"
        vm_item_vb.memory        = "6144"
    end

    config.vm.synced_folder "C:\\Source Codes\\", "/home/vagrant/Source Codes/"

    config.vm.provision "ansible" do |ansible|
        ansible.compatibility_mode = "2.0"
        ansible.playbook = "playbooks/configureVM.yml"
        ansible.become = true
        ansible.groups = {
            "setup_vm" => ["ubuntu-lab"]
        }
    end

end
