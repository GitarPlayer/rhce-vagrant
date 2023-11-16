### configuration parameters ###
# which host port to forward box SSH port to
ANSIBLE1_IPv4 = "192.168.56.10"
ANSIBLE2_IPv4 = "192.168.56.12"
ANSIBLE3_IPv4 = "192.168.56.13"
CONTROL_IPv4 = "192.168.56.14"


### /configuration parameters ###
# create 

Vagrant.configure("2") do |config|

    config.vm.box = "generic/rhel9"
    config.vm.boot_timeout = 600
    # use vagrant trigger to unregister before destroy
    config.trigger.before :destroy do |trigger|
      trigger.run_remote = {inline: "bash -c '/usr/bin/subscription-manager remove --all && /usr/bin/subscription-manager unregister && /usr/bin/subscription-manager clean || true'"} 
    end
  
    config.trigger.before :provision do |trigger|
      org = ENV['org']
      activationkey = ENV['activationkey']
      trigger.run_remote = {inline: "bash -c 'subscription-manager unregister || subscription-manager register --org=#{org} --activationkey=#{activationkey}'"} 
    end
  

    config.vm.define "ansible1" do |machine|
      machine.vm.network "private_network", ip: ANSIBLE1_IPv4
      machine.vm.hostname == "ansible1"
      machine.vm.provider :virtualbox
    end
   
    config.vm.define "ansible2" do |machine|
      machine.vm.network "private_network", ip: ANSIBLE2_IPv4
      machine.vm.hostname == "ansible2"
      machine.vm.provider :virtualbox
    end

   
    config.vm.define 'control' do |machine|
      machine.vm.network "private_network", ip: CONTROL_IPv4
      machine.vm.synced_folder ".", "/home/vagrant/ansible", create: true, mount_options: ["dmode=750,fmode=600"]
      machine.vm.hostname == "control"
      machine.vm.provider :virtualbox
      org = ENV['org']
      activationkey = ENV['activationkey']
      # install ansible 2.9
      machine.vm.provision "shell",
        inline: "subscription-manager unregister || subscription-manager register --org=#{org} --activationkey=#{activationkey} && dnf -y install ansible"
      machine.vm.provision :ansible_local do |ansible|
        ansible.groups = {
          "nodes" => ["ansible[1:2]"]
        }
        ansible.host_vars = {
          "ansible1" => {"ansible_host" => ANSIBLE1_IPv4,
          "ansible_ssh_private_key_file" => "/home/vagrant/ansible/.vagrant/machines/ansible1/virtualbox/private_key"},
          "ansible2" => {"ansible_host" => ANSIBLE2_IPv4,
          "ansible_ssh_private_key_file" => "/home/vagrant/ansible/.vagrant/machines/ansible2/virtualbox/private_key"},
        }
        ansible.playbook               = "playbook.yml"
        ansible.provisioning_path      = "/home/vagrant/ansible"
        ansible.verbose                = true
        ansible.limit                  = "all" # or only "nodes" group, etc.
        # ansible.inventory_path         = "vagrant-inventory"
      end
    end
   
  end
