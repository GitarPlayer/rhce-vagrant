### configuration parameters ###
# which host port to forward box SSH port to
ANSIBLE1_IPv4 = "192.168.56.10"
ANSIBLE2_IPv4 = "192.168.56.12"
ANSIBLE3_IPv4 = "192.168.56.13"
CONTROL_IPv4 = "192.168.56.14"

# Do not create a unique ssh key for each machine

### /configuration parameters ###
# create 


Vagrant.configure("2") do |config|

    config.vm.box = "generic/rhel9"
    config.vm.boot_timeout = 600
    config.ssh.insert_key = true
    # use vagrant trigger to unregister before destroy
    config.trigger.before :destroy do |trigger|
      trigger.run_remote = {inline: "bash -c '/usr/bin/subscription-manager remove --all && /usr/bin/subscription-manager unregister && /usr/bin/subscription-manager clean || true'"} 
    end
  
    config.trigger.after :up do |trigger|
      org = ENV['org']
      activationkey = ENV['activationkey']
      trigger.run_remote = {inline: "bash -c 'subscription-manager register --org=#{org} --activationkey=#{activationkey} --force'"} 
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
      machine.vm.synced_folder ".", "/vagrant", create: true, mount_options: ["dmode=750,fmode=600"]
      machine.vm.hostname == "control"
      machine.vm.provider :virtualbox
      org = ENV['org']
      activationkey = ENV['activationkey']
      # install ansible 2.9
      machine.vm.provision "shell",
        inline: "subscription-manager unregister || subscription-manager register --org=#{org} --activationkey=#{activationkey} && dnf -y install ansible && dos2unix /vagrant/.vagrant/machines/ansible?/virtualbox/private_key /vagrant/.vagrant/machines/control/virtualbox/private_key "
      machine.vm.provision :ansible_local do |ansible|
        ansible.groups = {
          "nodes" => ["ansible[1:2]"]
        }
        ansible.host_vars = {
          "ansible1" => {"ansible_host" => ANSIBLE1_IPv4,
          "ansible_ssh_private_key_file" => "/vagrant/.vagrant/machines/ansible1/virtualbox/private_key"},
          "ansible2" => {"ansible_host" => ANSIBLE2_IPv4,
          "ansible_ssh_private_key_file" => "/vagrant/.vagrant/machines/ansible2/virtualbox/private_key"},
        }
        ansible.playbook               = "playbook.yml"
        ansible.provisioning_path      = "/vagrant/"
        ansible.verbose                = true
        ansible.limit                  = "all" # or only "nodes" group, etc.
        # ansible.inventory_path         = "vagrant-inventory"
      end
    end
   
  end
