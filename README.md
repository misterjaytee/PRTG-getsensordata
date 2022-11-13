# PRTG-getsensordata
This python script pulls sensor data from a PRTG server and uploads it into tables in a SQL database.
<br/><br/>
The script has only been tested on Linux using a MySQL backend, but should work as-is (or with very little change) with PostgreSQL.
<br/><br/>
By default, the sensor data is pulled for the month prior to the time the script is started (so running it on 1st November will pull October's data). The script checks whether there is already data for that month for the sensor (it will only check the end date, which is the last day of the month unless running in debug mode).
<br/><br/>
A database needs to be created, but there is no need to create tables as these are automatically created based on config settings for the sensor (see Setup below).
<br/><br/>
If there is a need to backfill data for other months, then the script can be run with an additional parameter for the month number that you wish to pull data for, e.g. to get September's data  
```shell
./getsensordata 9
```

<br/>
If you need to add another sensor, then add it to config.ini and re-run the script. The script will simply skip sensor data collection where it already has data for other sensors and will download just the data for the new sensor.

<br/>

## Setup
<br/>

Copy config.ini.example to config.ini.  
<br/>

Create a database on your database server and give a user access to the database, e.g. for MySQL:  
```bash
mysql -u root -p
```
```sql
create database prtg_historicdata;
grant all on prtg_historicdata.* to 'prtguser'@'localhost' identified by 'mysecretpassword';
quit;
```
<br/>

Edit config.ini and update with:  
* [database]
  * host
  * username
  * password
  * database
* [prtg]
  * hostname
  * API token (Go to Setup -> Account Settings -> API Keys)
    * For security purposes, create a read-only token
* [sensors]
  * Each sensor line has two parts in the format:  
  **tablename=sensorid**
    * tablename - must be unique, with no spaces or special characters
    * sensorid - this is the id of the sensor on PRTG. Click into a sensor to find the id in the URL, e.g. the id is 12345 in the following URL:  
prtghost.example.com/sensor.htm?id=12345&tabid=1
<br/><br/>

## Dependencies
<br/>

This script was written and tested using Python 3.9 and uses the following Python modules (the first two should come with Python):  
configparser  
datetime  
pandas  
python-dateutil  
SQLAlchemy  


Additionally, you should install the module for your backend database driver. For MySQL use:  
PyMySQL

<br/>

To install the dependencies, including the PyMySQL driver, run:  
```shell
pip3 install -r requirements.txt
```

<br>
Make sure that the dependencies are installed before running the script.
