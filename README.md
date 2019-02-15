# InfluxDB

## Set up a server running InfluxDB

Add influxdata repository using the following command:
```
$ cat /etc/apt/sources.list.d/influxdb.list
deb https://repos.influxdata.com/debian stretch stable
```
Import repo gpg key for installing signed packages:
```
$ sudo curl -sL https://repos.influxdata.com/influxdb.key | sudo apt-key add -
```
Update apt index and install influxdb package:
```
$ sudo apt-get update
$ sudo apt-get install influxdb
```
Start influxdb service:
```
$ sudo systemctl start influxdb
$ sudo systemctl enable influxdb
```
You can check the status to confirm if running using:
```
$ sudo systemctl status influxdb
```
Open influxdb service ports on the firewall
I use ufw firewall on all my Ubuntu 18.04 and Debian 9 servers. 
If ufw is not installed, install it using the command:
```
$ sudo apt-get install ufw
```
Then activate firewall service:
```
$ sudo ufw enable
```
By default, InfluxDB uses the following network ports:
* TCP port 8086 is used for client-server communication over InfluxDB’s HTTP API
* TCP port 8088 is used for the RPC service for backup and restore

We will open port 8086 since telegraf will push metrics using this port.
```
$ sudo ufw allow 8086/tcp
```
InfluxDB default configuration file is located under _/etc/influxdb/influxdb.conf_. 
Most sections are commented out, you can modify it to your liking and restart influxdb service after.
