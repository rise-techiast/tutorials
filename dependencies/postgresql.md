# PostgreSQL Setup Guideline
This is a step-by-step guide to install PostgreSQL.

## Install PostgreSQL

```sh
sudo apt update
sudo apt install postgresql postgresql-contrib
```
## Basic Usage
Access to PostgreSQL Account
```sh
sudo -i -u postgres
```
Create a User Account
```sh
createuser --interactive
```
Create a New Database
```sh
createdb $DB_NAME
```
## Database Management
Access Postgres
```sh
psql
```
Exit PostgreSQL
```
\q
```
List all Databases
```
SELECT datname FROM pg_database;
```
Access to a Database
```
\c $DB_NAME
```
List all Tables in a Database
```
\dt *.*
```
Describe Columns of a Table
```
\d $SCHEMA.$TABLE_NAME