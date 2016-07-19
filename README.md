# docker-monitor

## Introduction
This stack is based on InfluxDB, Kapacitor, and grafana to build up a simple monitoring demo platform. It also use nginx as a reverse proxy to link the backend.

## Pre-request
- docker: 1.10.0 or above
- docker-compose: 1.6.0 or above

## How to use
#### 1. Monify the URL settings in `./etc/nginx/conf.d/monitor.conf` and `docker-compose.yml`
Change the example.com to the domain you use.
```
server {
    listen       80;
    server_name  grafana.example.com;

   location / {
      proxy_set_header   X-Real-IP $remote_addr;
      proxy_set_header   Host      $http_host;
      proxy_pass         http://grafana:3000/;
   }
}
```

Change the example.com to the slack URL in your channel.
```
      KAPACITOR_SLACK_URL: 'http://example.com'
```
#### 2. Use `docker-compose` to launch a whole stack
```
# docker-compose up -d
```
#### 3. Create a database to collect data
```
# docker-compose run influxdb-cli
Connected to http://influxdb:8086 version 0.13.0
InfluxDB shell version: 0.13.0
> CREATE DATABASE "telegraf"
```
#### 4. Check if the data collected in the new database.
Wiat about 10 seconds if everythings goes well, you can see the data send from telegraf and created as following:
```
> SHOW MEASUREMENTS
name: measurements
------------------
name
cpu
disk
diskio
kernel
mem
net
nginx
processes
swap
system
```
#### 5. Use `quit` to exit the influxdb cli interface.
```
> quit
```
