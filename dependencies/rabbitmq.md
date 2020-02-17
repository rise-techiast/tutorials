# RabbitMQ Setup Guideline
This is a step-by-step guide to setup a RabbitMQ Server.

## Install Erlang
```sh
cd /usr/src
## Add Erlang Solutions repository
sudo wget https://packages.erlang-solutions.com/erlang-solutions_1.0_all.deb
sudo dpkg -i erlang-solutions_1.0_all.deb

## Add the Erlang Solutions public key for "apt-secure"
sudo wget https://packages.erlang-solutions.com/ubuntu/erlang_solutions.asc
sudo apt-key add erlang_solutions.asc

## Install Erlang
sudo apt-get update
sudo apt-get install esl-erlang
```

## Install RabbitMQ
```sh
## Install RabbitMQ signing key
curl -fsSL https://github.com/rabbitmq/signing-keys/releases/download/2.0/rabbitmq-release-signing-key.asc | sudo apt-key add -

## Install apt HTTPS transport
sudo apt-get install apt-transport-https

## Add Bintray repositories that provision latest RabbitMQ and Erlang 21.x releases
sudo nano /etc/apt/sources.list.d/bintray.rabbitmq.list

## Input the following lines
deb https://dl.bintray.com/rabbitmq-erlang/debian bionic erlang-21.x
deb https://dl.bintray.com/rabbitmq/debian bionic main

## Update package indices
sudo apt-get update -y

## Install rabbitmq-server and its dependencies
sudo apt-get install rabbitmq-server -y --fix-missing
```

## Run the Server
Start the Server
```sh
## Start the service
sudo service rabbitmq-server start

## Check service status
sudo service rabbitmq-server status
```
## Troubleshooting
```sh
rm -rf /var/lib/rabbitmq/mnesia/*
systemctl start rabbitmq-server
```