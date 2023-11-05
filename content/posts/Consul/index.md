---
title: "Consul"
discription: Consul
date: 2023-05-08T21:29:01+08:00 
draft: false
type: post
tags: ["Devops","Proxy","json"]
showTableOfContents: true
---

![consul1](images/consul1.svg)



Consul provides feature such as:

- Service discovery

- Tagging

- Health checks

- Secure Service Communication

- Multi Datacenter

- System-wide key/value storage


![consul2](images/consul2.webp)


## Server vs Client

Server: 

running on individual machine

form a failsafe cluster

communicating with client machine

Client:

working on machine were running service

checking health of server

sending information on server

example config registration service
```json
`{ "service":{"name": "web", "tags":["be"], "port": 80,"check":{"args": ["curl", "localhost"], "interval": "10s" }}}`
```
## Instal consul 

create account consul
```
useradd -s /bin/false -m consul
```
download binar file https://developer.hashicorp.com/consul/downloads for ubuntu 

for example
```
https://releases.hashicorp.com/consul/1.10.3/consul_1.10.3_linux_amd64.zip

```
unzip
```
unzip consul_1.10.3_linux_amd64.zip.
```
Then move the file to `~/bin.`
```
cp consul ~/bin/
```

After this, add the directory with the binary file to `$PATH`. For this you need
open `~/.bashrc` and add the line
```
export PATH=$PATH:~/bin.
```
As a result, you will be able to call Consul from the command line by typing
```
consul
```
Option2:
```
 wget -O- https://apt.releases.hashicorp.com/gpg | sudo gpg --dearmor -o /usr/share/keyrings/hashicorp-archive-keyring.gpg
 echo "deb [signed-by=/usr/share/keyrings/hashicorp-archive-keyring.gpg] https://apt.releases.hashicorp.com $(lsb_release -cs) main" | sudo tee /etc/apt/sources.list.d/hashicorp.list
 sudo apt update && sudo apt install consul
```

for fix
```
sudo apt install software-properties-common

curl -fsSL https://apt.releases.hashicorp.com/gpg | sudo apt-key add -

sudo apt-add-repository "deb [arch=amd64] https://apt.releases.hashicorp.com $(lsb_release -cs) main"

sudo apt-get update && sudo apt-get install consul
```
test:
```
consul
```



see status of service
```
systemctl status consul
```
see config of consul service
```
cat /lib/systemd/system/consul.service
```

Agents Configuration File Reference
```
cd /etc/consul.d 

nano consul.hcl
```
```
datacenter = "dc"
server = true
data_dir = "/opt/consul"
client_addr = "192.168.0.1 127.0.0.1"
bootstrap_expect = 3
bind_addr = "192.168.0.1"
encrypt = "8kKJSxjc6Hs7w5Qi/1ojxs2OPeZTnGskAIpZ2GRw4Eo="

```
`server = true` -work like server false work like client

`client_addr = "192.168.0.1 127.0.0.1"` ip of server and 127.0.0.1 need for cli 

`bind_addr` my ip address

`bootstrap_expect = 3` min 3 servers
`encrypt = "8kKJSxjc6Hs7w5Qi/1ojxs2OPeZTnGskAIpZ2GRw4Eo="` its key for consul need generate first 

## Generate key consul

```
consul keygen
```


## Useful commands

`consul validate /etc/consul.d/consul.hcl` - checking the agent config

`consul join <IP address or node name>` — join to cluster, new member

`consul leave` — exclude a node from the cluster

`consul maint -enable` — enable service mode on the node

`consul maint -disable` — return to working mode from maintenance mode

`consul members` — show cluster members

`consul info` — general information about the cluster

`consul reload` — reread the agent configuration

`consul catalog services` — show a list of registered services in the cluster

`consul register backend.json` — register the service described in the fe.json file

`consul deregister backend.json` — deregister the service described in the fe.json file



## Configuratin Client of Consul


`nano /etc/consul.d/consul.hcl`
```
data_dir = "/opt/consul"
client_addr = "192.168.0.4 127.0.0.1"
bind_addr = "192.168.0.4"
encrypt = "8kKJSxjc6Hs7w5Qi/1ojxs2OPeZTnGskAIpZ2GRw4Eo="
enable_local_script_checks = true
```

`enable_local_script_checks = true` enable check scripts to work service online

start consul
```
systemctl start consul
```

### Config json script 

in `/etc/consul.d` create new file `backend.json` 
```json
"service":
  { "name": "be",
    "tags": [ "be" ],
    "check":
      {
         "id": "NGINX",
         "name": "Test check",
         "http": "http://localhost",
         "method": "GET",
         "interval": "10s",
         "timeout": "1s"
      }
  }
}
```

for test
```
{ "service": { "name": "be", "tags": [ "be" ], "port":
80, "check": { "args": [ "curl", "localhost" ],
"interval": "10s" } } }
```

## DNS Interface

Get information about a node:
```
dig @127.0.0.1 -p 8600 client.node.consul  # <node>.node[.datacenter].<domain>
```
```
dig @127.0.0.1 -p 8600 vm-9918226g.node.consul
```

Get information about a services:
```
dig @127.0.0.1 -p8600 be.service.consul # [tag.]<service>.service[.datacenter].<domain>
```

More details about the service:
```
dig @127.0.0.1 -p8600 be.service.consul SRV
```

## HTTP API

Show nodes in cluster"
```bash
curl http://localhost:8500/v1/catalog/nodes?pretty
```
Show services in cluster
```bash
curl http://127.0.0.1:8500/v1/catalog/services
```
Show node vm-cb4c04f2 status
```bash
curl http://127.0.0.1:8500/v1/health/node/vm-cb4c04f2?pretty
```
Show status serivce on local node `backend` 
```bash
curl http://localhost:8500/v1/agent/health/service/name/backend?pretty
```

`?pretty` format json


## UI Config enable

graphic interface

`nano /etc/consul.d/consul.hcl`
```
ui_config{
  enabled=true
}
```

need create tunnel for virtual machine
```
ssh -L 8500:localhost:8500 root@<ip_of_machine>
```

after goto `127.0.0.1:8500/ui`