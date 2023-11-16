![e2e workflow](https://github.com/GitarPlayer/rhce-vagrant/actions/workflows/vagrant-up.yml/badge.svg)
# rhce-vagrant

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

