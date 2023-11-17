![e2e workflow](https://github.com/GitarPlayer/rhce-vagrant/actions/workflows/vagrant-up.yml/badge.svg)
# rhce-vagrant
This contains a Vagrantfile to deploy one control node and two to three ansible managed nodes for practising RHCSA. It will automatically register your hosts using activation keys upon running `vagrant up` and automatically unregister your hosts upon running `vagrant destroy`.  
Please get a Red Hat developer subscription here: https://developers.redhat.com/articles/faqs-no-cost-red-hat-enterprise-linux
Then create an activation key and export the org and activationkey variable.

## Dependencies
We need vagrant, virtualbox and the vagrant plugin `vagrant-vbguest `. I need vagrant-vbguest since the shared folder will not work if the Virtual Guest additions are not in sync with the virtualbox version in use. One cannot use the file provisioner, since it will attempt to copy files that are not there yet (the ssh private_key that gets generated and vagrant ssh uses). 

You can install the dependencies like so with choco:
```Powershell
choco install -y vagrant; choco install -y virtualbox; vagrant plugin install vagrant-vbguest; Restart-Computer
```

Tested versions:
Vagrant 2.4.0 
virtualbox v7.0.6.20230201
vagrant-vbguest (0.31.0)

## Recommendation
If you are working from a Windows vagrant host I would go for Windows Terminal. 

## Usage MacOs *Nix
```bash
export org=1111
export activationkey='your_activation_key_name'
vagrant up 
vagrant ssh control
# when done
vagrant destroy
```

## Usage Windows
```Powershell
$Env:org='1111'
$Env:activationkey='your_activation_key_name'
vagrant up
vagrant ssh control
# when done
vagrant destroy
```

