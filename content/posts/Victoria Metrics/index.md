---
title: "Victoria Metrics"
discription: Victoria on ubuntu 
date: 2023-01-01T21:29:01+08:00 
draft: false
type: post
tags: ["Linux","Monitoring","Graphana"]
showTableOfContents: true
--- 






![victoria1](images/victoria1.svg)
```bash
wget https://github.com/VictoriaMetrics/VictoriaMetrics/releases/tag/v1.95.1/victoria-metrics-linux-amd64-v1.95.1.tar.gz

```


```bash
tar -zxvf victoria-metrics-linux-amd64-v1.95.1.tar.gz

```
run
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


```bash
wget https://github.com/VictoriaMetrics/VictoriaMetrics/releases/tag/v1.95.1/vmutils-linux-arm64-v1.95.1.tar.gz
```
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