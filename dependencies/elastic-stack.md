# Elasticsearch Stack Setup Guideline
This is a step-by-step guide to install Elasticsearch Stack (ELK).

## Prerequisites
* Install [Docker](https://techiast.com/docker/).
* A minimum of 4GB RAM assigned to Docker.
* A limit on mmap counts equal to 262,144 or more.

## Install Elasticsearch Stack

Increase limit on `mmap counts` equal to `262,144` or more:

```sh
## View the current value:
sysctl vm.max_map_count

## If the value is less then set using this command:
sudo sysctl -w vm.max_map_count=262144
```

Clone Docker Image:
```sh
sudo docker pull sebp/elk
```

Create `docker-compose.yml` file:
```sh
cd $YOUR_DIRECTORY
nano docker-compose.yml
```

Paste the following contents:
```sh
elk:
  image: sebp/elk
  ports:
    - "5601:5601"
    - "9200:9200"
    - "5044:5044"
```
Run the container using Docker Compose:
```sh
docker-compose up -d elk
```
## Install Metricbeat

Install Metricbeat:
```sh
## Download and install the Public Signing Key
wget -qO - https://artifacts.elastic.co/GPG-KEY-elasticsearch | sudo apt-key add -

## Download and install the Public Signing Key:
sudo apt-get install apt-transport-https

## Save the repository definition to  /etc/apt/sources.list.d/elastic-7.x.list:
echo "deb https://artifacts.elastic.co/packages/7.x/apt stable main" | sudo tee -a /etc/apt/sources.list.d/elastic-7.x.list

## Install Metricbeat by running:
sudo apt-get update && sudo apt-get install metricbeat
```

Configure Metricbeat with Elasticsearch:
```sh
## Install Elasticsearch index template 
sudo metricbeat setup --template -E 'output.elasticsearch.hosts=["localhost:9200"]'

## Start and enable Metricbeat
sudo systemctl start metricbeat
sudo systemctl enable metricbeat

## Verify that Elasticsearch is indeed receiving this data
curl -XGET 'http://localhost:9200/metricbeat-*/_search?pretty'
```

## Configure Metricbeat on monitored server

Configure to connect to ELK server:
```sh
sudo nano /etc/metricbeat/metricbeat.yml
```
Find the following section and update the IP address to the ELK Server IP:
```
#-------------------------- Elasticsearch output ------------------------------
output.elasticsearch:
  # Array of hosts to connect to.
  hosts: ["Elastic_Stack_server_ip:9200"]

...
```
Start and enable Metricbeat:
```sh
sudo systemctl start metricbeat
sudo systemctl enable metricbeat
```





## Basic Usage
Access Kibana's web interface:
```sh
http://<your-host>:5601
```