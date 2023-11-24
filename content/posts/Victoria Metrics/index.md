---
title: "Victoria Metrics"
discription: Victoria 
date: 2023-01-01T21:29:01+08:00 
draft: false
type: post
tags: ["VictoriaMetrics","Monitoring","Graphana", "Prometheus"]
showTableOfContents: true
--- 

![victoria1](images/victoria1.svg)


## Pre-setting

Installing packages
Let's make sure some packages are available:

`tar` - to unpack the archive with the binary.
`wget` - to download the archive.
`curl` - to execute queries to test the system.
Depending on the Linux distribution, the installation will be slightly different.

for DEB-based systems (Astra Linux / Debian / Ubuntu):
```bash
apt install tar wget curl
```
for RPM-based systems (RED OS / Rocky Linux / CentOS):
```bash
yum install tar wget curl
```

## Firewall
By default, VictoriaMetrics listens for requests on port `8428`. To communicate with it over the network, we need to open the appropriate TCP port. Our actions will differ depending on the firewall management utility we are using.


Iptables (typically for DEB-based systems):

```bash
iptables -I INPUT -p tcp --dport 8428 -j ACCEPT
```
We use iptables-persistent to save the rule:
```bash
apt install iptables-persistent
```
```bash
netfilter-persistent save
```

Firewalld (typically for RPM-based systems):
```bash
firewall-cmd --permanent --add-port=8428/tcp
```
```bash
firewall-cmd --reload
```

## Installing and running VictoriaMetrics

loading the binary and studying its startup parameters

download https://github.com/VictoriaMetrics/VictoriaMetrics/releases/tag/v1.95.1/victoria-metrics-linux-amd64-v1.95.1.tar.gz

or
```bash
wget https://github.com/VictoriaMetrics/VictoriaMetrics/releases/download/v1.95.1/victoria-metrics-linux-amd64-v1.95.1.tar.gz
```
Let's unpack the downloaded archive:
```bash
tar zxf victoria-metrics-linux-amd64-*.tar.gz -C /usr/local/bin/
```
You can run the command to examine all the application startup parameters:
```bash
victoria-metrics-prod -help | less
```
We will use:

`storageDataPath` - the path to the data storage directory.

`retentionPeriod` - period in months, after which the data will be removed from the database.

## Creating and configuring the systemd unit

In our example we will create an autorun with the following nuances:

The service will be started from a separate user to maximize security.
We will store the data in the `/var/lib/victoriametrics` directory.

Create a service user victoriametrics:
```bash
useradd -r -c 'VictoriaMetrics TSDB Service' victoriametrics
```
We also create directories for store data and process identifier
```bash
mkdir -p /var/lib/victoriametrics /run/victoriametrics
```
Let's set our new user as the owner for the created directories
```bash
mkdir -p /var/lib/victoriametrics /run/victoriametrics
```
```bash
chown victoriametrics:victoriametrics /var/lib/victoriametrics /run/victoriametrics
```
```bash
```
```bash
```
 download https://github.com/VictoriaMetrics/VictoriaMetrics/releases/tag/v1.95.1/victoria-metrics-linux-amd64-v1.95.1.tar.gz



```bash
```
























```bash
tar -zxvf victoria-metrics-linux-amd64-v1.95.1.tar.gz
```
This command breaks down as follows:

`-x`: Extract

`-z`: Use gzip compression

`-v`: Verbose mode (show the progress)

`-f`: Specify the file to extract

Run

```bash
./victoria-metrics-prod
```

`-storageDataPath` - VictoriaMetrics stores all the data in this directory. Default path is victoria-metrics-data in the current working directory.

`-retentionPeriod` - retention for stored data. Older data is automatically deleted. Default retention is 1 month (31 days). The minimum retention period is 24h or 1d. See these docs for more details.



## Vmagent


`vmagent` is an agent designed for collecting and sending time series data to the central data store in VictoriaMetrics.

Data Collection: vmagent can be configured to collect time series data from various sources, such as Prometheus, InfluxDB, and others.

Data Conversion: It can convert data from different formats into a format understandable by VictoriaMetrics.

Data Sending: vmagent sends the collected data to the central VictoriaMetrics data store for subsequent analysis and visualization.

Data Aggregation: vmagent can perform pre-aggregation of data before sending it to the data store. This can help reduce the data volume and lower the network load.

Scalability and High Performance: VictoriaMetrics and its components, including vmagent, provide high performance and scalability for efficient handling of large volumes of time series data.

![victoria2](images/vmagent.webp)



download https://github.com/VictoriaMetrics/VictoriaMetrics/releases/tag/v1.95.1/vmutils-linux-arm64-v1.95.1.tar.gz

```bash
tar -zxvf vmutils-linux-arm64-v1.95.1.tar.gz
```

```bash
cd vmutils-linux-arm64-v1.95.1
```

```bash
./vmagent -storageDataPath=<were storage db>
```

```bash
scrape_configs:
  - job_name: node_exporter
    consul_sd_configs:
      - server: 'localhost:9100'
```