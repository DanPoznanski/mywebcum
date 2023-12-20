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


###  Configurate Prometheus for Systemd 

```bash
useradd -s /sbin/nologin -d /opt/prometheus prometheus
```
```bash
chown -R prometheus:prometheus /opt/prometheus
```
Copy && Past
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

## Exporters

### Node_Exporter from Binary

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
```bash
cd node_exporter
```
Run Node-Exporter (default port `localhost:9100`)
```
./node_exporter
```
See all metrics
```bash
http://localhost:9100/metrics
```
>>check added target 
>>`localhost:9090` => Status => Targets 

**Collectors**
```bash
./node_exporter --help 2>&1 | grep collector
```

In addition to selecting the collectors that will give us different metrics, we can specify the following parameters to run:

`--web.listen-address=":9100"` - the address and port where the `node_exporter` will wait for incoming connections;

`--web.telemetry-path="/metrics"` - URL where our metrics will be available;

`--web.max-requests` - the maximum number of simultaneous requests to receive metrics on the port specified in the first parameter;

`--web.config=""` - experts




### Configurate Node_Exporter for Systemd

```bash
useradd -s /sbin/nologin -d /opt/node_exporter node_exporter
```
```bash
chown -R node_exporter:node_exporter /opt/node_exporter
```
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

### Connect Node_Exporter to Prometheus

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


### Redis_Exporter from Binary 

Install Redis server
```bash
apt install redis-server
```
check status
```bash
systemctl status redis-server
```
connect to redis
```
connect to redis-cli  
```bash
redis-cli
info
```

**Install Redis-Exporter** 

Download
```bash
cd /opt/
wget https://github.com/oliver006/redis_exporter/releases/download/v1.55.0/redis_exporter-v1.55.0.linux-amd64.tar.gz
```
```bash
tar -xvf redis_exporter-v1.55.0.linux-amd64.tar.gz
```
Remove archive `tar.gz`
```bash
rm -rf redis_exporter-v1.55.0.linux-amd64.tar.gz 
```
Rename name to Redis-Exporter 
```bash
mv redis_exporter-v1.55.0.linux-amd64 redis_exporter
```
```bash
cd redis_exporter
```
Run PostgreSQL-Exporter (default port `localhost:9187/metrics`)
```
./postgresql_exporter
```
```
./postgresql_exporter -help
```

### Configurate Redis_Exporter for Systemd

Add user 
```bash
useradd -s /sbin/nologin -d /opt/redis_exporter redis_exporter
```
```bash
chown -R redis_exporter:redis_exporter /opt/node_exporter
```
Copy && Past
```bash
nano /etc/systemd/system/redis_exporter.service
```

```bash
[Unit]
Description=Redis Exporter
Wants=network-online.target
After=network-online.target

[Service]
User=redis_exporter
Group=redis_exporter
Type=simple
ExecStart=/opt/redis_exporter/redis_server /etc/redis/redis.conf
RestartSec=5s
Restart=on-success

[Install]
WantedBy=multi-user.target
```
other option no redis config:
```bash
ExecStart=/usr/bin/redis_exporter \
    -web.listen-address ":9121" \
    -redis.addr "redis://ip.of.redis.server:6379" \
    -redis.password "your-strong-redis-password"
```

```bash
systemctl start redis_exporter.service
```
...bash
systemctl enable redis_exporter.service
```
```bash
systemctl status redis_exporter.service
```

### Connect Redis_Exporter to Prometheus

Edit `prometheus.yml`
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

  - job_name: 'redis'
    static_configs:
      - targets: ['localhost:9121']
```
```bash
 systemctl restart prometheus
```





### PostgreSQL from Repository

install postgresql-14
```bash
apt install  postgresql-14
```
check if postgresql running
```bash
ps ax | grep post
```
switch user to postgres
```bash
su postgres
```
enter to postrges
```bash
psql
\l
\q
```

### PostgreSQL Server Configuration

The PostgreSQL server provides two different password encryption methods: `md5` and `scram-sha-256`. Both password encryptions can be configured via the PostgreSQL config file `postgresql.conf`.

In this step, you'll set up PostgreSQL to use the `scram-sha-256` password encryption.

This example uses the PostgreSQL server v14 that is installed on an Ubuntu system, so the PostgreSQL configuration files is stored in the `/etc/postgresql/14/main` directory.

Move to the working directory to the `/etc/postgresql/14/main` directory and open the configuration file `postgresql.conf` via the nano editor command.

```bash
cd /etc/postgresql/14/main
sudo nano postgresql.conf
```
Uncomment the option `password_encryption` and change the value to `scram-sha-256`.

```
password_encryption = scram-sha-256     # scram-sha-256 or md5
```
> Save the file and exit the editor when you are finished.

Next, open the config file `pg_hba.conf`` via the nano editor command below. The file `pg_hba.conf`` is the configuration where password authentication methods are defined for hosts or IP addresses.

```bash
sudo nano pg_hba.conf
```
Change the default authentication methods for the host `127.0.0.1/32` and `::1/128` to `scram-sha-256`. With this, the authentication method `scram-sha-256` will be used for every client connection to the PostgreSQL server `127.0.0.1`.
```
# "local" is for Unix domain socket connections only
local   all             all                                     peer
# IPv4 local connections:
host    all             all             127.0.0.1/32            scram-sha-256
# IPv6 local connections:
host    all             all             ::1/128                 scram-sha-256
```
Save and exit the editor when you're done.

Lastly, run the below systemctl command utility to restart the PostgreSQL service and apply the changes.

```bash
sudo systemctl restart postgresql
```



### Postgres_Exporter from Binary


```bash
cd /opt/
wget https://github.com/prometheus-community/postgres_exporter/releases/download/v0.15.0/postgres_exporter-0.15.0.linux-arm64.tar.gz
```
```bash
tar -xvf postgres_exporter-0.15.0.linux-arm64.tar.gz
```
Remove archive `tar.gz`
```bash
rm -f postgres_exporter-0.15.0.linux-arm64.tar.gz
```
Rename name to PostgreSQL-Exporter 
```bash
mv postgres_exporter-0.15.0.linux-arm64.tar.gz postgres_exporter
```
```bash
cd porstgres_exporter
```

Run Postgres_exporter (localhost:9187/mertics)
```bash
DATA_SOURCE_NAME=postgresql://postgres_exporter:password@localhost:5432/postgres?sslmode=disable ./postgress_exporter
```

**Configuring Postgres Exporter**

I this step, you'll configure the `postgres_exporter` to gather PostgreSQL metrics, and this can be done by defining the PostgreSQL user and password. You'll also set up and configure the systemd service for the `postgres_exporter`.

With the `postgres_exporter`, you can expose metrics for all available databases on the PostgreSQL server, or you can expose specific databases that you want to monitor. You can also use secure SSL mode or non-SSL mode.

Move the current working directory to `/opt/postgres_exporter`. via the cd command below.

```bash
cd /opt/postgres_exporter
```
Now create a new file `.env` using the below nano editor command.
```bash
nano .env
```
Add the following lines to the file. Also, be sure to change the details of the PostgreSQL user, password, and host. With this `.env` file, you'll scrape and gathers PostgreSQL metrics from all available databases. You can also gather metrics from a specific PostgreSQL database, and adjust the following config file.
```
# Format
#DATA_SOURCE_NAME=postgresql://username:password@localhost:5432/postgres?sslmode=disable

# Monitor all databases via postgres_exporter
DATA_SOURCE_NAME="postgresql://postgres:strongpostgrespassword@localhost:5432/?sslmode=disable"

# Monitor specific databases on the PostgreSQL server
# DATA_SOURCE_NAME="postgresql://username:password@localhost:5432/database-name?sslmode=disable"
```
Save the file and exit the editor when you're finished.



### Configurate Postgres_Exporter for Systemd

Create a new system user `postgres_exporter` 
```bash
useradd -s /sbin/nologin -d /opt/postgres_exporter postgres_exporter
```
ownership of the `/opt/postgres_exporter` directory to the user `postgres_exporter`.
```bash
chown -R postgresql_exporter:postgres_exporter /opt/postgres_exporter
```
Copy && Past
```bash
sudo nano /etc/systemd/system/postgres_exporter.service
```
```ini
[Unit]
Description=Postgresql Exporter
Wants=network-online.target
After=network-online.target

[Service]
User=postgres_exporter
Group=postgres_exporter
WorkingDirectory=/opt/postgres_exporter
EnvironmentFile=/opt/postgres_exporter/.env
ExecStart=/opt/postgres_exporter/postgres_exporter --web.listen-address=:9187 --web.telemetry-path=/metrics
Restart=always

[Install]
WantedBy=multi-user.target
```
Reload the systemd manager and apply the changes.
```bash
sudo systemctl daemon-reload
```
After the systemd manager is reloaded, start and enable the `postgres_exporter` service via the systemctl command utility below.
```bash
sudo systemctl start postgres_exporter
sudo systemctl enable postgres_exporter
```
The `postgres_exporter` should be running and scrape metrics from the PostgreSQL server. Also, it should be enabled and will be run automatically upon the bootup.

You'll receive the output similar to this - the `postgres_exporter` service is running and it's enabled.
```bash
sudo systemctl status postgres_exporter
```
**Setting up Firewall**
In this step, you'll set up the firewall to open the default port of `postgres_exporter` - **TCP 9187**. After that, you'll verify that the `postgres_exporter` metrics is accessible via the web browser.

For Ubuntu systems that used UFW as the firewall, run the below ufw command to add port 9187 to the ufw firewall. Then, reload the firewall to apply the changes.
```bash
sudo ufw allow 9187/tcp
sudo ufw reload
```
You can now verify the list of ports on UFW via the ufw command below.
```bash
sudo ufw status
```


### Connect Postgres_Exporter to Prometheus

Edit prometheus.yml

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

  - job_name: 'redis'
    static_configs:
      - targets: ['localhost:9121']

  - job_name: 'postgres'
    static_configs:
      - targets: ['localhost:9187']
```
```bash
ps aux | prom
```

Reload process 
```bash
kill -HUP process_id
```
Or restart the Prometheus service and apply the changes.
```bash
sudo systemctl restart prometheus
```
&nbsp;

&nbsp;
> Tips: Create Postgresql Superuser for easy connect 
>> su - postgresql
>>
>> psql
>>> create user prom with password '12345678';
>>>
>>> alter user prom superuser;



### MyAPP_Exporter


>  I have my own application i need install golang and integrate golang prometheus client 

Download Golang
```bash
wget https://go.dev/dl/go1.21.5.linux-amd64.tar.gz
```
unpack `tar.gz`
```bash
tar  -xzvf go1.21.5.linux-amd64.tar.gz
```
remove pack 
```bash
rm -rf go1.21.5.linux-amd64.tar.gz
```

```bash
echo 'export GOROOT=$HOME/go' >> .bashrc
echo 'export PATH=$PATH:$GOROOT/bin' >> .bashrc
```

The first variable GOROOT is needed by the golang interpreter itself: thanks to it it finds the path to its built-in libraries. The second variable is required so that we do not write the full path to the go binary, but the system finds it by itself. Now we can log in and check that golang has been successfully installed:
```bash
go version
```
### Creating the application

Create directory for our project:
```bash
mkdir -p app/src/app
cd app/src/app
```
Inside it we will create a simple `main.go` file
```go
package main

// Import basic modules - fmt, log, net/http, time, math/rand
import (
    "fmt"
    "log"
    "net/http"
    "time"
    "math/rand"

    // Add Prometheus client library import
    "github.com/prometheus/client_golang/prometheus"
    "github.com/prometheus/client_golang/prometheus/promhttp"
)

var (
        // Metric for calculating request processing time
        apiDurations = prometheus.NewSummary(
                prometheus.SummaryOpts{
                        Name:       "app_api_durations_seconds",
                        Help:       "API latency distributions.",
                        Objectives: map[float64]float64{0.5: 0.05, 0.9: 0.01, 0.99: 0.001},
                })

        // Metric for counting the number of incoming requests
        apiProcessed = prometheus.NewCounter(prometheus.CounterOpts{
                Name: "app_api_processed_ops_total",
                Help: "The total number of processed requests",
        })
)

// A function that will handled all requests to our web server
func handler(w http.ResponseWriter, r *http.Request) {
    // Increase the counter of the number of incoming requests
    apiProcessed.Inc()
    
    // Time to start processing the request
    start := time.Now()
    
    // asleep for a random number of seconds - from 0 to 2х
    time.Sleep(time.Duration(rand.Intn(2)) * time.Second)
    // In response, we send the path that the user has requested
    fmt.Fprintf(w, "%s\n", r.URL.Path)

    // After the end of processing, count how much time has passed since the start of processing
    duration := time.Since(start)
    // Save the processing time to a metric
    apiDurations.Observe(duration.Seconds())
}

// The main function is the starting point of our program
func main() {
    // Register our metrics
    prometheus.MustRegister(apiDurations)
    prometheus.MustRegister(apiProcessed)

    // Initialize the random number generator
    rand.Seed(time.Now().UnixNano())
    // We define twhen making requests to / - to any http path, it is necessary to call function handler 
    http.HandleFunc("/", handler)


    // When requesting the path "/metrics" we will output metrics in Prometheus format
    http.Handle("/metrics", promhttp.Handler())

    // start server on port 8080
    log.Fatal(http.ListenAndServe(":8080", nil))
}
```
Build project 
```bash
export GOPATH=$HOME/app
go build -o app main.go
```
Run
```
./main
```
fort test 
```bash
curl localhost:8080/test
curl localhost:8080/test/user/path
```





### Conncet Myapp to Prometheus


Edit file `prometheus.yml`
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

  - job_name: 'redis'
    static_configs:
      - targets: ['localhost:9121']

  - job_name: 'postgres'
    static_configs:
      - targets: ['localhost:9187']
  
  - job_name 'myapp'
    static_configs:
      - targets: ['localhost:8080']
```



### Pushgateway_Exporter from Binary

The Pushgateway is an intermediary service which allows you to push metrics from jobs which cannot be scraped. For details, see Pushing metrics.

> only use short programs like Crone or something end quickly


**[OPTIONS]**

`--web.config.file=""` - [EXPERIMENTAL] Path to configuration file that can enable TLS or authentication. See: https://github.com/prometheus/exporter-toolkit/blob/master/docs/web-configuration.md

`--web.listen-address=:9091` - Addresses on which to expose metrics and web interface. Repeatable for multiple addresse

`--web.telemetry-path="/metrics"` - Path where the metrics for Prometheus will be available.

`--web.enable-lifecycle` - Enable shutdown via HTTP request.

`--web.enable-admin-api` - Enable API endpoints for admin control actions.

`--persistence.file="/var/lib/prometheus/pushgateway.data"` - File to persist metrics. If empty, metrics are only kept in memory.

`--persistence.interval=5m` - The minimum interval at which to write out the persistence file.

Download 
```bash
cd /opt/
wget https://github.com/prometheus/pushgateway/releases/download/v1.6.2/pushgateway-1.6.2.linux-arm64.tar.gz
```

Unpack 
```bash
tar -xvf pushgateway-1.6.2.linux-arm64.tar.gz
```

Remove archive
```bash
rm -rf pushgateway-1.6.2.linux-arm64.tar.gz
```

Rename 
```bash
mv pushgateway-1.6.2.linux-arm64 pushgateway
```

Run (default port `127.0.0.1:9091`)

```bash
./pushgateway
```





### Configurate Pushgateway For Systemd

Create a new system user `pushgateway` 
```bash
useradd -s /sbin/nologin -d /opt/pushgateway pushgateway
```
ownership of the `/opt/pushgateway` directory to the user `pushgateway`.
```bash
chown -R pushgateway:pushgateway /opt/pushgateway
```
Copy && Past
```bash
sudo nano /etc/systemd/system/pushgateway.service
```
```ini
[Unit]
Description=Prometheus Pushgateway
Wants=network-online.target
After=network-online.target

[Service]
User=pushgateway
Group=pushgateway
Type=simple
ExecStart=/opt/pushgateway

[Install]
WantedBy=multi-user.target
```
Start and enable the pushgateway service:
```bash
sudo systemctl enable pushgateway
```

```bash
sudo systemctl start pushgateway
```
Verify the service is running and serving metrics:
```bash
sudo systemctl status pushgateway
```

```bash
curl localhost:9091/metrics
```


### Conncet Myapp to Prometheus


Edit file `prometheus.yml`
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

  - job_name: 'redis'
    static_configs:
      - targets: ['localhost:9121']

  - job_name: 'postgres'
    static_configs:
      - targets: ['localhost:9187']
  
  - job_name 'myapp'
    static_configs:
      - targets: ['localhost:8080']

  - job_name: 'Pushgateway'
    honor_labels: true
    static_configs:
      - targets: ['localhost:9091']  
```
> * Of interest here is the `honor_labels: true` parameter. Let's remember how Prometheus collects metrics. First of all, it accesses the exporter via http at the path `/metrics`, from where it takes all metrics with their tags. After that it adds its own internal tags - job, which is specified in the `job_name` configuration, as well as instance - endpoint from which these metrics were collected. In our case, we expose these tags ourselves in pushgateway. Since it can collect data from different services, we directly pass the job name and instance name to `db1`. So this option `honor_labels: true` prohibits Prometheus from replacing these tags with its own, but requires using the ones specified in pushgateway.So this option `honor_labels: true` disallows Prometheus to replace these tags with its own, but requires to use the ones specified in pushgateway.

Restart Prometheus to load the new configuration:
```bash
sudo systemctl restart prometheus
```


### Send Metrics to PushGateway

Sending basic metrics to pushgateway

First, it's worth mentioning that pushgateway has a built-in web interface, which can be accessed using port 9091 (default). We will use the console to work with it. So, pushgateway provides a simple API that you can use to send metrics to it. After receiving the metrics, pushgateway can give them to Prometheus server via the /metrics endpoint. Let's try to create our first metric using curl. Let's say our cron script runs once an hour and takes money from users' accounts. Then we can send information to pushgateway about the number of users processed. Or the total amount of debit:

```bash
echo "cron_app_processed_users 112" | curl --data-binary @- http://localhost:9091/metrics/job/cron_app

echo "cron_app_payed_sum 13423" | curl --data-binary @- http://localhost:9091/metrics/job/cron_app
```
In this example we sent two metrics - `cron_app_processed_users` - number of processed users with the value `112` and `cron_app_payed_sum` - total amount of debit in the amount of `13423`. We also used the `--data-binary` option, which does not modify the data passed to it from stdin in any way, but just passes it in the POST request.

we used the path `/metrics/job/cron_app`. In this case, `job` is a label, and `cron_app` is its value. Remember in `scrape_config` for Prometheus we set the job option, which was displayed in metrics as a tag? Actually, this is exactly that tag. Pushgateway will group your metrics by this tag. This is very useful when working with a large number of crowns or other short-lived tasks. You can see the grouped metrics by them.



```bash
curl http://localhost:9091/metrics
# TYPE cron_app_payed_sum untyped
cron_app_payed_sum{instance="",job="cron_app"} 13423
# TYPE cron_app_processed_users untyped
cron_app_processed_users{instance="",job="cron_app"} 112
```

Let's now try to see how our metrics will be given to Prometheus:

```bash
curl http://localhost:9091/metrics
# TYPE cron_app_payed_sum untyped
cron_app_payed_sum{instance="",job="cron_app"} 13423
# TYPE cron_app_processed_users untyped
cron_app_processed_users{instance="",job="cron_app"} 112

... skipped ...

# HELP push_failure_time_seconds Last Unix time when changing this group in the Pushgateway failed.
# TYPE push_failure_time_seconds gauge
push_failure_time_seconds{instance="",job="cron_app"} 0
# HELP push_time_seconds Last Unix time when changing this group in the Pushgateway succeeded.
# TYPE push_time_seconds gauge
push_time_seconds{instance="",job="cron_app"} 1.6208582543334155e+09

... skipped ...
```

Additional parameters when sending metrics :

In addition to the basic option of sending metrics, we can also use the advanced option - specifying the type of metric to be sent:
```bash
cat <<EOF | curl --data-binary @- http://localhost:9091/metrics/job/cron_app
# TYPE cron_app_payed_sum gauge
cron_app_payed_sum 15487
# TYPE cron_app_processed_users gauge
# HELP cron_app_processed_users Processed Users Counter.
cron_app_processed_users 238
EOF
```
In this example we sent the same metrics as before, but we specified their type in the comment. We will talk more about data types in Prometheus in the promql assignment, now the main thing is to understand that you can specify the type of data to be sent.

Another important feature when passing metrics is the ability to tag your metrics. Let's add a new metric to our application. For example, the number of users processed, categorized by groups.

For example our script processes two groups of users - corporate clients and individuals. We want to see how many users from one or the other group were processed. To do this, it is enough to send metrics in this format:

```bash
cat <<EOF | curl --data-binary @- http://localhost:9091/metrics/job/cron_app
cron_app_users{type="corp"} 64
cron_app_users{type="man"} 47
EOF

```
That is, we specified the tag directly in our metric. Let's see what happened with our metrics:

```bash
curl http://localhost:9091/metrics

# TYPE cron_app_payed_sum gauge
cron_app_payed_sum{instance="",job="cron_app"} 15487
# HELP cron_app_processed_users Processed Users Counter.
# TYPE cron_app_processed_users gauge
cron_app_processed_users{instance="",job="cron_app"} 238
# TYPE cron_app_users untyped
cron_app_users{instance="",job="cron_app",type="corp"} 64
cron_app_users{instance="",job="cron_app",type="man"} 47

... skipped ...

```
As you can see from the output, we have a new metric with different tags.


**Dividing metrics into groups**

In addition to adding labels directly in metrics, you can add tags via URLs. For example like this:

```
echo 'test_app_processed{type="users"} 12' | curl --data-binary @- http://localhost:9091/metrics/job/test_app/instance/db1
```
In this case, we added a new metric `test_app_processed` with three tags. One tag `type=users`, which was specified directly in the metric, the other two were specified via URL: `job=test_app` and `instance=db1`. Let's see how this metric will look like in Prometheus format:

```bash
curl -s http://localhost:9091/metrics

... skipped ...
# TYPE test_app_processed untyped
test_app_processed{instance="db1",job="test_app",type="users"} 12
```

Delete Metrics
```bash
curl -X DELETE http://localhost:9091/metrics/job/test_app/instance/db1
```
And all metrics that belonged to this group will be removed. To summarize, pushgateway grouping allows you to flexibly manage incoming metrics.















## Prometheus Service Discovery



**Manual Add Targets scripts**

Create dirctory for scripts
```bash
mkdir -p /opt/targets
```

```bash
cd /opt/targets
```

Now modify the config in `targets.json` by adding an entry for the new Node Exporter:
```json
[
  {
    "targets": [
      "localhost:9100"
    ],
    "labels": {
      "job": "node"
    }
  }
]
```

test config

```bash
cat targets.json | jq .
```

```yml
global:
  scrape_interval: 15s

scrape_configs:
  - job_name: 'prometheus'
    static_configs:
    - targets: ['localhost:9090']

  - job_name: 'node'
    scrape_interval: 5s
    static_configs:
      - targets: ['localhost:9100']

  - job_name: 'redis'
    static_configs:
      - targets: ['localhost:9121']

  - job_name: 'postgres'
    static_configs:
      - targets: ['localhost:9187']
  
  - job_name 'myapp'
    static_configs:
      - targets: ['localhost:8080']

  - job_name: 'Pushgateway'
    honor_labels: true
    static_configs:
      - targets: ['localhost:9091']
  
  - job_name: 'file'
    file_sd_config:
    - file:
      - '/opt/targets/*.json
      refresh_interval: 10s
```

Go on Prometheus Website `localhost:9090` Status => Server Discovery 



**DNS SRV(Service Record) method**




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

  - job_name: 'redis'
    static_configs:
      - targets: ['localhost:9121']

  - job_name: 'postgres'
    static_configs:
      - targets: ['localhost:9187']
  
  - job_name 'myapp'
    static_configs:
      - targets: ['localhost:8080']

  - job_name: 'Pushgateway'
    honor_labels: true
    static_configs:
      - targets: ['localhost:9091']
  
  - job_name: 'file'
    file_sd_config:
    - file:
      - '/opt/targets/*.json
      refresh_interval: 10s

  - job_name: 'dns'
    dns_sd_configs:
  - names:
    - _payment._tcp.api.prod.example.org'
    type: SRV
    refresh_interval: 120s
```

If you don't want to bother with SRV record detection, you can use regular A types. The only difference is that you will have to specify the port to which prometheus should connect. In SRV record the port is specified, but in A record there is no such field, so the configuration will take the following form:

```yaml
  - job_name: 'dns'
    dns_sd_configs:
    - names:
      - example.com
      type: A
      port: 9090
      refresh_interval: 10s
```



## PromQL

Prometheus there are only 4 types of metrics:

![prometheus3](images/prometheus3.png)


- **Counter** is a simple counter. Its peculiarity is that the value of this metric cannot decrease. For example, the `processed_requests` metric is the number of processed requests. Agree that it cannot decrease - with each new request it will increase by one.

- **Gauge** this type is responsible for a simple metric that can both increase and decrease. A good example of such a metric is temperature. At the moment it is 18 degrees, in a couple hours it will be 21 degrees, and at night it will drop to 10 degrees.

- **Histograms** Collects several data about a metric - the total number of values, their sum, as well as a breakdown by groups. It is most often used to collect temporal metrics (e.g. response time). All values are calculated on the server side.

- **Summaries** are essentially the same histograms, but all calculations are done on the client, so the client can eat up some percentage of CPU time. But they do not load the prometheus server, unlike Histograms.


**Data filtering**

```bash
./promtool query instant http://localhost:9090/ up

up{instance="localhost:8080", job="app"} => 1 @[1622717586.873]
up{instance="localhost:9090", job="prometheus"} => 1 @[1622717586.873]
```
In this case, we have output the latest values of the `up` metric. To filter the values - we can add the tags we want to see there to the queries:

```bash
./promtool query instant http://localhost:9090/ "up{job='app'}"

up{instance="localhost:8080", job="app"} => 1 @[1622717644.331]
```
In this way prometheus shows us all up metrics that have a job tag identical to app. Besides direct comparison you can also use negation and even `regexp`:
```bash
./promtool query instant http://localhost:9090/ "up{job!='prometheus'}"

up{instance="localhost:8080", job="app"} => 1 @[1622717699.7]

./promtool query instant http://localhost:9090/ "up{job=~'..p'}"

up{instance="localhost:8080", job="app"} => 1 @[1622717711.238]
root@prom-test:/opt/prometheus#
```
By the way, in prometheus you can use the secret tag `__name__`, which contains the name of the metric. In this way you can output all metrics with the name you are interested in with one query:
```bash
./promtool query instant http://localhost:9090/ "{__name__=~'^up$'}"

up{instance="localhost:8080", job="app"} => 0 @[1622759105.491]
up{instance="localhost:9090", job="prometheus"} => 1 @[1622759105.491]
```
In addition to filtering by tags, you can also filter metrics by current values - for example, let's display all `up` metrics that have a value of `1`:
```bash
./promtool query instant http://localhost:9090/ "up == 1"

up{instance="localhost:8080", job="app"} => 1 @[1622717784.272]
up{instance="localhost:9090", job="prometheus"} => 1 @[1622717784.272]
```
And now all app metrics that have a metric value not equal to 1:

```bash
./promtool query instant http://localhost:9090/ "up != 1"

```
Everything is logical - the output is empty - we don't have such metrics. Or let's output all up metrics that have a value greater than 0:
```bash
./promtool query instant http://localhost:9090/ "up > 0"

up{instance="localhost:8080", job="app"} => 1 @[1622717832.933]
up{instance="localhost:9090", job="prometheus"} => 1 @[1622717832.933]
```

**Working with basic functions**

https://prometheus.io/docs/prometheus/latest/querying/functions/

*Instant Vectors* are multiple time series metrics that reflect a single value over a specific time, for example:
```bash
./promtool query instant http://localhost:9090/ "up"
up{instance="localhost:8080", job="app"} => 1 @[1622719041.253]
up{instance="localhost:9090", job="prometheus"} => 1 @[1622719041.253]
```
> This displays the value of the up metric for all tags for the same timestamp - 1622719041.253.

*Range Vectors* are multiple time series metrics that reflect multiple values over a specific time interval, for example:
```bash
/promtool query instant http://localhost:9090/ "up[2m]"
up{instance="localhost:8080", job="app"} =>
1 @[1622718987.573]
1 @[1622719002.573]
1 @[1622719017.573]
1 @[1622719032.573]
1 @[1622719047.573]
1 @[1622719062.573]
1 @[1622719077.573]
1 @[1622719092.573]
up{instance="localhost:9090", job="prometheus"} =>
1 @[1622718989.926]
1 @[1622719004.926]
1 @[1622719019.926]
1 @[1622719034.926]
1 @[1622719049.926]
1 @[1622719064.926]
1 @[1622719079.926]
1 @[1622719094.926]
```
This displays the value of the up metric for the last two minutes. When using range vector, you specify the time period in square brackets. You can also use tag filtering in front of them:
```bash
./promtool query instant http://localhost:9090/ "up{job='app'}[2m]"
up{instance="localhost:8080", job="app"} =>
1 @[1622719047.573]
1 @[1622719062.573]
1 @[1622719077.573]
1 @[1622719092.573]
1 @[1622719107.573]
1 @[1622719122.573]
1 @[1622719137.573]
1 @[1622719152.573]
```
**Scalar** - simple floating point numbers. You will be able to use them, for example, by adding to or multiplying by metrics.

Now let's move on to the operators. In general, they are all intuitive:

- `+` addition

- `-` subtraction

- `*` multiplication

- `/` division

- `%` modulus

- `^` ascending

These operators work both between scalar values (5*5) and instant vectors, for example:

```bash
./promtool query instant http://localhost:9090/ "2 + 2"

scalar: 4 @[1622719951.177]

 ./promtool query instant http://localhost:9090/ "up + 1"

{instance="localhost:8080", job="app"} => 2 @[1622719953.617]
{instance="localhost:9090", job="prometheus"} => 2 @[1622719953.617]

./promtool query instant http://localhost:9090/ "up + up"

{instance="localhost:8080", job="app"} => 2 @[1622719955.02]
{instance="localhost:9090", job="prometheus"} => 2 @[1622719955.02]
```

>In the first example, we add just two scalar values. In the second example we add a scalar value of 1 to the up metric. And in the third example we add two instant vectors.

In addition to standard additions there are also aggregation operators - they allow to aggregate values of one instant vector by different tags - for example, to summarize the number of requests between all hosts with the tag `env=dev`. Here are the main ones:

- `sum` - summation
- `min` - take the minimum value
- `max` - take the maximum value
- `avg` - take average value
- `count` - count the number of elements in the vector

Let's try to summarize all the values of all the up metrics:
```bash
./promtool query instant http://localhost:9090/ "sum(up)"
{} => 2 @[1622720202.092]
```
> In this case prometheus took all the up metrics, discarded all the tags and summed their values - the result is 2.

You can do a job-by-job breakdown:
```bash
./promtool query instant http://localhost:9090/ "sum by (job) (up)"
{job="app"} => 1 @[1622720257.153]
{job="prometheus"} => 1 @[1622720257.153]
```
> In this case, prometheus took the up metric, grouped them by `job` tag and summed up the values in each group. Since we have only one metric in each group (`app` and `prometheus`), the values are identical to the original ones.

**Working with Functions in Prometheus**

The `rate` function takes a range vector as input and then performs a simple conversion - it takes the difference between the last and the first value and divides it by the number of seconds in the interval - so you get the counter growth rate. This function works only with the counter data type. Let's look at an example:
```bash
./promtool query instant http://localhost:9090/ "prometheus_http_requests_total{}"

prometheus_http_requests_total{code="200", handler="/api/v1/query", instance="localhost:9090", job="prometheus"} => 27 @[1622721465.532]
prometheus_http_requests_total{code="200", handler="/metrics", instance="localhost:9090", job="prometheus"} => 366 @[1622721465.532]
prometheus_http_requests_total{code="400", handler="/api/v1/query", instance="localhost:9090", job="prometheus"} => 9 @[1622721465.532]

 ./promtool query instant http://localhost:9090/ "rate(prometheus_http_requests_total{}[2m])"

{code="200", handler="/api/v1/query", instance="localhost:9090", job="prometheus"} => 0 @[1622721473.43]
{code="200", handler="/metrics", instance="localhost:9090", job="prometheus"} => 0.06666666666666667 @[1622721473.43]
{code="400", handler="/api/v1/query", instance="localhost:9090", job="prometheus"} => 0 @[1622721473.43]
```
> The first time we just asked for our metrics and the second time we asked prometheus to calculate the average number of requests for the last two minutes. Prometheus took the current value of the `prometheus_http_requests_total` metric and then subtracted the value of the `prometheus_http_requests_total` metric that was 2 minutes ago and then divided the obtained value by 120 seconds (2 minutes). Thus we got the average number of requests per second.

We can also use the `sum` operator to summarize the number of requests for all the same instances:
```bash
./promtool query instant http://localhost:9090/ "sum by (instance) (rate(prometheus_http_requests_total{}[2m]))"

{instance="localhost:9090"} => 0.06666666666666667 @[1622721815.76]
```
> Everything is the same here - we got the number of requests per second using rate, and after that we added up our metrics by instance tag - all those who had it matched were added up. In this way you can output, for example, the number of requests per second on all `dev` and `prod` servers with a simple query.

In addition to the rate function, there is the `irate` function - it does the same thing, but it takes the difference of the last and penultimate value instead of the first and last value. That is, it does not care what interval you specify - `[2m]` or `[10m]` - it will take the last and penultimate values for calculation


The next function in our list is `delta`. It works only with the gauge metrics type and calculates the difference of the first and the last value in the range vector. For example:
```bash
./promtool query instant http://localhost:9090/ "delta(prometheus_tsdb_blocks_loaded[2m])"

{instance="localhost:9090", job="prometheus"} => 0 @[1622722060.54]
```
> So `delta` will take the last value of the `prometheus_tsdb_blocks_loaded` metric and subtract from it the value of the `prometheus_tsdb_blocks_loaded` metric from 2 minutes ago.

Another interesting function for gauge metrics is `predict_linear` - it predicts the value of a metric for a given number of seconds ahead. Linear regression is used. That is, in essence, a straight line is drawn using the average values in the past and future values are predicted on its basis:
```bash
./promtool query instant http://localhost:9090/ "predict_linear(prometheus_tsdb_blocks_loaded[120m], 120)"

{instance="localhost:9090", job="prometheus"} => 1.0048419915591933 @[1622722310.225]
```
> In this case we took the value of `prometheus_tsdb_blocks_loaded` metric for the last 2 minutes and tried to predict the value for 120 seconds in the future. This function is very useful to use with the available disk space - it will be able to predict when the disk space will end.

let's take a look at the function that is designed to work with histograms. Since summaries are counted immediately on the client, this function does not apply to summaries. Let's try to calculate 85% persistence for prometheus' http response time:
```bash
./promtool query instant http://localhost:9090/ "histogram_quantile(0.85, prometheus_http_request_duration_seconds_bucket)"

{handler="/metrics", instance="localhost:9090", job="prometheus"} => 0.085 @[1622722586.628]
{handler="/api/v1/query", instance="localhost:9090", job="prometheus"} => 0.085 @[1622722586.628]
```
In this case, we use the histogram_quantile function with a parameter of 0.85 (that's 85%) and specifying the histogram metric to use - `prometheus_http_request_duration_seconds_bucket`. You can also calculate the value on a time segment by adding the rate function:

```bash
root@prom-test:/opt/prometheus# ./promtool query instant http://localhost:9090/ "histogram_quantile(0.85, rate(prometheus_http_request_duration_seconds_bucket[10m]))"

{handler="/api/v1/query", instance="localhost:9090", job="prometheus"} => 0.085 @[1622722648.458]
{handler="/metrics", instance="localhost:9090", job="prometheus"} => 0.085 @[1622722648.458]
```
> This calculates the same metric, but over a 10 minute interval.






## Before install Alertmanager (Alerts of Prometheus)

### Customizing  Rules Alerts in Prometheus

To configure alerts in promethues you will need to specify the `rule_files` parameter in the main config file `prometheus.yml`, which is responsible for which file prometheus will look for alerting rules. 
```bash
alerting:
  alertmanagers:
  - static_configs:
    - targets:
      - alertmanager:9093

rules_files:
  - "alerts.yml"
```

After that we can create an `alerts.yml` file (it should be in the same directory as prometheus.yml) with our host availability alert:
```bash
nano alerts.yml
```
```yml 
groups:
- name: instances
  interval: 10s
  rules:
  - alert: InstanceDown
    expr: up == 0
    for: 1m
    labels:
      severity: critical
    annotations:
      summary: "Instance {{ $labels.instance }} down"
      description: "{{ $labels.instance }} of job {{ $labels.job }} has been down for 1 minute."
```
> here we use go-template https://pkg.go.dev/text/template

> for more alerts can take for here https://samber.github.io/awesome-prometheus-alerts/


Let's turn off our application that was polled by prometheus and look at the current alerts:
```bash
./promtool query instant http://localhost:9090/ "ALERTS{}"

ALERTS{alertname="InstanceDown", alertstate="firing", instance="localhost:8080", job="app", severity="critical"} => 1 @[1622757949.277]
```
>  there is a special ALERTS metric in prometheus that allows you to see all current alerts. In our case we can see alert `alertname=InstanceDown` and its tags. Please note that all tags of the triggered metric are added to the alert


### Testing of Alerting Rules


```bash
# Путь к файлу с алертами
rule_files:
    - alerts.yml

# Как часто будут выполняться запросы из тестовых правил
evaluation_interval: 10s

tests:
    - interval: 15s # Как часто собираются данные по нашей метрике
      input_series: # Тестовая метрика
          - series: 'up{job="prometheus", instance="localhost:9090"}' # Название и теги тестовой метрики
            values: '0 0 0 0 0 0 0 0 0 0 0 0 0 0 0' # Значения тестовой метрики - каждое значение по факту будет с
                                                    # обираться каждые interval секунд. То есть в нашем примере
                                                    # interval 15 sec, значит при тестах первое значение - ноль
                                                    # - будет собрано в самом начале, когда прошло 0 секунд, 
                                                    # второе значение - ноль - будет собрано в 15 секунд, 
                                                    # третье значение - ноль - появится в 30 секунд и так 
                                                    # далее. Это важно, поскольку вам так же захочется 
                                                    # тестировать крайние случаи - когда for и evaluation_interval
                                                    # и scrape_interval отличаются и алерт может не сработать.

      # Тестовые правила
      alert_rule_test:
          - eval_time: 5m # Время прошедшее с момента запуска теста - за это 
                          # время тесты соберут все значения метрики
                          # (раз в 15 секунд, а так же будут раз в 10 секунд
                          # выполнять наши правила алертинга)
            alertname: InstanceDown # Названия алерта, которое тестируем
            exp_alerts: # что ожидаем получить на выходе
                - exp_labels: # Какие лейблы должны быть у алерта
                      severity: critical
                      instance: localhost:9090
                      job: prometheus
                  exp_annotations: # Какие аннотации должны быть у алерта
                      summary: "Instance localhost:9090 down"
                      description: "localhost:9090 of job prometheus has been down for 1 minute."
```
Try running our tests:
```bash
root@prom-test:/opt/prometheus# ./promtool test rules test.yaml

Unit Testing:  test.yaml
  SUCCESS
```

### Standard alerts for systems

The example will be about disk space:
```yml
- alert: HostOutOfDiskSpace
    expr: (node_filesystem_avail_bytes * 100) / node_filesystem_size_bytes < 10 and ON (instance, device, mountpoint) node_filesystem_readonly == 0
    for: 2m
    labels:
      severity: warning
    annotations:
      summary: Host out of disk space (instance {{ $labels.instance }})
      description: "Disk is almost full (< 10% left)\n  VALUE = {{ $value }}\n  LABELS = {{ $labels }}"
```

Same example with disk space, but only using the `predict_linear` function, which predicts the value in 24 hour
```yml
 - alert: HostDiskWillFillIn24Hours
    expr: (node_filesystem_avail_bytes * 100) / node_filesystem_size_bytes < 10 and ON (instance, device, mountpoint) predict_linear(node_filesystem_avail_bytes{fstype!~"tmpfs"}[1h], 24 * 3600) < 0 and ON (instance, device, mountpoint) node_filesystem_readonly == 0
    for: 2m
    labels:
      severity: warning
    annotations:
      summary: Host disk will fill in 24 hours (instance {{ $labels.instance }})
      description: "Filesystem is predicted to run out of space within the next 24 hours at current write rate\n  VALUE = {{ $value }}\n  LABELS = {{ $labels }}"
```

The following alert will help you quickly locate heavily loaded servers:
```yml
- alert: HostHighCpuLoad
    expr: 100 - (avg by(instance) (rate(node_cpu_seconds_total{mode="idle"}[2m])) * 100) > 80
    for: 0m
    labels:
      severity: warning
    annotations:
      summary: Host high CPU load (instance {{ $labels.instance }})
      description: "CPU load is > 80%\n  VALUE = {{ $value }}\n  LABELS = {{ $labels }}"
```

The amount of free memory:
```yml
- alert: HostOutOfMemory
    expr: node_memory_MemAvailable_bytes / node_memory_MemTotal_bytes * 100 < 10
    for: 2m
    labels:
      severity: warning
    annotations:
      summary: Host out of memory (instance {{ $labels.instance }})
      description: "Node memory is filling up (< 10% left)\n  VALUE = {{ $value }}\n  LABELS = {{ $labels }}"
```

This example, the alert is triggered if the network interface receives more than 100 mb of traffic per second. You can always adjust this value to suit your requirements.
```yml
  - alert: HostUnusualNetworkThroughputIn
    expr: sum by (instance) (rate(node_network_receive_bytes_total[2m])) / 1024 / 1024 > 100
    for: 5m
    labels:
      severity: warning
    annotations:
      summary: Host unusual network throughput in (instance {{ $labels.instance }})
      description: "Host network interfaces are probably receiving too much data (> 100 MB/s)\n  VALUE = {{ $value }}\n  LABELS = {{ $labels }}"
```

## Alertsmanager 

### Alertsmanager from Binary

```bash
cd /opt
wget https://github.com/prometheus/alertmanager/releases/download/v0.26.0/alertmanager-0.26.0.linux-amd64.tar.gz
```
Unpack
```bash
tar zxf alertmanager-0.26.0.linux-amd64.tar.gz
```
Rename
```bash
mv alertmanager-0.26.0.linux-amd64 alertmanager
```
```bash
rm -rf alertmanager-0.26.0.linux-amd64.tar.gz
```
Move to alermanager directory 
```bash
cd alertmanager
```
Run (default port `localhost:9093`)
```bash
./alertmanager
```
```bash
./alertmanager --config.file=alertmanager.yml
```

### Alertmanager Systemd 


```bash
useradd -s /sbin/nologin -d /opt/alertmanager alertmanager
```
Change ownership
```bash
chown alertmanager:alertmanager -R /opt/alertmanager
```
Create systemd unit file
```bash
nano /etc/systemd/system/alertmanager.service
```

```ini
[Unit]
Description=Alertmanager
Wants=network-online.target
After=network-online.target

[Service]
User=alertmanager
Group=alertmanager
Type=simple
WorkingDirectory=/opt/alertmanager/
ExecStart=/opt/alertmanager --config.file=/opt/alertmanager/alertmanager.yml --web.external-url http://0.0.0.0:9093

[Install]
WantedBy=multi-user.target
```

Run service (website on localhost:9093 => `Alerts`)
```bash
systemctl daemon-reload
```
website on localhost:9093 => `Alerts`
```bash
systemctl start alertmanager
```
```bash
systemctl enable alertmanager
```
```bash
systemctl status alertmanager
```

### Basic Alertmanager Configuration



That's actually the whole installation. Now let's discuss the main features of alertmanager in more detail:

- Grouping. This is a very important point - imagine that one node on your network crashed. And a bunch of alerts were triggered on it at once - node_exporter unavailability, mysql unavailability and so on. With the help of grouping you can group alerts into one group and send only one notification, not dozens or hundreds.

- Inhibition. One more important thing about alertmanager is that it allows you not to send a new group of alerts if some of the specified alerts have already been sent. From the example above - you may not send a new alert that the site is unavailable, if you already have an alert that the database is unavailable - because most likely it is the root of all problems.

- Silences. The ability to stop sending alerts for a period of time - for example, a weekend for a dev server or all alerts for a period of system maintenance.

- High Availability - alertmanager can be run in cluster mode to ensure high availability. Prometheus will send information about alerts to all available alertmanagers, and they in turn will select a leader that will directly handle them. In case of leader's failure - he will be replaced by another available alertmanager. This way you will not miss any alert.









Create `alertmanager.yml` file
```bash
nano /opt/alertmanager/alertmanager.yml
```
Pagerduty and Slack
```yml 
route:
  group_by: ['alertname']
  group_wait: 30s
  group_interval: 5m
  repeat_interval: 1h
  receiver: 'web.hook'
  routes:
  - receiver: 'pagerduty-critical'
    group_wait: 10s
    matchers:
    - severity="critical"
  - receiver: 'slack-warning'
    group_wait: 60s
    matchers:
    - severity="warning"

receivers:
- name: 'web.hook'
  webhook_configs:
  - url: 'http://127.0.0.1:5001/'
- name: 'pagerduty-critical'
  pagerduty_configs:
  - routing_key: 'test'
    service_key: 'md5-hash'
- name: 'slack-warning'
  slack_configs:
  - api_url: 'https://hooks.slack.com/services/TOKEN'
    channel: '#warnings'


inhibit_rules:
  - source_matchers:
    - severity="critical"
    target_matchers:
    - severity="warning"
    equal: ['instance']
```

### Test Configuration of Alertmanager

Here it is worth mentioning an interesting utility `amtool` - it allows, for example, to check the alertmanager configuration for correctness. In fact, it is similar to the `promtool` utility for prometheus. examples:

```bash
./amtool check-config alertmanager.yml
Checking 'alertmanager.yml'  SUCCESS
Found:
 - global config
 - route
 - 1 inhibit rules
 - 3 receivers
 - 0 templates
```
Test routing for your Alerts:
```bash
amtool config routes --config.file alertmanager.yml

Routing tree:
.
└── default-route  receiver: web.hook
    ├── {severity="critical"}  receiver: pagerduty-critical
    └── {severity="warning"}  receiver: slack-warning
```
```bash
./amtool config routes test severity="warning" --config.file alertmanager.yml

slack-warning
```
```bash
./amtool config routes test severity="info" --config.file alertmanager.yml

web.hook
```

### Connect Alertmanager to Prometheus

The only need change prometheus `prometheus.yml`:

```yml
# my global config
global:
  scrape_interval:     15s # Set the scrape interval to every 15 seconds. Default is every 1 minute.
  evaluation_interval: 15s # Evaluate rules every 15 seconds. The default is every 1 minute.
  # scrape_timeout is set to the global default (10s).

# Alertmanager configuration
alerting:
  alertmanagers:
  - static_configs:
    - targets:
      - localhost:9093
```




### Connecting Notifications in Telegram

In fact, from alertmanager's point of view everything is simple - we just need to send a **Webhook** to **Telegram Api** to send notification. But on the telegram side, we have a few steps to take. Let's go through them all:

- First, you need to create a new bot. To do this, you should turn to the bot `@BotFather`. Directly type this nickname in the Telegram search and then type the command `/newbot`. Specify its name - for example `rebrainme_notify`, its nickname (it must end with bot) - `rebrainme_notify_bot`. In response `botFather` will give you an API key to access the bot using API. Save this token - we will use it further as `$TOKEN`.

- Find your bot by searching for the nickname - `rebrainme_notify_bot` and send it a message - to do this, first press the start command and then write anything. Yes, it won't reply, but we don't need to - we just need to know our `chat_id`, which appeared when we sent it a message.


To find out the `chat_id`, you can send an http request using curl:
```bash
$ curl -s https://api.telegram.org/bot$TOKEN/getUpdates | jq .
{
  "ok": true,
  "result": [
    {
      "update_id": 736752606,
      "message": {
        "message_id": 2,
        "from": {
          "id": 113317645,
          "is_bot": false,
          "first_name": "Vasiliy",
          "last_name": "Ozerov",
          "username": "vasiliyozerov",
          "language_code": "en"
        },
        "chat": {
          "id": 113317645,
          "first_name": "Vasiliy",
          "last_name": "Ozerov",
          "username": "vasiliyozerov",
          "type": "private"
        },
        "date": 1623194263,
        "text": "gfhjkm"
      }
    }
  ]
}
```
> nstead of `$TOKEN` you need to substitute your token, which was given to you by botFather in the first step. And you can get your `chat_id` - in our case it is `113317645`.


Now you can send messages through the bot using api requests, using the bot's `$TOKEN` and the `chat_id` obtained in the previous step. Let's try sending a test message:

```bash
curl -s 'https://api.telegram.org/bot$TOKEN/sendMessage?chat_id=113317645&text=Heya'

{"ok":true,"result":{"message_id":3,"from":{"id":1765727458,"is_bot":true,"first_name":"rebrainme_notify","username":"rebrainme_notify_bot"},"chat":{"id":113317645,"first_name":"Vasiliy","last_name":"Ozerov","username":"vasiliyozerov","type":"private"},"date":1623194449,"text":"Heya"}}
```

### Configure alertmanager

The official configuration documentation is on https://prometheus.io/docs/alerting/latest/configuration

For my tests, I added these 2 snippets.
```bash
route:
  - match:
      severity: test
    receiver: test-telegram
receivers:
 
- name: 'test-telegram'
  telegram_configs:
  - bot_token: YOUR_BOT_TOKEN
    api_url: https://api.telegram.org
    chat_id: YOUR_CHAT_ID
    parse_mode: ''
```
If you need a proxy, add the `http_config` section below:

```bash
receivers:
- name: 'stardata-telegram'
  telegram_configs:
  - bot_token: YOUR_BOT_TOKEN
    api_url: https://api.telegram.org
    chat_id: YOUR_CHAT_ID
    parse_mode: ''
    http_config:
      proxy_url: 'http://your-proxy-server-if-required:3128'
```

**Test the configuration**

To create a temporary alert to test the configuration, run 
```bash
amtool --alertmanager.url=http://localhost:9093/ alert add alertname="test123" severity="test-telegram" job="test-alert" instance="localhost" exporter="none" cluster="test"
```






## Prometheus Federation

**High Availability**
First things first, let’s tackle the question of high availability.
Unlike Alertmanager, which allows clustering and communication between multiple instances, Prometheus doesn’t extend the same privilege. However, the truth is, clustering Prometheus is not needed to achieve high availability. This tool has been ingeniously engineered to maintain high availability without requiring server communication.

In fact, setting up a highly available Prometheus system is a breeze just by running multiple Prometheus servers with identical configurations.
![prometheus8](images/prometheus8.svg)

Without further ado, I’ll explain how to set up this HA in a few steps, if you don’t mind.

Let’s assume that we have two Prometheus servers, `Prometheus_DC1` and `Prometheus_DC2`.

To set up the HA, we have to copy all Prometheus configurations from Prometheus_DC1 to Prometheus_DC2 :

- Copy the Prometheus configuration file `/etc/prometheus/prometheus.yml` from the `Prometheus_DC1` to `Prometheus_DC2`.

- Copy the rules configuration `/etc/prometheus/rules/rules.yaml` from the `Prometheus_DC1` to `Prometheus_DC2`

- On `Prometheus_DC2`, enable the Prometheus service, and start it.
```
sudo systemctl enable prometheus
sudo systemctl start prometheus
```

And that’s it. We don’t need anything else to configure HA at the Prometheus level. If we wish, we can add a load balancer configured in round-robin. It can be helpful if we’re using Grafana.



**Federation**

When we start monitoring with Prometheus, we typically begin with one server per datacenter or environment, which is sufficient due to its efficiency and lower failure rates. However, as operational overhead and performance issues arise, it may be necessary to split the Prometheus server into separate servers for network, infrastructure, and application monitoring, known as vertical sharding. Teams can also run their own Prometheus servers for greater flexibility and control over target labels and scrape intervals. Social factors often lead to Prometheus servers being split before performance concerns arise.

But how do we aggregate all this data?
To perform global aggregations, federation allows for a global Prometheus server to pull aggregated metrics from datacenter Prometheus servers. This ensures reliability and simplicity in monitoring systems, particularly for graphing and alerting purposes.

We can easily share our metric data with Federation across different Prometheus servers.

For informational purposes, there are two types of Federation

- Hierarchical Federation
This means we have bigger Prometheus servers that collect time-series data from smaller ones. We have a top-down approach where data is gathered from different levels.


![prometheus7](images/prometheus7.svg)

- Cross-Service Federation

This method involves one Prometheus server monitoring a particular service or group of services, gathering specific time-series data from another server that is monitoring a different set of services.
By doing this, we can run queries and alerts on the merged data from both servers.



![prometheus9](images/prometheus9.svg)


**Implementing a hierarchical Federation**

For this blog, we will assume a configuration with two servers to be monitored – one in Basel and the other in Zurich, each hosting a node_exporter.
These servers will be monitored by dedicated Prometheus servers named dc1 and dc2, respectively. Finally, the metrics from these two Prometheus servers will be aggregated by a federation server.

![prometheus](images//prometheus10.svg)

From the Prometheus user interface, we can observe that the “Office Server” jobs gather metrics from the Basel and Zurich servers through their respective endpoints `http:/dbi-basel-srv1:9100/metrics` and `http:/dbi-zurich-srv1:9100/metrics`, via the Prometheus servers `DC1` and `DC2`.

Prometheus DC1:
![prometheus11](images/prometheus11.png)


Prometheus DC2:
![prometheus12](images/prometheus12.png)

Now, we can log in to our third Prometheus server (the Federate one) and edit the Prometheus configuration file `/etc/prometheus/prometheus.yml`.

Add the following block at the bottom of the file, just after the static configuration of the “Prometheus” job name.
```bash
# Federation configuration
  - job_name: 'federation'
    scrape_interval: 15s
    
    honor_labels: true
    metrics_path: '/federate'
    
    params:
      'match[]':
        - '{job!~"prometheus"}'
        - '{__name__=~"job:.*"}'
    
    static_configs:
    - targets:
      - 'prometheus-dc-1:9090'
      - 'prometheus-dc-2:9090'
```
check config
```bash
./promtool check config ./prometheus.yml
```
Once configured, we enable the Federal Prometheus Server
```bash
sudo systemctl enable prometheus
```
And start Prometheus
```bash
sudo systemctl start prometheus
```
> We can access Federal Prometheus Server through the web UI at http://<Federated Prometheus Server Public IP>:9090.

Then, click on status and finally on target.

![prometheus12](images/prometheus13.png)




### Creating new metrics via recording rules
Let's look at a simple example. First, let's configure the aggregation rules on the local prometheus host. We haven't analyzed the aggregation rules with you, but in fact they are very similar to alerting rules - they will be executed once in a given interval and will essentially generate a new metric based on your request. This way you can create new metrics that will be stored in prometheus.

So we have a local prometheus that collects data from all nodes in the datacenter via a simple node_exporter. Let's take a look at its configuration:

So we have a local prometheus that collects data from all nodes in the datacenter via a simple `node_exporter`. Let's take a look at its configuration:
```yml
global:
  scrape_interval:     15s
  evaluation_interval: 15s
  external_labels:
    datacenter: dc1

rule_files:
  - "rules.yml"

scrape_configs:
  - job_name: 'prometheus'
    static_configs:
    - targets: ['localhost:9090']

  - job_name: 'node'
    static_configs:
    - targets: ['localhost:9100']
```
Check config
```bash
./promtool check config ./prometheus.yml
```

> In addition to `external_labels`, we specified a file with rules - `rules.yml`. In fact, we already did this when we added the rules for alerting, but now we will add the rules for creating new metrics. So, let's have a look at rules.yml:

rules.yml
```yml
groups:
  - name: globaldc
    interval: 5s
    rules:
    - record: job:node_memory_MemTotal_bytes:sum
      expr: sum without(instance)(node_memory_MemTotal_bytes{job="node"})
```

```bash
systemctl restart prometheus
```

test new metric
```bash
./promtool query instant http://localhost:9090/ 'job:node_memory_MemTotal_bytes:sum'
```


## Long Term Storage for Prometheus

### How TSDB works (short time storage)
All metrics that prometheus receives from exporters are grouped into two-hour blocks and saved to disk. One block is a separate directory, which contains the data itself (chunks), a metadata file, and an index file, which contains information on all metrics available for a given time period, as well as all labels of these metrics. The metrics themselves within the chunks directory are grouped into files up to 512 MB in size

```bash
root@prom-test:/opt/prometheus/data# ls -la
total 24
drwxr-xr-x 5 prometheus prometheus  4096 Jun 16 07:52 .
drwxr-xr-x 5 prometheus prometheus  4096 Apr 30 08:03 ..
drwxr-xr-x 3 prometheus prometheus  4096 Jun 16 07:52 01F89WWE7DY9X2X3Q2E47VAA8C
drwxr-xr-x 2 prometheus prometheus  4096 Jun 16 07:52 chunks_head
-rw-r--r-- 1 prometheus prometheus     0 Apr 30 08:03 lock
-rw-r--r-- 1 prometheus prometheus 20001 Jun 16 07:51 queries.active
drwxr-xr-x 2 prometheus prometheus  4096 Jun 16 07:52 wal
root@prom-test:/opt/prometheus/data# ls -la 01F89WWE7DY9X2X3Q2E47VAA8C/
total 68
drwxr-xr-x 3 prometheus prometheus  4096 Jun 16 07:52 .
drwxr-xr-x 5 prometheus prometheus  4096 Jun 16 07:52 ..
drwxr-xr-x 2 prometheus prometheus  4096 Jun 16 07:52 chunks
-rw-r--r-- 1 prometheus prometheus 45866 Jun 16 07:52 index
-rw-r--r-- 1 prometheus prometheus   272 Jun 16 07:52 meta.json
-rw-r--r-- 1 prometheus prometheus     9 Jun 16 07:52 tombstones
root@prom-test:/opt/prometheus/data#
```
The metrics that prometheus currently receives from exporters do not immediately get to disk - they are stored in memory for some time. To protect against unexpected termination, write ahead log is used, thanks to which prometheus can run the history of metrics retrieval again at startup and save them to disk in the necessary directory. WAL logs are stored in the wal directory and occupy a maximum of 128 MB of space each:
```bash
root@prom-test:/opt/prometheus/data# ls -la wal/
total 100
drwxr-xr-x 2 prometheus prometheus  4096 Jun 16 07:52 .
drwxr-xr-x 5 prometheus prometheus  4096 Jun 16 07:52 ..
-rw-r--r-- 1 prometheus prometheus 32768 Apr 30 08:03 00000000
-rw-r--r-- 1 prometheus prometheus 32768 Jun 16 07:52 00000001
-rw-r--r-- 1 prometheus prometheus 25704 Jun 16 07:55 00000002
```
After writing to wal, prometheus performs a compaction operation in the background and stores the resulting metrics into the desired data blocks. In addition, initial two-hour data blocks can be grouped into larger blocks - usually up to 10% of retention time. That is, if you set 30 days to store metrics, the blocks may contain 3 days of data.

The only limitation of this storage is that it cannot be clustered or replicated, but it is very fast. The developers claim that you will be able to store a year's worth of metrics in this engine. It is worth noting that you should take care of prometheus failover by using a raid array or migrated virtual machines, otherwise you may lose all your data if a hard disk fails.


Basic TSBD settings

- `--storage.tsdb.path` - path where tsdb data will be located. If you run prometheus in docker, it makes sense to mount this directory on the host system.

- `--storage.tsdb.retention.time` - storage time of collected metrics. By default metrics are retained for 15 days.

- `--storage.tsdb.retention.size` - experimental function to clean up metrics based on the size they take.

- `--storage.tsdb.wal-compression` - enable wal archive compression. Enabled by default since version 2.20.

- `--storage.tsdb.no-lockfile` - do not create lockfile in data directory. It will allow to run the second prometheus on the same data. But data consistency cannot be guaranteed anymore.


### Settings Long Term Storage.

This magic works thanks to the standardized API offered by prometheus. In fact, the scheme works as follows - prometheus accesses an external application using its api, which in turn converts the request into the required format and sends the request to the storage - for example, to postgresql or clickhouse. Then it receives the response, converts it back into prometheus format and sends it back. Thus the whole scheme is very similar to the scheme with exporters. Prometheus contacts the adapter, and the adapter already interacts with the storage. An example of such an adapter can be seen here(https://github.com/prometheus/prometheus/tree/release-2.27/documentation/examples/remote_storage/example_write_adapter).


https://prometheus.io/docs/operating/integrations/#remote-endpoints-and-storage





### Kafka

So, let's start by installing zookeeper, since kafka uses it:

```bash
apt-get install zookeeper zookeeper-bin zookeeperd
```

Now we can configure it and add the id of our first zookeeper:

```bash
echo 1 > /etc/zookeeper/conf/myid
```
```bash
systemctl start zookeeper
```

```bash
systemctl status zookeeper
```
```bash
zookeeper.service - LSB: centralized coordination service
     Loaded: loaded (/etc/init.d/zookeeper; generated)
     Active: active (running) since Wed 2021-06-16 08:24:11 UTC; 18s ago
       Docs: man:systemd-sysv-generator(8)
      Tasks: 17 (limit: 2345)
     Memory: 37.2M
     CGroup: /system.slice/zookeeper.service
             └─11859 /usr/bin/java -cp /etc/zookeeper/conf:/usr/share/java/jline.jar:/usr/share/java/log4j-1.2.jar:/usr/share/java/xercesImpl.jar:/usr/share/java/xmlParserAPIs.j>
```

Install Kafka from binary 
```bash
cd /opt/
wget https://downloads.apache.org/kafka/3.6.1/kafka_2.13-3.6.1.tgz
```
Unpack
```bash
tar zxf kafka_2.13-3.6.1.tgz
```
```bash
mv afka_2.13-3.6.1 kafka
```
```bash
rm -rf kafka_2.13-3.6.1.tgz
```
```bash
cd kafka/
```
run kafka 
```bash
./bin/kafka-topics.sh --list --zookeeper localhost:2181
```

So, Kafka is installed - now we just need to get prometheus to write to it.

Great, we just need to check that everything works as it should - for this purpose we will create a new topic:
```bash
./bin/kafka-topics.sh --zookeeper localhost:2181 --topic "test" --create --partitions 5 --replication-factor 1
Created topic test.
./bin/kafka-topics.sh --list --zookeeper localhost:2181
test
```


### Configuring Prometheus to Write to Remote Storage

To connect prometheus, we need to run an adapter that will receive messages from prometheus and send them to kafka. To do this, let's use the officially recommended adapter - prometheus-kafka-adapter. It will accept messages from prometheus in its format, convert it to json and save it to kafka.

So, before installing the adapter, we will need to install the `librdkafka` dependencies. The official repositories contain an outdated version, so we will have to add a new repository:
```bash
wget -qO - https://packages.confluent.io/deb/6.2/archive.key | sudo apt-key add -
```

```bash
sudo add-apt-repository "deb [arch=amd64] https://packages.confluent.io/deb/6.2 stable main"
```

```bash
echo "deb http://security.ubuntu.com/ubuntu bionic-security main" | sudo tee -a /etc/apt/sources.list.d/bionic.list
```

```bash
 apt-get install librdkafka-dev librdkafka1 zlib1g zlib1g-dev libcrypto++-dev libcrypto++6 libssl-dev
```

install prom-kafka-adapter:
```bash
cd /opt/
wget https://go.dev/dl/go1.21.5.linux-amd64.tar.gz
```


```bash
 
export GOROOT=/opt/go
export PATH=/opt/go/bin:$PATH
go version
go version go1.21.5 linux/amd64
```

```bash
mkdir prom-kafka-adapter
```
```bash
cd prom-kafka-adapter/
```
```bash
export GOPATH=`pwd`
```
```bash
go get github.com/Telefonica/prometheus-kafka-adapter@1.9.0
```

check that everything works:
```bash
KAFKA_BROKER_LIST=localhost:9092 ./bin/prometheus-kafka-adapter
```

Only left to configure prometheus. For this we add to `prometheus.yml`:
```yml
global:
  scrape_interval:     15s
  evaluation_interval: 15s

scrape_configs:
  - job_name: 'prometheus'
    static_configs:
    - targets: ['localhost:9090']

remote_write:
  - url: "http://localhost:8080/receive"
```


```bash
systemctl restart prometheus
```

```bash
./bin/kafka-topics.sh --list --zookeeper localhost:2181
```
```bash
./bin/kafka-console-consumer.sh --from-beginning --bootstrap-server localhost:9092 --topic metrics
```
> Prometheus throws all our metrics into prom-kafka-adapter, and that in turn into kafka, in json format. 








## Firewall and SSL/TCL

iptables firewall for prometheus
```bash
cat /etc/iptables/rules.v4
# Generated by iptables-save v1.4.21 on Tue Jun 23 15:46:13 2020
*nat
:PREROUTING ACCEPT [17566285:1114295190]
:INPUT ACCEPT [8292701:445269860]
:OUTPUT ACCEPT [12857064:888777529]
:POSTROUTING ACCEPT [12857064:888777529]
COMMIT
*filter
:INPUT DROP [4:176]
:FORWARD ACCEPT [0:0]
:OUTPUT ACCEPT [67101:106192188]
-A INPUT -i lo -j ACCEPT
-A INPUT -p tcp -m tcp --dport 22 -j ACCEPT
-A INPUT -m state --state RELATED,ESTABLISHED -j ACCEPT
COMMIT
```

### Configuring Prometheus SSL

In our example we will use a self-signed certificate. First we need to generate the root key and certificate so that we can issue certificates for prometheus and node_exporter. This can be done using the openssl utility:


```bash
cd /opt/
mkdir ssl
```
```bash
cd ssl
```
```bash
openssl genrsa -out ca.key 4096
```
```bash
openssl req -new -x509 -days 720 -key ca.key -out ca.crt
Country Name (2 letter code) [AU]:IL
State or Province Name (full name) [Some-State]:Center
Locality Name (eg, city) []:Moscow
Organization Name (eg, company) [Internet Widgits Pty Ltd]:Mycompany
Organizational Unit Name (eg, section) []:IT
Common Name (e.g. server FQDN or YOUR name) []:myweb.com
Email Address []:admin@myweb.com

Please enter the following 'extra' attributes
to be sent with your certificate request
A challenge password []:
An optional company name []:
```
```bash
ls -la
```
> Here we created a private key `ca.key`, as well as a certificate `ca.crt`, in which we added general information about us.

Now all that's left is to generate a server certificate and key for Prometheus, and then sign it with the root certificate:
```bash
openssl genrsa -out server.key 4096

Country Name (2 letter code) [AU]:IL
State or Province Name (full name) [Some-State]:Center
Locality Name (eg, city) []:Tel-aviv
Organization Name (eg, company) [Internet Widgits Pty Ltd]:Mycompany
Organizational Unit Name (eg, section) []:IT
Common Name (e.g. server FQDN or YOUR name) []:mywebsite.com
Email Address []:admin@mywebsite.com

```
```bash
openssl x509 -req -days 365 -in server.csr -CA ca.crt -CAkey ca.key -set_serial 1 -out server.crt
```


> We should end up with a `server.key` and a `server.crt` certificate


Now we can describe the TLS and basic auth settings in the `/opt/prometheus/web.config` file for our prometheus server:
```bash
tls_server_config:
  cert_file: /opt/ssl/server.crt
  key_file: /opt/ssl/server.key

http_server_config:
  http2: true

basic_auth_users:
  dan: $2a$10$j6bz1OjUBADJ.dx5CxZGA.KVMAsNvAEEmJdrk0W1LHlxQDyqXGte.
```
> In this example, we simply specified the server key and certificate, and enabled `http2` support and also set a login/password for our user dan. We obtained the password using `htpasswd`. You must use bcrypt encryption:

install htpasswd 
```bash
apt install apache2-utils
```
Generate password for `auth_users`:
```bash
htpasswd -bnBC 10 "" password | tr -d ':\n' | sed 's/$2y/$2a/'
```

Now let's assign permissions to our certificates, since prometheus is running under the prometheus user:
```bash
chown prometheus:prometheus /opt/ssl/server.*
```
Now let's add a new parameter to start (`--web.config.file`) where we describe our web.config in `/etc/systemd/system/prometheus.service`:
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
    --web.console.libraries=/opt/prometheus/console_libraries \
    --web.config.file=/opt/prometheus/web.config

[Install]
WantedBy=default.target
```

Now you can reboot systemd and run prometheus:

```bash
systemctl daemon-reload
```
```bash
systemctl restart prometheus
```
```bash 
systemctl status prometheus
```

let's try to address prometheus:
```bash
curl -D - -u dan:password --cacert /opt/ssl/ca.crt --resolve mywebsite.com:9090:127.0.0.1 https://mywebsite.com:9090
```
> Let's deal with these parameters. The first parameter `-D` will cause curl to display all the headers on the screen. 

> The second parameter `--cacert /opt/ssl/ca.crt` specifies the path to our root certificate, which will allow us to check its validity, since it is the one that the prometheus server certificate is signed with.

> >The third parameter `--resolve prom.kis.im:9090:127.0.0.1` - since we don't have a DNS entry for `mywebsite.com` with this parameter we force curl to define `mywebsite.com:9090` as `127.0.0.1`. And so when we specify the url `https://mywebsite.com:9090` at the end, curl will not go to DNS, but will go straight to `127.0.0.1:9090`. This is required for curl to be able to verify the server name mywebsite.com in our server certificate. And actually we got a valid response from prometheus: `302 Found`.



### Configuring Node_Exporter with Nginx Proxy

Let's try to implement the scheme with tls and basic auth for `node_exporter`.
So, let's start by generating a key and certificate for node_exporter, similar to prometheus:

So, let's start by generating a key and certificate for `node_exporter`, similar to prometheus (on machine prometheus):
```bash
openssl genrsa -out node_exporter.key 4096
```
```bash
cat > req.conf << EOF
```
```ini
[req]
distinguished_name = req_distinguished_name
x509_extensions = v3_req
prompt = no
[req_distinguished_name]
C = IL
ST = Telaviv
L = Telaviv
O = Mycompany
OU = IT
CN = node_exporter.mywebsite.com
[v3_req]
keyUsage = keyEncipherment, dataEncipherment
extendedKeyUsage = serverAuth
subjectAltName = @alt_names
[alt_names]
DNS.1 = node_exporter.mywebsite.com
EOF
```
```bash
openssl req -new -key node_exporter.key -out node_exporter.csr -config req.conf -extensions 'v3_req'
```

```bash
openssl x509 -req -days 365 -in node_exporter.csr -CA ca.crt -CAkey ca.key -set_serial 1 -extensions 'v3_req' -extfile req.conf -out node_exporter.crt
```
> After these steps, we will get the files `node_expoter.key` - the key and `node_exporter.crt` - the certificate for our exporter. 

> We used the `req.conf` configuration where we described all the fields we need, as well as additional SAN names for our host. This is important, because if we make an old certificate (without SAN names), prometheus will get an error like this: 

> Get "https://127.0.0.1:9100/metrics": x509: certificate relies on legacy Common Name field, use SANs or temporarily enable Common Name matching with GODEBUG=x509ignoreCN=0


Now let's install Nginx and make a configuration for it:
```bash
apt-get install nginx
```
We place the configuration in `/etc/sites-enabled/node_exporter.conf`:
```bash
server {
  listen 9100 ssl;

  ssl_certificate /opt/ssl/node_exporter.crt;
  ssl_certificate_key /opt/ssl/node_exporter.key;

  server_name node_exporter.mywebsite.com;

  location / {
    proxy_pass http://127.0.0.1:9100/;
    auth_basic "Restricted!";
    auth_basic_user_file .htpasswd;
  }
}
```
> Here everything is quite simple - we use the generated certificate and key for `node_exporter` and also enable basic authentication for requests. We listen on port 9100 with ssl enabled. 


Now we can create htpasswd file and Restart Nginx:
```bash
htpasswd -n dan
```
```bash
echo 'dan:$apr1$P1sF6Xzd$4rCqbl8FN.Fk9DqmUSEeb0' > /etc/nginx/.htpasswd
```
reload nginx
```bash
/etc/init.d/nginx  reload
```


```bash
./node_exporter --web.listen-address="127.0.0.1:9100"
```
> In this case we just run node_exporter in tmux, although it is better to run it through systemd. The main thing is that we added the `--web.listen-address=127.0.0.0.1:9100` parameter, so it will only listen for connections on the loopback interface and can only connect to it via nginx. Let's check if everything works:


```bash
curl -D - -u dan:password --cacert /opt/ssl/ca.crt --resolve node_exporter.mywebsite.com:9100:127.0.0.1 https://node_exporter.mywebsite.com:9100
```
> All parameters are taken from the previous command, so there is no point in parsing them here - the only thing we have changed is the server name to `node_exporter.mywebsite.com`, as it is specified in our certificates and port 9100, on which nginx listens.

Connect this exporter to prometheus:
```bash
global:
  scrape_interval:     15s
  evaluation_interval: 15s

scrape_configs:
  - job_name: 'prometheus'
    static_configs:
    - targets: ['localhost:9090']

  - job_name: node_Wexporter
    static_configs:
    - targets: ['127.0.0.1:9100']
    scheme: https
    tls_config:
      ca_file: /opt/ssl/ca.crt
      server_name: node_exporter.mywebsite.com
    basic_auth:
      username: dan
      password: password
```
> In this case we have defined a new task `node_exporter`, which connects to our nginx at 127.0.0.1:9100 via https: scheme: https. Then we specify the path to the root certificate so that prometheus can make sure that it connects to the right host, as well as the server name we set in the certificate - node_exporter.mywebsite.com. In addition, we add authentication rules - basic_auth. After this configuration we can restart prometheus and make sure that all metrics are successfully collected.
















## Prometheus Useful Cases

Case 1 is an active prometheus replica.

TSDB works only locally and cannot be configured to replicate or work in a cluster. But it is much simpler than you think. Since prometheus works on the pull model you can simply put a second virtualization next to prometheus, thus providing reservation. Do the same configuration and prometheus starts pulling metrics from the same sources in parallel. If one server die, you can easily start using a reserve server.


Case 2 - prometheus backup.

How to backup prometheus. It's really simple - you just need to backup the data directory (`/opt/prometheus/data`). Although there is an interesting dump command in promtool, but it just outputs a list of all metrics from the data directory:
```bash
./promtool tsdb dump /opt/prometheus/data | head -2
```
> It can output them, but you can't import them :) Prometheus even has an issue on this topic

But promtool allows you to import data in openmetrics format:
```bash
./promtool tsdb create-blocks-from openmetrics <input file> [<output directory>]
```
back to backups - you can use the API to create a snapshot of tsdb roughly like this:
```bash
curl -XPOST -D - -s http://localhost:9090/api/v1/admin/tsdb/snapshot
```
> That is, we just send a POST request to a specific URL. By the way, this url is not available by default and to enable it you need to add the `--web.enable-admin-api` option when starting prometheus. After running this command we will get a snapshot in the `/opt/prometheus/data/snapshots` directory:
```bash
ls -la /opt/prometheus/data/snapshots/
```

Case 3 - how to monitor prometheus itself.

If prometheus goes down, nobody will even notice it, because prometheus will not be able to send alerts to alertmanager. and alertmanager will probably die with prometheus.

To solve this problem we use a very simple method - "Dead Man's Switch". Its essence is the following - you set up an alert like this inside prometheus:
```bash
     - alert: DeadMansSwitch
       annotations:
         description: This is a DeadMansSwitch meant to ensure that the entire Alerting
           pipeline is functional.
         summary: Alerting DeadMansSwitch
       expr: vector(1)
       labels:
         severity: none
```
Since the vector(1) expression is used, this alert will essentially always be active. Next, you configure the alertmanager roughly as follows:

```bash
global:
 ...
route:
 ...
    routes:
    - match:
        alertname: DeadMansSwitch
        receiver: 'cole'
        group_wait: 0s
        group_interval: 1m
        repeat_interval: 50s
receivers:
- name: 'cole'
webhook_configs:
- url: 'https://nosnch.in/c2354d53d2'
    send_resolved: false
```
> This url can be obtained from deadmanssnitch.com service. Thus prometheus will constantly send alerts to alertmanager, and alertmanager in turn to deadmanssnitch.If your prometheus or alertmanager is down, the alerts will also be lost, which means something went wrong. Alerts can be configured in the deadmanssnitch itself. same functionality is offered by pagerduty. https://www.pagerduty.com/docs/guides/dead-mans-snitch-integration-guide/






## Grafana


### Install Grafana from Binary
```bash
cd /opt/
wget https://dl.grafana.com/enterprise/release/grafana-enterprise-10.2.2.linux-amd64.tar.gz
```
```bash
tar -zxvf grafana-enterprise-10.2.2.linux-amd64.tar.gz
```

```bash
mv grafana-enterprise-10.2.2.linux-amd64 grafana
```



```
[Unit]
Description=Grafana instance
Documentation=http://docs.grafana.org
Wants=network-online.target
After=network-online.target
After=postgresql.service mariadb.service mysql.service influxdb.service

[Service]
EnvironmentFile=/etc/default/grafana-server
User=grafana
Group=grafana
Type=simple
Restart=on-failure
WorkingDirectory=/usr/share/grafana
RuntimeDirectory=grafana
RuntimeDirectoryMode=0750
ExecStart=/usr/share/grafana/bin/grafana server                                     \
                            --config=${CONF_FILE}                                   \
                            --pidfile=${PID_FILE_DIR}/grafana-server.pid            \
                            --packaging=deb                                         \
                            cfg:default.paths.logs=${LOG_DIR}                       \
                            cfg:default.paths.data=${DATA_DIR}                      \
                            cfg:default.paths.plugins=${PLUGINS_DIR}                \
                            cfg:default.paths.provisioning=${PROVISIONING_CFG_DIR}

LimitNOFILE=10000
TimeoutStopSec=20
CapabilityBoundingSet=
DeviceAllow=
LockPersonality=true
MemoryDenyWriteExecute=false
NoNewPrivileges=true
PrivateDevices=true
PrivateTmp=true
ProtectClock=true
ProtectControlGroups=true
ProtectHome=true
ProtectHostname=true
ProtectKernelLogs=true
ProtectKernelModules=true
ProtectKernelTunables=true
ProtectProc=invisible
ProtectSystem=full
RemoveIPC=true
RestrictAddressFamilies=AF_INET AF_INET6 AF_UNIX
RestrictNamespaces=true
RestrictRealtime=true
RestrictSUIDSGID=true
SystemCallArchitectures=native
UMask=0027

[Install]
WantedBy=multi-user.target
```


Nginx configuration example

upstream grafana {
  server localhost:3000;
}

server {
    listen 80;
    #listen 443 ssl;

    root /var/www/grafana;
    index index.html index.htm index.nginx-debian.html;

    server_name grafana.localhost;

    #ssl_certificate     /etc/letsencrypt/live/grafana.localhost/fullchain.pem;
    #ssl_certificate_key /etc/letsencrypt/live/grafana.localhost/privkey.pem;

    location / {
        proxy_set_header Host $http_host;
        proxy_pass http://grafana;
    }
}




Reload the NGINX configuration via the command line:

/etc/init.d/nginx reload







### Grafana from repository


Install the Grafana repository authentication key:
```bash
wget -q -O - https://packages.grafana.com/gpg.key | sudo apt-key add
```
Add a repository with the command:
```bash
sudo add-apt-repository "deb https://packages.grafana.com/oss/deb stable main"
```
Update the list of packages in the repositories:
```bash
sudo apt update
```
Installation:
```bash
sudo apt install grafana
```
After installation, reboot of the daemons:
```bash
sudo systemctl daemon-reload
```
After installation, enable autoloader and start the Grafana daemon:
```bash
sudo systemctl start grafana-server
```
After installation, enable autoloader and start the Grafana daemon:
```bash
sudo systemctl enable grafana-server
```
Start Grafana 
```bash
sudo systemctl start grafana-server
```
> Grafana is up and running and ready to accept incoming connections on port 3000.
> Login and password for login: admin/admin



### Config file and Environment Variables

By default, the config file with Grafana settings is located in the working directory of the OS. For Ubuntu, this is `/etc/grafana/grafana.ini`. You can override the location of the file by using the `--config <path>` flag.

The `.ini` configuration file consists of sections labeled with square brackets.
```ini
# default section
instance_name = ${HOSTNAME}

[security]
admin_user = admin

[auth.google]
client_secret = 0ldS3cretKey

[plugin.grafana-image-renderer]
rendering_ignore_https_errors = true
```
> A detailed description of the parameters can be found in the official documentation: `https://grafana.com/docs/grafana/latest/administration/configuration/`.

You can also use environment variables to customize Grafana. The name of the environment variables is always given in capital letters and is built from the section name and parameter name in the configuration file: GF_<SectionName>_<KeyName>.
```bash
export GF_DEFAULT_INSTANCE_NAME=my-instance
export GF_SECURITY_ADMIN_USER=owner
export GF_AUTH_GOOGLE_CLIENT_SECRET=newS3cretKey
export GF_PLUGIN_GRAFANA_IMAGE_RENDERER_RENDERING_IGNORE_HTTPS_ERRORS=true
```
> Using environment variables can be particularly useful when using a Docker container or for testing a new parameter configuration.

As with a configuration file, Grafana needs to be restarted for changes to take effect


### Configuring Nginx Reverse Proxy for Grafana

Install nginx
```bash
sudo apt install nginx
```
Check nginx status
```bash
sudo service nginx status
```
> If you see Active status, it means that nginx is installed and working correctly.

> Best practice:
>>Before starting nginx, you may need to first stop another web server, Apache2, and remove it from autoloading. To do this, run the following commands in sequence:
>>```bash
>>sudo systemctl disable apache2
>>sudo systemctl status apache2
>>```

Go to the IP address of our server (or localhost if you execute commands locally). Note that this time port 3000 should not be specified! The browser should display the nginx start page.
 
Create an nginx config file for Grafana.:
```bash
nano /etc/nginx/sites-enabled/my-grafana.conf
```
A text editor will open. Let's fill in the nginx configuration file
```bash
server {
    listen 80;
    listen [::]:80;
    server_name  my-grafana;

    location / {
        proxy_pass http://localhost:3000/;
    }
}
```
> Where my-grafana is the domain name you plan to use to access Grafana.
>> Note: that port 80 is specified in the nginx configuration as an example only. 
>> In a production environment, SSL (and HTTPS connection) must be used.


## Basic Authentication

Basic Authentication is basic authentication using a login/password pair.



## LDAP Authentication

LDAP Authentication is an authentication method that uses the LDAP protocol and access to a directory service.

LDAP is most often encountered in the case of Windows domain authorization. Let's take a closer look at the LDAP configuration process in Grafana.

Before configuring Grafana itself, we need to go to the Windows server where we have the domain controller configured and create a grafana user. 

With this technical account we will "go" to LDAP and fetch users from there when they try to log in.

- To do this, go to `Active Directory` `Users and Computers`. Open our DC and in the `Users group` right-click New and User.

- Fill in the parameters of the new user.

- On the "Account" tab, enter the name "grafana" in the "User Login Name" field.


Next, log in to our server with Grafana and go to the home directory of the service.
```bash
cd /etc/grafana
```
Open the grafana.ini file
```bash
sudo nano grafana.ini
```
Go down to the [auth.ldap] parameter group.

Set enabled = true and remove the semicolons to make it work:
```bash
[auth.ldap]
enabled = true
config_file = /etc/grafana/ldap.toml
allow_sign_up = true
```
Open the file ldap.toml
```bash
sudo nano ldap.toml
```
Replace the parameters with the ones shown below.
```bash
host = "192.168.67.138"
```
> ...where host IP is the address of your Windows server raised as a Domain Controller.

```bash
bind_dn = "cn=grafana LDAP,cn=users,dc=MRS,DC=CONTOSO,DC=COM"
```
To find out the exact `bind_dn`, you need to run the cmdlet on a Windows server in PowerShell:
```ps1
Get-ADUser grafana -Properties *
```
```bash

```
```bash

```
```bash

```
```bash

```
```bash

```
```bash

```
```bash

```
```bash

```
```bash

```
```bash

```
























## LDAP Authentication



















## Configuring Nginx Reverse Proxy for Grafana SSL


![prometheus5](images/prometheus5.svg)




**Data filtering**

```bash
mkdir -p /home/certs/prometheus
```

```bash
cd /home/certs/prometheus
```

```bash
sudo openssl req -newkey rsa:4096 -nodes -x509 -keyout prometheus.key -out prometheus.crt 
```

записать все поля 



```bash
sudo nano /etc/nginx/nginx.conf 
```
Edit conf
```conf
server {
    listen              443 ssl;
    server_name         www.example.com;
    ssl_certificate     home/certs/prometheus.crt;
    ssl_certificate_key home/certs/prometheus.key;
    location / {
        proxy_pass http://localhost:3000/;
    }
}
```
```bash
systemctl restart nginx
```
```bash
systemctl stop nginx
```
```bash
sudo prometheus \
--config.file=/etc/prometheus/prometheus.yml \
--web.external-url=https://prometheus.do.rbr-grafana.com \
--web.route-prefix="/" 
```


##

sudo apt-get install apache2-utils 

sudo htpasswd -c .htpasswd admin 

```bash
sudo nano /etc/nginx/nginx.conf 
```
Edit conf
```conf
server {
    listen              443 ssl;
    server_name         www.example.com;
    ssl_certificate     home/certs/prometheus.crt;
    ssl_certificate_key home/certs/prometheus.key;
    location / {
        proxy_pass http://localhost:3000/;
        auth_basic           "Prometheus";
        auth_basic_user_file /home/user.htpasswd;            
    }
}
```
```bash
systemctl restart nginx
```

curl -u admin -k https://prometheus.do.rbr-grafana.com:443/metrics



On grafana 

Configuration => Data source => Add data source => Select Prometheus 

```
HTTP
URL : https://prometheus.do.rbr-grafana.com
Basic Auth [x]
Skip TLS verify [x]
with Credentials [x]
User: admin 
Password: *********
```
[Save & Test]



## Configuring Nginx Reverse Proxy for Grafana


Create an Nginx config file for Grafana. To do this, type the following command:
```bash
nano /etc/nginx/sites-enabled/my-grafana.conf
```
A text editor opens. Let's fill in the nginx configuration file:
```conf
server {
    listen 80;
    listen [::]:80;
    server_name  my-grafana;

    location / {
        proxy_pass http://localhost:3000/;
    }
}
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






### Change Configuration of Promtail 


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

