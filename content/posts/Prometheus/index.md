---
title: "Prometheus"
discription: prometheus on ubuntu 
date: 2023-08-01T21:29:01+08:00 
draft: false
type: post
tags: ["Linux","Monitoring"]
showTableOfContents: true
--- 


![prometheus1](images/prometheus1.svg)

## Install 

install Prometheus, Alert Manager, Node Exporter and Ngnix exporter
```
sudo apt install prometheus prometheus-alertmanager prometheus-node-exporter prometheus-ngnix-exporter
```

see if all modules online
```
ps auxf | grep prometheus
```


### if ngnix not start 
```
sudo nano /etc/ngnix/sites-available/site-local.conf
```

if ngnix exporter not in local machine change 127.0.0.1 to ip ngnix exporter
```
listen 8080 default_server;

location /stub status {
    stub status;
    allow 127.0.0.1;
    deny all; 
}

```
after it need restart ngnix `sudo systemctl restart ngnix` 
 
and ngnix exporter

```
sudo systemctl restart prometheus-nginx-exporter
```

and see status 
```
sudo systemctl status prometheus-nginx-exporter
```

advanced option can edit in 
```
sudo nano /etc/prometheus/prometheus.yml
```
add ngnix job
```
- job name:
```