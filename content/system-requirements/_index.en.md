---
title: "System Requirements"
date: 2020-07-12T15:21:02+02:00
draft: false
---

### Contents of this documentation

In this documentation, we will make sure all your system and application dependencies are correct, before you start using *Middleware*.

### Prepare your dependencies in Docker

We will use Logstash, Elasticsearch and Kibana as a logging platform.
 
As those systems are complex to instakll and out-of-scope of this documentation, we will use Docker to start those services.

#### Logstash

Logstash is a tool whose primary goal is to consume logging from other applications, optionally apply transformations to
it and then transfer it to a log storage.

In your project, create a file named `.docker/logstash/logstash.yml` with the following contents:

```yaml
pipeline:
  batch:
    delay: 50
    size: 125
```

To configure a logging pipeline for logstash, we will create a second file named `.docker/logstash/pipeline/tcp.conf` with the following content:

```
input {
  tcp {
     port => 5044
     host => "0.0.0.0"
     codec => json
  }
}
filter {
  date {
    match => [ "timeMillis", "UNIX_MS" ]
  }
}

output {
  elasticsearch {
    hosts => "elasticsearch:9200"
  }
}
```

Now, you will need to add a new logstash service in your `docker-compose.yaml` file:

```yaml
services:
  logstash:
    image: docker.elastic.co/logstash/logstash-oss:7.8.0
    volumes:
      - ./.docker/logstash/logstash.yml:/usr/share/logstash/config/logstash.yml
      - ./.docker/logstash/pipeline:/usr/share/logstash/pipeline/
```

#### ElasticSearch

ElasticSearch is a Document oriented database, whose main usage are logs storage and search indexation. We will use if for both.

In your project, create a file named `.docker/elasticsearch/elasticsearch.yml` with the following contents:

```yaml
cluster.name: 'middleware-cluster'
bootstrap.memory_lock: true
discovery.type: 'single-node'

http.host: '0.0.0.0'
http.port: 9200

# Uncomment the following lines for a production cluster deployment
#transport.host: 0.0.0.0
#discovery.zen.minimum_master_nodes: 1

http.cors.enabled : true
http.cors.allow-headers: 'X-Requested-With,X-Auth-Token,Content-Type,Content-Length,Authorization'
# Uncomment the following if you wish to open access to a 3rd-party application, like Dejavu.
#http.cors.allow-origin: "http://localhost:1234,http://127.0.0.1:1234"
http.cors.allow-credentials: true
http.cors.allow-methods : 'OPTIONS, HEAD, GET, POST, PUT, DELETE'

cluster.routing.allocation.disk.threshold_enabled: false
```

Now, you will need to add a new elasticsearch service and a dedicated volume in your `docker-compose.yaml` file:

```yaml
services:
  elasticsearch:
    image: docker.elastic.co/elasticsearch/elasticsearch-oss:7.8.0
    ports:
      - ${ELASTICSEARCH_PORT}:9200
    environment:
      - "ES_JAVA_OPTS=-Xms512m -Xmx512m"
    volumes:
      - elasticsearch:/usr/share/elasticsearch/data
      - ./.docker/elasticsearch/elasticsearch.yml:/usr/share/elasticsearch/config/elasticsearch.yml

volumes:
  elasticsearch:
    driver: local
```

#### Kibana

Kibana is web interface for ElasticSearch storage, capable of building graphic representations of aggregation searches.

In your project, create a file named `.docker/kibana/kibana.yml` with the following contents:

```yaml
server.port: 5601
server.host: "0.0.0.0"
elasticsearch.hosts: "http://elasticsearch:9200"
```

Now, you will need to add a new kibana service in your `docker-compose.yaml` file:

```yaml
services:
  kibana:
    image: docker.elastic.co/kibana/kibana-oss:7.8.0
    ports:
      - ${KIBANA_PORT}:5601
    environment:
      - monitoring.elasticsearch.hosts=http://elasticsearch:9200
    volumes:
      - ./.docker/kibana/kibana.yml:/usr/share/kibana/config/kibana.yml
```


