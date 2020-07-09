Vagrant.configure("2") do |config|
  
    config.vm.hostname    = "ubuntu-lab"
    config.vm.box         = "bento/ubuntu-20.04"
    config.vm.box_version = "202005.21.0"
    config.disksize.size  = "40GB"

    config.vm.network "private_network", ip: "192.168.50.100",
        virtualbox__intnet: true

    config.vm.network "forwarded_port", guest: 22, host: 2222
    config.vm.network "forwarded_port", guest: 8080, host: 8080

    config.vm.provider "virtualbox" do |vm_item_vb|
        vm_item_vb.cpus          = "2"
        vm_item_vb.memory        = "6144"
    end

    config.vm.synced_folder ".", "/vagrant", mount_options: ["dmode=700,fmode=700"]
    config.vm.synced_folder "C:\\Source Codes\\", "/home/vagrant/Source Codes/", mount_options: ["dmode=700,fmode=700"]

    config.vm.provision "shell",
        inline: "sudo apt update; \
                 sudo apt install -y python3; \
                 sudo apt-get install -y python3-pip; \ 
                 pip3 install ansible;"

    config.vm.provision "ansible_local" do |ansible|
        ansible.compatibility_mode = "2.0"
        ansible.playbook = "playbooks/configure-virtual-machine.yml"
        ansible.become = true
        ansible.groups = {
            "ubuntu-lab" => ["localhost"]
        }
    end

end
