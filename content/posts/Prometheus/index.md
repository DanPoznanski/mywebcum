---
title: "Prometheus"
discription: prometheus on ubuntu 
date: 2023-08-01T21:29:01+08:00 
draft: false
type: post
tags: ["Linux","Monitoring","Graphana"]
showTableOfContents: true
--- 


![prometheus1](images/prometheus1.svg)



## Prometheus

- Prometheus - database of time series
- Exporter - instrument for get metric
- Alertmanager - notification and everything is connected with it


### push and pull model :

Prometheus uses a Pull model (also called Scraping) to collect metrics, 
meaning the Prometheus server will reach out to specified services 
by calling their configured HTTP endpoint to pull those metrics.

For example, in the configuration defined in `prometheus.yml`
file tells the Prometheus servers to fetch metrics every 15s on the specified endpoint.

![prometheus2](images/prometheus2.svg)


### 4 Types of Prometheus Metrics 

![prometheus3](images/prometheus3.png)








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




### install from website

download `prometeus-2.26.0-linux-amd64.tar.gz`

create new folder `mkdir monitoring` and copy past in folder

unzip him
```
tar -xvf prometeus-2.26.0-linux-amd64.tar.gz 
```
run prometius default site `localhost:9090`
```
./prometius
```
test
```
rate(prometheus_http_requests_total[1m])
```

### install Node_exporter from website

download `node_exporter-1.1.2-linux-amd64.tar.gz` 

create new folder `mkdir monitoring` and copy past in folder
unzip him
```
tar -xvf pnode_exporter-1.1.2-linux-amd64.tar.gz  
```
run exporter to see default site `localhost:9100`
```
./node_exporter
```

### Take metrics from node_exporter

in prometeus.yml
```
scrape_configs:
  - jobe name: 'hello from node_exporter'
    statics_configs:
    - targets: [localhost:9100]
```

to see added target 
localhost:9090 => Status => Targets 


### Install Alertmanager from website
![prometheus](images/prometheus4.png)

download `alertmanager-0.21.0.linux-amd64.tar.gz` 

create new folder `mkdir monitoring` and copy past in folder
unzip him
```
tar -xvf alertmanager-0.21.0.linux-amd64.tar.gz  
```
run alermanager
```
./alertmanager
```


`alertrules.yml` in prometheus server not in endpoint
```
groups:
 - name: AllInstances
   rules:
   - alert: InstanceDown
     expr: up==0
     for: 30s
```
prometheus.yml add 
```
rule_files:
  - alertrules.yml
alerting:
  alertmanagers:
    - static_configs:
      - targets:
        - localhost: 9093
```
restart all and test on localhost:9090 => `Alerts`


### PagerDuty

have alternative zenduty

Services =>  Service Directory => New Service 

name of service and add integration type `prometheus` and push `Add Service`

after it go to `integration` and copy `integration key` and `integration url` 

and change `nano alertmanager.yml`
```
global:
  resolve_timeout: 5m
  pagerduty_url: '<url from web>'
route:
  receiver: 'pagerduty_notifications'
receivers:
- name: 'pagerduty_notifications'
  pagerduty_configs: 
  - service_key: '<key from web>'
    send_resolved: true
inhibit_rules:
  - source_match:
      severity: 'warning'
    equel: ['alertname', 'dev', 'instance']
```

### Grafana

### install Grafana on Ubuntu

Install the prerequisite packages:

install `apt-transport-https` `software-properties-common` `wget`
```bash
sudo apt-get install -y apt-transport-https software-properties-common wget
```
Import the GPG key:
```bash
sudo mkdir -p /etc/apt/keyrings/
wget -q -O - https://apt.grafana.com/gpg.key | gpg --dearmor | sudo tee /etc/apt/keyrings/grafana.gpg > /dev/null
```
To add a repository for stable releases, run the following command:

```bash
echo "deb [signed-by=/etc/apt/keyrings/grafana.gpg] https://apt.grafana.com stable main" | sudo tee -a /etc/apt/sources.list.d/grafana.list
```
run the following command to update the list of available packages:
```
# Updates the list of available packages
sudo apt-get update
```
To install Grafana OSS, run the following command:
```
# Installs the latest OSS release:
sudo apt-get install grafana
```

see status grafana 
```
systemctl status grafana-server.service
```
start grafana default port `localhost:3000`
```
systemctl status grafana-server.service
```
first time in, user `admin` password `admin` need change password..


### Add metrics for dashboard Grafana :

`+` => `Dashboard` => `+ Add new panel` => `default` change to `promethius`

for add panel :

`Panel` => `Graph` => `panel title: my_event` 

for add metrics:

`A` => `metrics` => `prometheus_http_requests_total` and take most likely legend 

for add name to legend `{{handler}}`

after `Apply`

metrics for cpu im take from `node_exporter` its `node_cpu_seconds_total(mode="iowait")` for legend `{{cpu}} - {{job}}`

for save dashboard `save dashboard` and give name

### Simple metrics

for monitoring up or down server A : metrics `up` Legend `{{job}}`

for monitoring time scrape A : metrics `scrape_duration_seconds` Legend `{{instance}} {{job}}`

for monitoring memory ram A:  metrics `100 - node_filesystem_avail_bytes{fstype!="tmfs"}/node_filesystem_size_bytes * 100` Legend {{device}}

### Install Grafana Loki 

database logs of grafana

![prometheus5](images/prometheus4.webp)

Download `loki-linux-amd64.zip` and on `promtail-linux-amd64.zip` (promtail need for endpoint node ) unzip all 

```
curl -O -L "https://github.com/grafana/loki/releases/download/v2.9.2/loki-linux-amd64.zip"
# extract the binary
unzip "loki-linux-amd64.zip"
make sure it is executable
chmod a+x "loki-linux-amd64"
```

download to server
```
wget https://raw.githubusercontent.com/grafana/loki/main/cmd/loki/loki-local-config.yaml
```

download  to  node
```
wget https://raw.githubusercontent.com/grafana/loki/main/clients/cmd/promtail/promtail-local-config.yaml
```


### In Grafana add loki 

`Configuration` => `Data Source` => `Add data source` => `HTTP` => `URL localhost:3100` => Save+Test






### Change Configuration of promtail 


promtail-local-config.yaml
```
server:
  http_listen_port: 9080
  grpc_listen_port: 0

positions:
  filename: /tmp/positions.yaml

clients:
  - url: http://localhost:3100/loki/api/v1/push

scrape_configs:
- job_name: system
  static_configs:
  - targets:
      - localhost
    labels:
      job: varlogs
      __path__: /var/log/*log
```


### Run Loki 

run with config (default port `localhost:9080`)
```
./loki-linux-amd64 -config.file=loki-local-config.yaml
```

### Run promtail 

run with config
```
./promtail-linux-amd64 -config.file=promtail-local-config.yaml
```


### Explorer logs

`Explorer` => `log labels` => `{filename="/var/log/syslog"}`

for 2 files `{filename=-"/var/log/syslog|/var/log/auth.log"}`



### LogQL: Log query language

https://grafana.com/docs/loki/latest/query/

`{filename=-"/var/log/syslog|/var/log/auth.log"} |= "requested_routers"` see all logs **with** `requested_routers` 

`{filename=-"/var/log/syslog|/var/log/auth.log"} != "Succseed"` see all logs **without** `Succseed`

`{job="app"} |= count() by (status) |- {job="db"} |= count() by (status)` combine 2 results to 1 

`count_over_time=({filename="/var/log/syslog"} |= "error" [12h])` 
 
### Wiki Awesome Prometheus alerts

Collection of alerting rules 

https://samber.github.io/awesome-prometheus-alerts/ 



### Grafana Dashboard Collection

https://grafana.com/grafana/dashboards/


### Import Dashboard
`+` => `import` => `load`

