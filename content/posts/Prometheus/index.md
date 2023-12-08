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
rm -f redis_exporter-v1.55.0.linux-amd64.tar.gz 
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

**Configuring postgres_exporter**

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



### Configure Postgres_Exporter for Systemd

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
mv pushgateway-1.6.2.linux-arm64.tar.gz pushgateway
```

Run (default port `127.0.0.1:9091`)

```bash
./pushgateway
```





### Configurate Pushgateway For systemd

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
> * honor_labels: true must be!

Restart Prometheus to load the new configuration:
```bash
sudo systemctl restart prometheus
```


## Send Metrics to PushGateway

Sending basic metrics

First, it's worth mentioning that pushgateway has a built-in web interface, which can be accessed using port 9091 (default). We will use the console to work with it. So, pushgateway provides a simple API that you can use to send metrics to it. After receiving the metrics, pushgateway can give them to Prometheus server via the /metrics endpoint. Let's try to create our first metric using curl. Let's say our cron script runs once an hour and takes money from users' accounts. Then we can send information to pushgateway about the number of users processed. Or the total amount of debit:

```bash
echo "cron_app_processed_users 112" | curl --data-binary @- http://localhost:9091/metrics/job/cron_app

echo "cron_app_payed_sum 13423" | curl --data-binary @- http://localhost:9091/metrics/job/cron_app
```
In this example we sent two metrics - `cron_app_processed_users` - number of processed users with the value `112` and `cron_app_payed_sum` - total amount of debit in the amount of `13423`. We also used the `--data-binary` option, which does not modify the data passed to it from stdin in any way, but just passes it in the POST request.

we used the path `/metrics/job/cron_app`. In this case, `job` is a label, and `cron_app` is its value. Remember in scrape_config for Prometheus we set the job option, which was displayed in metrics as a tag? Actually, this is exactly that tag. Pushgateway will group your metrics by this tag. This is very useful when working with a large number of crowns or other short-lived tasks. You can see the grouped metrics by them.




root@prom:/opt# curl http://localhost:9091/metrics
# TYPE cron_app_payed_sum untyped
cron_app_payed_sum{instance="",job="cron_app"} 13423
# TYPE cron_app_processed_users untyped
cron_app_processed_users{instance="",job="cron_app"} 112



















## Prometheus 

























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

