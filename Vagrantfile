require 'dotenv/load'

vms = {
    "ubuntu-lab" => {
        :vm_box => "bento/ubuntu-20.04",
        :vm_box_version => "202005.21.0",
        :vm_cpu => ENV['UBUNTU_LAB_CPU'],
        :vm_ram => ENV['UBUNTU_LAB_RAM'],
        :vm_disk_size => ENV['UBUNTU_LAB_DISK_SIZE'],
        :vm_ip => ENV['UBUNTU_LAB_IP']
    }
}

Vagrant.configure("2") do |config|

    vms.each do | vm_name, vm_params |

        config.vm.define "#{vm_name}" do |vm_item|
        
            vm_item.vm.hostname    = "#{vm_name}"
            vm_item.vm.box         = "#{vm_params[:vm_box]}"
            vm_item.vm.box_version = "#{vm_params[:vm_box_version]}"
            vm_item.disksize.size  = "#{vm_params[:vm_disk_size]}"

            vm_item.vm.network "private_network", ip: "#{vm_params[:vm_ip]}", virtualbox__intnet: true
            vm_item.vm.network "forwarded_port", guest: 22, host: 2222
            vm_item.vm.network "forwarded_port", guest: 8080, host: 8080

            vm_item.vm.provider "virtualbox" do |vm_item_vb|
                vm_item_vb.cpus     = vm_params[:vm_cpu]
                vm_item_vb.memory   = vm_params[:vm_ram]
            end
        end
    end

    config.ssh.private_key_path = '~/.ssh/id_rsa'

    config.vm.synced_folder ".", "/vagrant", type: "rsync"
    config.vm.synced_folder "C:\\Source Codes\\", "/home/vagrant/Source Codes/", type: "rsync"

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
