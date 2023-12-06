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

Basic startup parameters

We will start looking at the configuration file in the next task, but now let's look at the main Prometheus startup keys:

`--config.file="prometheus.yml"` - which configuration file to use;

`--web.listen-address="0.0.0.0:9090"` - the address that the embedded web server will listen to;

`--web.enable-admin-api` - enable or disable administrative API via web-interface;

`--web.console.templates="consoles"` - path to the directory with html templates;

`--web.console.libraries="console_libraries"` - path to the directory with libraries for templates;

`--web.page-title` - the title of the web page;

`--web.cors.origin=".*"` - CORS settings for web-interface;

`--storage.tsdb.path="data/"` - path for storing time series database;

`--storage.tsdb.retention.time` - default metrics retention time is 15 days, anything older will be deleted;

`--storage.tsdb.retention.size` - TSDB size, after which Prometheus will start deleting the oldest data;

`--query.max-concurrency` - maximum simultaneous number of requests to Prometheus via PromQL;

`--query.timeout=2m` - maximum execution time of one query;

`--enable-feature` - flag to enable various functions described here;

`--log.level` - logging level


Let's take a look at the main files:

`console_libraries` - a directory that contains libraries for html templates;

`consoles` - directory that contains html page templates for the web interface;

`prometheus` - Prometheus server executable;

`prometheus.yml` - Prometheus configuration file;

`prometheus.yml` - Prometheus configuration file; prometheus.yml - Prometheus configuration file; promtool - utility for checking configuration, working with TSDB and getting metrics.






### Push and Pull Model:

Prometheus uses a Pull model (also called Scraping) to collect metrics, 
meaning the Prometheus server will reach out to specified services 
by calling their configured HTTP endpoint to pull those metrics.

For example, in the configuration defined in `prometheus.yml`
file tells the Prometheus servers to fetch metrics every 15s on the specified endpoint.

![prometheus2](images/prometheus2.svg)


### 4 Types of Prometheus Metrics 

![prometheus3](images/prometheus3.png)








## Install 

install Prometheus, Alert Manager, Node Exporter and Nginx exporter
```
sudo apt install prometheus prometheus-alertmanager prometheus-node-exporter prometheus-nginx-exporter
```

see if all modules online
```
ps auxf | grep prometheus
```


### If nginx not start 
```bash
sudo nano /etc/nginx/sites-available/site-local.conf
```

if nginx exporter not in local machine change 127.0.0.1 to ip nginx exporter
```bash
listen 8080 default_server;

location /stub status {
    stub status;
    allow 127.0.0.1;
    deny all; 
}

```
after it need restart nginx `sudo systemctl restart nginx` 
 
and nginx exporter

```
sudo systemctl restart prometheus-nginx-exporter
```

and see status 
```
sudo systemctl status prometheus-nginx-exporter
```

Advanced Option can edit in 
```
sudo nano /etc/prometheus/prometheus.yml
```
add nginx job
```
- job name:
```




### Install Prometheus from Binary

download `prometheus-2.48.0.linux-amd64.tar.gz`
```bash
cd /opt/
wget https://github.com/prometheus/prometheus/releases/download/v2.48.0/prometheus-2.48.0.linux-amd64.tar.gz
``
Unzip him
```bash
tar -zxf prometheus-2.26.0-linux-amd64.tar.gz 
```
Delete prometheus archive `tar.gz` file
```bash
rm -rf prometheus-2.48.0.linux-amd64.tar.gz
```
Rename name to prometheus 
```bash
mv prometheus-2.48.0.linux-amd64 prometheus
```
Enter in directory
```bash
cd prometheus
```

Run prometheus (default site on port `localhost:9090`)
```
./prometius
```
---
Test
```
rate(prometheus_http_requests_total[1m])
```
---


### User for Systemd and Prometheus 

```bash
useradd -s /sbin/nologin -d /opt/prometheus prometheus
```
```bash
chown -R prometheus:prometheus /opt/prometheus
```
copy && past
```bash
nano /etc/systemd/system/prometheus.service
```
```ini
[Unit]
Description=Prometheus
Wants=network-online.target
After=network-online.target

[Service]
User=prometheus
Group=prometheus
ExecStart=/opt/prometheus/prometheus \
    --config.file=/opt/prometheus/prometheus.yml \
    --storage.tsdb.path=/opt/prometheus/data \
    --web.console.templates=/opt/prometheus/consoles \
    --web.console.libraries=/opt/prometheus/console_libraries

[Install]
WantedBy=default.target
```

```bash
systemctl daemon-reload
```
```bash
systemctl enable prometheus
```
```bash
systemctl start prometheus
```
```bash
systemctl status prometheus
```
```bash
ps ax | prometheus
```
See all Metrics  
```
localhost:9090/metrics
```

### Test with Promtool

Test config 
```bash
./promtool check config prometheus.yml
```
```bash
./promtool query instant http://localhost:9090 up
```
```
up{instance="localhost:9090", job="prometheus"} => 1 @[1617970111.787]
up{instance="localhost:9100", job="node"} => 1 @[1617970111.787]
```

```bash
root@node1:~# ./promtool query instant http://localhost:9090 'up{job="node"}'
```
```
up{instance="localhost:9100", job="node"} => 1 @[1617970210.143]
```

```bash
./promtool query instant http://localhost:9090 'node_disk_written_bytes_total'
```
```
node_disk_written_bytes_total{device="vda", instance="localhost:9100", job="node"} => 52323148800 @[1617970275.39]
```
```bash
./promtool query instant http://localhost:9090 'node_disk_written_bytes_total'
```
```
node_disk_written_bytes_total{device="vda", env="dev", instance="localhost:9100", job="node"} => 52338947072 @[1617970356.777]
node_disk_written_bytes_total{device="vda", instance="localhost:9100", job="node"} => 52332880896 @[1617970356.777]
```
### install Node_exporter from Binary

Download `node_exporter-1.7.0.linux-amd64.tar.gz`
```bash
cd /opt/
wget https://github.com/prometheus/node_exporter/releases/download/v1.7.0/node_exporter-1.7.0.linux-amd64.tar.gz
```

Unzip him
```bash
tar -xvf node_exporter-1.7.0.linux-amd64.tar.gz 
```
Remove archive `tar.gz`
```bash
rm -f node_exporter-1.7.0.linux-amd64.tar.gz 
```
Rename name to Node-Exporter 
```bash
mv node_exporter-1.7.0.linux-amd64 node_exporter 
```

Run Node-Exporter (default port `localhost:9100`)
```
./node_exporter
```
See all metrics
```bash
http://localhost:9100/metrics
```


**Collectors**
```bash
./node_exporter --help 2>&1 | grep collector
```

In addition to selecting the collectors that will give us different metrics, we can specify the following parameters to run:

`--web.listen-address=":9100"` - the address and port where the `node_exporter` will wait for incoming connections;

`--web.telemetry-path="/metrics"` - URL where our metrics will be available;

`--web.max-requests` - the maximum number of simultaneous requests to receive metrics on the port specified in the first parameter;

`--web.config=""` - experts




### User for Systemd for Exporter

```bash
useradd -s /sbin/nologin -d /opt/node_exporter node_exporter
```
```bash
chown -R node_exporter:node_exporter /opt/node_exporter
Copy && Past
```bash
nano /etc/systemd/system/node_exporter.service
```
```ini
[Unit]
Description=Node Exporter
Wants=network-online.target
After=network-online.target

[Service]
User=root
Group=root
Type=simple
ExecStart=/opt/node_exporter/node_exporter

[Install]
WantedBy=multi-user.target
```
Start the service with systemd 
```bash
sudo systemctl daemon-reload
```
```bash
systemctl enable node_exporter
```
```bash
systemctl start node_exporter
```
```bash
systemctl status node_exporter
```
> On the prometheus server, dont' forget to add the static config `prometheus.yml` for the collection of data!

> BEST PRACTICE: The official documentation does NOT recommend running node_exporter in docker, because it needs access to the host system to get all metrics. And you will need to mount all file systems inside the docker container. So it is much easier to run it as a service from the example above.

Test all metrics

```bash
curl localhost:9100/metrics
```

### Exporter connections to Prometheus

on Prometheus `prometheus.yml`
```yml 
global:
  scrape_interval:     15s

scrape_configs:
  - job_name: 'prometheus'
    static_configs:
    - targets: ['localhost:9090']

  - job_name: 'node'
    scrape_interval: 5s
    static_configs:
      - targets: ['localhost:9100']
```

```bash
 systemctl restart prometheus
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






















### Install Alertmanager from Website
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

### Simple Metrics

for monitoring up or down server A : metrics `up` Legend `{{job}}`

for monitoring time scrape A : metrics `scrape_duration_seconds` Legend `{{instance}} {{job}}`

for monitoring memory ram A:  metrics `100 - node_filesystem_avail_bytes{fstype!="tmfs"}/node_filesystem_size_bytes * 100` Legend `{{device}}`

for monitoring aggiration cpu  A: metrics `avg by (instance, cpu) (node_cpu_seconds_total)`

for monitoring how many cpu core A: metrics `count (count(node_cpu_seconds_total)without (mode)) without (cpu)`

### Function

for monitoring how many places take on disk `delta(node_filesystem_avail_bytes {device!='tmpfs'}[45s])`

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

### Run Promtail 

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

