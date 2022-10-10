
Vagrant.configure("2") do |config|
 
    config.vm.box = "almalinux/8"
    config.vm.boot_timeout = 600
   
    config.vm.define "ansible1" do |machine|
      machine.vm.network "private_network", ip: "192.168.56.10"
      machine.vm.hostname == "ansible1"
    end
   
    config.vm.define "ansible2" do |machine|
      machine.vm.network "private_network", ip: "192.168.56.12"
      machine.vm.hostname == "ansible2"
    end
  
    config.vm.define "ansible3" do |machine|
      machine.vm.network "private_network", ip: "192.168.56.13"
      machine.vm.hostname == "ansible3"
    end
   
    config.vm.define 'controller' do |machine|
      machine.vm.network "private_network", ip: "192.168.56.14"
      machine.vm.synced_folder ".", "/home/vagrant/ansible", create: true, mount_options: ["dmode=750,fmode=600"]
      machine.vm.hostname == "controller"
      # install ansible 2.9
      machine.vm.provision "shell",
        inline: "dnf -y install centos-release-ansible-29 && dnf -y install ansible"
      machine.vm.provision :ansible_local do |ansible|
        ansible.playbook               = "playbook.yml"
        ansible.provisioning_path      = "/home/vagrant/ansible"
        ansible.verbose                = true
        ansible.limit                  = "all" # or only "nodes" group, etc.
        ansible.inventory_path         = "inventory"
      end
    end
   
  end