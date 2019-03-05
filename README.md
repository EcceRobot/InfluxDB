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
You may need to install the apt-transport-https package which is needed to fetch packages over HTTPS.
```
sudo apt-get install -y apt-transport-https
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


## Authentication
The local InfluxDB configuration file is located in _/etc/influxdb/influxdb.conf_.
Determines whether user authentication is enabled over HTTP and HTTPS. By default, it is not required. To require authentication, set the value to true.
```
auth-enabled = true
```


## InfluxDB HTTP API

Use _/ping_ to check the status of your InfluxDB instance and your version of InfluxDB.
```
curl -sl -I http://localhost:8086/ping
```
Create a database
```
curl -XPOST 'http://localhost:8086/query' --data-urlencode 'q=CREATE DATABASE "mydb"'
```
Create a database using HTTP authentication
```
curl -XPOST 'http://localhost:8086/query?u=myusername&p=mypassword' --data-urlencode 'q=CREATE DATABASE "mydb"'
```
Write a point to the database mydb
```
curl -i -XPOST "http://localhost:8086/write?db=mydb" --data-binary 'mymeas,mytag=1 myfield=90 1463683075'
```
Query data with a SELECT statement
```
curl -G 'http://localhost:8086/query?db=mydb' --data-urlencode 'q=SELECT * FROM "mymeas"'
```
Request query results in CSV format
```
$ curl -H "Accept: application/csv" -G 'http://localhost:8086/query?db=mydb' --data-urlencode 'q=SELECT * FROM "mymeas"'
```
