**IMPLEMENTATION JOURNAL \- OpenSearch, Prometheus, and Grafana Integration**

| Submitted By | Devansh Ditya Dubey |
| :---- | :---- |
| Submitted To | Mr. Vipin Tripathi |
| Test Case Version | 1 |
| Reviewer  Name | Mr. Vipin Tripathi |

**Goal**

The objective of this implementation is to set up OpenSearch, Prometheus, and Grafana in a containerized environment using Podman Compose. This setup enables monitoring of OpenSearch performance through Prometheus and visualization of metrics in Grafana. The key functionalities include:

* Deploying OpenSearch for indexing and querying data.  
    
* Configuring Prometheus to collect metrics from OpenSearch Exporter.  
    
* Setting up Grafana to visualize OpenSearch performance metrics.  
    
* Performing CRUD operations on OpenSearch and monitoring their impact through dashboards.

**Table of Contents**

**Prerequisites:**

**Step 1: Setting up Podman Compose**
**Step 2: Running the Containers**

**Step 3: Configuring Prometheus**

**Step 4: Setting up Grafana**

**Step 5: Performing CRUD Operations on OpenSearch**

**Step 6: Monitoring OpenSearch Performance**

# **Prerequisites:**

**Hardware Requirements**

* Minimum 8GB RAM  
* 4-core CPU  
* 50GB available storage

**Software Requirements**

* Ubuntu or any Linux-based OS  
* Podman installed  
* Podman Compose installed  
* Postman (for CRUD operations)

**Network Requirements**

* Ports required:  
* OpenSearch: 9200, 9600  
* Prometheus: 9090  
* Grafana: 3000  
* OpenSearch Exporter: 9114

**Steps**

# **Step 1: Setting up Podman Compose**

Command Executed

$ vim podman-compose.yml

```
# podman-compose.yml Content

version: "3.8"
services:
  opensearch:
	image: docker.io/opensearchproject/opensearch:latest
	container_name: opensearch
	ports:
  	- "9200:9200"
  	- "9600:9600"
	environment:
  	- discovery.type=single-node
  	- plugins.security.disabled=true
  	-OPENSEARCH_INITIAL_ADMIN_PASSWORD=Devansh123@
  	-OPENSEARCH_PROMETHEUS_EXPORTER_ENABLED=true
	volumes:
  	- opensearch-data:/usr/share/opensearch/data

opensearch-exporter:
	image: quay.io/prometheuscommunity/elasticsearch-exporter:latest
	container_name: opensearch-exporter
	command:
  	- '--es.uri=http://admin:Devansh123@@opensearch:9200'
	ports:
  	- "9114:9114"
	depends_on:
```
  	
```
- opensearch


 
  prometheus:
	image: docker.io/prom/prometheus:latest
	container_name: prometheus
	ports:
  	- "9090:9090"
	volumes:
  	- ./prometheus.yml:/etc/prometheus/prometheus.yml
  	- prometheus-data:/prometheus

  grafana:
	image: docker.io/grafana/grafana:latest
	container_name: grafana
	ports:
  	- "3000:3000"
	volumes:
  	- grafana-data:/var/lib/grafana

volumes:
  opensearch-data:
  prometheus-data:
  Grafana-data:
```
![Images](Images/image14.png)

# **Step 2: Running the Containers** 

Command Executed

$ podman compose up \-d

Verify Deployment

$ podman ps \-a  
Output

```
$ podman ps
CONTAINER ID  IMAGE                                                  	COMMAND           	CREATED 	STATUS  	PORTS                                       	NAMES
1702a462e2e3  docker.io/opensearchproject/opensearch:latest          	opensearch        	4 days ago  Up 3 hours  0.0.0.0:9200->9200/tcp, 0.0.0.0:9600->9600/tcp  opensearch
8dc5d6a8593d  docker.io/prom/prometheus:latest                       	--config.file=/et...  4 days ago  Up 3 hours  0.0.0.0:9090->9090/tcp                      	prometheus
4998d1a3dc2c  docker.io/grafana/grafana:latest                                             	4 days ago  Up 3 hours  0.0.0.0:3000->3000/tcp                      	grafana
e9ea813ad95f  quay.io/prometheuscommunity/elasticsearch-exporter:latest  --es.uri=http://a...  4 days ago  Up 3 hours  0.0.0.0:9114->9114/tcp                      	opensearch-exporter


```
![Images](Images/image1.png)

# **Step 3: Configuring Prometheus**

Command Executed

$ vim prometheus.yml

```
# prometheus.yml Content

global:
  scrape_interval: 15s

scrape_configs:
  - job_name: 'opensearch-exporter'
	metrics_path: '/metrics'
	static_configs:
  	- targets: ['opensearch-exporter:9114']
```
![Images](Images/image15.png)

# **Step 4: Setting up Grafana**

```
Open Grafana at http://localhost:3000

Configure Prometheus as a data source

Import the Elasticsearch Exporter Quickstart and Dashboard JSON file

Dashboard includes:

Cluster Health

CPU and Memory Usage

Disk Usage
Query & Indexing Time

```
![Images](Images/images16.png)

# 

# 

# **Step 5: Performing CRUD Operations on OpenSearch**

Create an Index

```
PUT http://localhost:9200/devansh
Content-Type: application/json
{
  "settings": {
	"index": {
  	"number_of_shards": 1,
  	"number_of_replicas": 1
	}
  },
  "mappings": {
	"properties": {
  	"title": { "type": "text" },
  	"description": { "type": "text" },
  	"price": { "type": "float" },
  	"created_at": {
    	"type": "date",
    	"format": "strict_date_optional_time||epoch_millis",
    	"null_value": "2024-02-05T00:00:00Z"
  	}
	}
  }
}
```
![Images](Images/image6.png)

Insert Document

```
POST http://localhost:9200/devansh/_doc
```

```
Content-Type: application/json
{
  "title": "Devansh1",
  "description": "My Name is Devansh",
  "price": 200.00
}
```
![Images](Images/image8.png)

Fetch Documents

```
GET http://localhost:9200/devansh/_search
```
![Images](Images/image9.png)

Search by Title

```
GET http://localhost:9200/devansh/_search
{
  "query": {
	"match": {
  	"title": "Devansh1"
	}
  }
}
```
Update Documents

# 

# **Step 6: Monitoring OpenSearch Performance**

```
Open Prometheus UI at http://localhost:9090

Check if Elasticsearch Exporter is up

Open Grafana Dashboard to visualize the OpenSearch performance metrics
```
**Conclusion**

This implementation successfully integrates OpenSearch with Prometheus and Grafana for monitoring and visualization. The OpenSearch instance allows CRUD operations, which can be tracked in real-time via the Grafana dashboard. This setup provides essential insights into cluster health, resource usage, and query performance, making it a robust solution for search-based applications.