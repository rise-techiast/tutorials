# MongoDB Setup Guideline
This is a step-by-step guide to install MongoDB.

## Install MongoDB
Import the public key used by the package management system:
```sh
wget -qO - https://www.mongodb.org/static/pgp/server-4.2.asc | sudo apt-key add -
```
Create a list file for MongoDB:
```sh
## For Ubuntu 18.04 (Bionic)
echo "deb [ arch=amd64 ] https://repo.mongodb.org/apt/ubuntu bionic/mongodb-org/4.2 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-4.2.list

## For Ubuntu 16.04 (Xenial)
echo "deb [ arch=amd64 ] https://repo.mongodb.org/apt/ubuntu xenial/mongodb-org/4.2 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-4.2.list
```
Reload local package database.
```sh
sudo apt-get update
```
Install the MongoDB packages:
```sh
sudo apt-get install -y mongodb-org
```
## Basic Usage
#### Start MongoDB
Issue the following command to start mongod:
```sh
sudo service mongod start
```
Verify that MongoDB has started successfully:
```sh
sudo cat /var/log/mongodb/mongod.log

## If you can see this line then the service has been enabled successfully:
## [initandlisten] waiting for connections on port 27017
```
#### Stop MongoDB
```sh
sudo service mongod stop
```
#### Restart MongoDB
```sh
sudo service mongod restart
```

## Database Management
#### List running instances
```sh
sudo lsof -iTCP -sTCP:LISTEN | grep mongo
```
#### Connect to an instance
```sh
mongo --port $PORT
```
#### Create an administrator account
```sh
use admin
db.createUser(
  {
    user: "admin",
    pwd: "admin",
    roles: [ { role: "userAdminAnyDatabase", db: "admin" } ]
  }
)
```
#### Authenticate as the user administrator
```sh
mongo --port $PORT -u "admin" -p "admin" --authenticationDatabase "admin"
```
#### List all databases
```sh
show dbs
```
#### Connect to a database
```sh
use $DATABASE_NAME
```
#### List all tables
```sh
show tables
```
#### Find a record
```sh
db.$TABLE.findOne()
```
#### Find the latest inserted record
```sh
db.$TABLE.find().sort({'_id': -1}).limit(1)
```
#### List all keys of a table
```sh
Object.keys(db.$TABLE.findOne())
```