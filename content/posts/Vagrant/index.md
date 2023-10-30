---
title: "Vagrant"
discription: Vagrant
date: 2023-05-03T21:29:01+08:00 
draft: false
type: post
tags: ["Linux","vagrant","Terraform"]
showTableOfContents: true
--- 








![vagrant1](images/vagrant1.svg)



## Example

vagrantfile
```vagrantfile
vagrant.configure("2") do |config|
  
  config.vm.provider :virtualbox do |v|
    v.memory = 2048
  end

  config.vm.define "reactjsserver" do |db|
    db.vm.box = "ubuntu/focal64"
    db.vm.hostname = "reactjsserver"
    db.vm.network :private_network, ip: "10.10.10.10"
    db.vm.provision "ansible" do |ansible"
        ansible.playbook = "OneServer/reactjs.yaml"
        ansible.become = true
    end 
  end
end
```


run vagrant  
```
vargant up
```

connect to vagrant virtualmachine
```
vagrant ssh
```

`ss -tlpn` see all ports


see all boxes
```
vagrant box list
```

delete virtual machine
```
vagrant destroy
```


## Search Boxes 

https://app.vagrantup.com/boxes/search