# Logicfinder

# Zeek aka bro

##  Intro to zeek 
  - Zeek is a passive network monitoring system
  - zeek can be installed on a "sensor", which can be hardware, software, virtual, or cloud platform that passively sniffs the network traffic
  - zeek generates compact, high-fidelity log files bases on what it sees in the network traffic
  - log files are fully customizable, suitble for manual review on disk or in a more analyst-firendly tool like a security and info event management (SIEM).
  
  ## Installing zeek
  ```bash
  # sudo su
● # sudo apt-get install cmake make gcc g++ flex bison libpcap-dev libssl-dev python-dev swig
zlib1g-dev
● # sudo sh -c "echo 'deb
http://download.opensuse.org/repositories/security:/zeek/xUbuntu_18.04/ /' >
/etc/apt/sources.list.d/security:zeek.list"
● # wget -nv
https://download.opensuse.org/repositories/security:zeek/xUbuntu_18.04/Release.key -O
Release.key
● # sudo apt-key add - < Release.key
● # sudo apt-get update
● # sudo apt-get install zeek-lts
● # /opt/zeek/bin/zeek -h

``` 
## Or 
[zeek docs](https://docs.zeek.org/en/master/install.html)

## Comprehensive logs = Attackers can't hide

<p>
   Zeek turns raw network traffic into Comprehensive logs organized by protocol, with key fiels extracted specifically for security use cases
   </p>
   
   
`network.cfg`
 - Monitor the network you want enternal or external
 
 `node.cfg`
 - Select the interface like eth0, wlan0
 
 `zeekctl.cfg`
 - All the configrations, where are the roots files etc
 
 ### Runing the zeek
 ```bash
 zeekctl deploy
 
 ## Check status
 zeekctl status
 ```
 ### Check the logs
  
```bash
/opt/zeel/spool

```
### stop the zeek
```bash
zeekctl stop
```
#### Check all the logs
```
/opt/zeek/logs/
```

[Wazuh] can be used to view the details


#  Elastic Stack on Ubuntu 


## what is elk stack?
  Elastic Stack formerly ELK is a collection of below open source tools.

- Elasticsearch
- Logstash
- Kibana
- Filebeat
--------------------------------------------------------------------------------------------------
Elastic Stack provides Log monitoring for our server and applications at one location.

A proper Log management is done by using ELK stack which includes collection of logs, its processing and visualization. Elastic Stack allows to fetch data from any source and in any format.

For data collection Filebeat is needed which renamed the ELK to Elastic Stack.
--------------------------------------------------------------------------------------------------

## What is Elasticsearch ?
Elastic search is a open source tool which is used to store logs, Elasticsearch is a NoSQL database based on Lucene search engine, After storing logs we can also search it and indexed it.

In Elasticsearch we can store and analyze large volume data, Management becomes more easier by using Elasticsearch because it offers simple deployment.
--------------------------------------------------------------------------------------------------
## What is Logstash ?
Logstash is used for collection of data and logs, After collected logs it gets transformed and feeds logs forward to Elasticsearch.

Logstash consists of three components –

Input
Filtering
Output
Logstash can analyze structured as well as unstructured data.

Logstash provides plugins to get connected with different types of input sources to Logstash.
--------------------------------------------------------------------------------------------------
## What is Kibana ?
Kibana is a visualization tool without which Elastic Stack is incomplete, Kibana provides a web interface on which we can search queries from Elasticsearch DB and then we can visualize it from the results obtained.

Kibana provides various types of dashboards which includes varieties of data diagrams, charts and graphs to visualize data, We can perform real-time search of indexed information.

We can save the dashboards and can share the snapshots of the logs been visualized.

--------------------------------------------------------------------------------------------------
## Prerequisites

- Ubuntu Server with 20.04 LTS
- JDK
- 2 CPU and 4 GB RAM
- Open Ports 9200, 5601, 5044
--------------------------------------------------------------------------------------------------
### Install JDK on Ubuntu
```bash
sudo apt-get install openjdk-11-jdk wget apt-transport-https curl gnupg2 -y
## To check java version
java -version

```
##  Install and Configure ElasticSearch on Ubuntu
```bash
wget -qO - https://artifacts.elastic.co/GPG-KEY-elasticsearch --no-check-certificate | sudo apt-key add -
```
```bash
# update the system packages

sudo apt-get update
# Install elasticsearch on Ubuntu using below command

sudo apt-get install elasticsearch -y
# Let’s make changes in below elesticsearch configuration file

 sudo nano /etc/elasticsearch/elasticsearch.yml
# Go to Network section and uncommnet network.host and replace your system IP or localhost

network.host: localhost
Add the below line in Discovery section also.

discovery.type: single-node
```

### start the elacticsearch service
```bash
sudo systemctl start elasticsearch
To enable elacticsearch at system startup

sudo systemctl enable elasticsearch
To check the status of elasticsearch

sudo systemctl status elasticsearch
To stop elasticsearch service

sudo systemctl stop elasticsearch
To check elasticsearch service pid using command line

sudo ss -antpl | sudo grep 9200
```
--------------------------------------------------------------------------------------------------
## Install and Configure Kibana on Ubuntu
```bash
sudo apt-get install kibana
# Kibana runs on localhost so will have to change in kibana configuration file.

sudo nano /etc/kibana/kibana.yml
# Uncomment the below lines

server.port: 5601
server.host: "0.0.0.0"
elasticsearch.hosts: ["http://localhost:9200"]
```
### To start kibana service
```bash
sudo systemctl start kibanaCopy
To enable kibana at system startup

sudo systemctl enable kibana
To check the status of kibana service

sudo systemctl status kibana
To stop kibana service

sudo systemctl stop kibana
```
--------------------------------------------------------------------------------------------------
### Install and Configure Logstash on Ubuntu

use below command to install logstash on ubuntu


```bash

sudo apt-get install logstash
To load logstash beat create the below config file and insert below lines.

sudo nano /etc/logstash/conf.d/02-beats-input.conf
input {

  beats {

    port => 5044

  }

}
save and close the file.
```
Create the logstash configuration file to send the logs, filter and showing in kibana and insert below lines
```bash
sudo nano /etc/logstash/conf.d/30-elasticsearch-output.conf

output {

  elasticsearch {

    hosts => ["localhost:9200"]

    manage_template => false

    index => "%{[@metadata][beat]}-%{[@metadata][version]}-%{+YYYY.MM.dd}"

  }
```
To start logstash service
```bash
sudo systemctl start logstash
To enable logstash at system startup

sudo systemctl enable logstash
To stop logstash service

sudo systemctl stop logstash
To check status of logstash

sudo systemctl status logstash
```
--------------------------------------------------------------------------------------------------
# Install and Configure Filebeat on Ubuntu

Filebeat is an input to the logstash which gives logs collected from different sources. Follow the below command to install Filebeat so it can send logs to Logstash.
```bash
sudo apt-get install filebeat
Now lets make changes in filebeat configuration file

sudo nano /etc/filebeat/filebeat.yml
Comment the below lines

#output.elasticsearch:
  # Array of hosts to connect to.
#  hosts: ["localhost:9200"]Copy
Uncomment the below lines

output.logstash:
hosts: ["localhost:5044"]
```
### To start filebeat service

```bash
sudo systemctl start filebeat
To enable filebeat at system startup

 sudo systemctl enable filebeat
To check status of filebeat service

sudo systemctl status filebeat
Enable filebeat system module

sudo filebeat modules enable system
``` 
### Load the index template
```bash
filebeat setup --index-management -E output.logstash.enabled=false -E 'output.elasticsearch.hosts=["localhost:9200"]'
Now lets check that ElasticSearch is receiving datalog from filebeat using below command

curl -XGET http://localhost:9200/_cat/indices?v
```
--------------------------------------------------------------------------------------------------
--------------------------------------------------------------------------------------------------
