# InfluxDB

## Set up a server running InfluxDB

Add influxdata repository using the following command:
```
$ sudo nano /etc/apt/sources.list.d/influxdb.list 
```
and write into the file
```
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
```
Start influxdb service on boot:
```
$ sudo systemctl enable influxdb
```

You can check the status to confirm if running using:
```
$ sudo systemctl status influxdb
```
InfluxDB default configuration file is located under _/etc/influxdb/influxdb.conf_. 
Most sections are commented out, you can modify it to your liking and restart influxdb service after.


## Authentication
The local InfluxDB configuration file is located in _/etc/influxdb/influxdb.conf_.
Determines whether user authentication is enabled over HTTP and HTTPS. By default, it is not required. To require authentication, set the value to true.
```
auth-enabled = true
```
## Disk Usage

```
du -sh /var/lib/influxdb/data/<db name>
```
Where /var/lib/influxdb/data is the data directory defined in influxdb.conf

## Terminal
```
influx
```
```
CREATE USER nome_admin WITH PASSWORD 'password_admin' WITH ALL PRIVILEGES
```
Quindi si rientra con
```
influx -username nome_utente -password mia_password
```
verfichiamo con 
```
SHOW USERS
```
```
CREATE DATABASE house_monitoring_DB
```
```
DROP DATABASE house_monitoring_DB
```
```
SHOW DATABASES
```
```
USE nome_database
```
```
SELECT * FROM nome_measurement
```
```
DROP MEASUREMENT measurement_name
```
```
DROP SERIES FROM "Temperature" WHERE station='client_1'
```

creo un utente con la sintassi:
```
CREATE USER <username> WITH PASSWORD '<password>'
```
e poi gli assegno i diritti:
```
GRANT [READ,WRITE,ALL] ON <database_name> TO <username>
```
es:
```
CREATE USER mkr1010 WITH PASSWORD '123456'
```
```
GRANT WRITE ON myDB TO mkr1010
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
