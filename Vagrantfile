### configuration parameters ###
# which host port to forward box SSH port to
ANSIBLE1_IPv4 = "192.168.56.10"
ANSIBLE2_IPv4 = "192.168.56.12"
ANSIBLE3_IPv4 = "192.168.56.13"
CONTROL_IPv4 = "192.168.56.14"


### /configuration parameters ###


Vagrant.configure("2") do |config|
 
    config.vm.box = "generic/rhel9"
    config.vm.boot_timeout = 600
   
    config.vm.define "ansible1" do |machine|
      machine.vm.network "private_network", ip: ANSIBLE1_IPv4
      machine.vm.hostname == "ansible1"
    end
   
    config.vm.define "ansible2" do |machine|
      machine.vm.network "private_network", ip: ANSIBLE2_IPv4
      machine.vm.hostname == "ansible2"
    end
  
    config.vm.define "ansible3" do |machine|
      machine.vm.network "private_network", ip: ANSIBLE3_IPv4
      machine.vm.hostname == "ansible3"
    end
   
    config.vm.define 'control' do |machine|
      machine.vm.network "private_network", ip: CONTROL_IPv4
      machine.vm.synced_folder ".", "/home/vagrant/ansible", create: true, mount_options: ["dmode=750,fmode=600"]
      machine.vm.hostname == "control"
      # install ansible 2.9
      machine.vm.provision "shell",
        inline: "dnf -y install ansible"
      machine.vm.provision :ansible_local do |ansible|
        ansible.groups = {
          "nodes" => ["ansible[1:3]"]
        }
        ansible.host_vars = {
          "ansible1" => {"ansible_host" => ANSIBLE1_IPv4,
          "ansible_ssh_private_key_file" => "/home/vagrant/ansible/.vagrant/machines/ansible1/virtualbox/private_key"},
          "ansible2" => {"ansible_host" => ANSIBLE2_IPv4,
          "ansible_ssh_private_key_file" => "/home/vagrant/ansible/.vagrant/machines/ansible2/virtualbox/private_key"},
          "ansible3" => {"ansible_host" => ANSIBLE3_IPv4,
          "ansible_ssh_private_key_file" => "/home/vagrant/ansible/.vagrant/machines/ansible3/virtualbox/private_key"}
        }
        ansible.playbook               = "playbook.yml"
        ansible.provisioning_path      = "/home/vagrant/ansible"
        ansible.verbose                = true
        ansible.limit                  = "all" # or only "nodes" group, etc.
        # ansible.inventory_path         = "vagrant-inventory"
      end
    end
   
  end
