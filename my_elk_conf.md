    # Configuring elk on Ubuntu

## Elastic search
configraton file location

`/etc/elasticsearch/elasticsearch.yml`

My port = 9200
my host = "localhost"

discovery.type: single-node

Path locations

path.data: /var/lib/elasticsearch

path.logs: /var/log/elasticsearch

-------------------------------------------------

## Kibana

Kibana configration file 

`/etc/kibana/kibana.yml`

Port = 5061
host = "localhost"
