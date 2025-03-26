# **TEST CASES: OpenSearch Monitoring Stack**

| Submitted By | Devansh Ditya Dubey |
| :---- | :---- |
| Submitted To | Mr.Vipin Tripathi |
| Test Case Version | 1 |
| Reviewer  Name | Ms. Manmeet Narang |

**Goal**

The objective of this project is to set up OpenSearch, Prometheus, and Grafana using Podman on Ubuntu 24.04. The system should:

* Run OpenSearch as a single-node cluster with Prometheus exporter enabled.  
* Integrate OpenSearch with Prometheus for monitoring.  
* Use Grafana to visualize OpenSearch metrics from Prometheus.  
* Ensure proper functionality by inserting and removing data via Postman.  
* Validate the services are correctly running and interconnected.

### 

**Table of Content:**

1. [Test Environment](https://chatgpt.com/c/67c97539-d1a8-8013-aab9-b6db0d3bff8c#test-environment)  
2. [TC1: OpenSearch Container Startup](https://chatgpt.com/c/67c97539-d1a8-8013-aab9-b6db0d3bff8c#tc1-opensearch-container-startup)  
3. [TC2: OpenSearch API Accessibility](https://chatgpt.com/c/67c97539-d1a8-8013-aab9-b6db0d3bff8c#tc2-opensearch-api-accessibility)  
4. [TC3: Prometheus Exporter Metrics Availability](https://chatgpt.com/c/67c97539-d1a8-8013-aab9-b6db0d3bff8c#tc3-prometheus-exporter-metrics-availability)  
5. [TC4: Prometheus Integration](https://chatgpt.com/c/67c97539-d1a8-8013-aab9-b6db0d3bff8c#tc4-prometheus-integration)  
6. [TC5: Grafana Dashboard Configuration](https://chatgpt.com/c/67c97539-d1a8-8013-aab9-b6db0d3bff8c#tc5-grafana-dashboard-configuration)  
7. [TC6:](https://chatgpt.com/c/67c97539-d1a8-8013-aab9-b6db0d3bff8c#tc6-data-insertion--retrieval-in-opensearch)Create index in OpenSearch Using Postman  
8. TC7:Insert Document in OpenSearch Using Postman  
9. TC8:Retrieve Document from OpenSearch Using Postman  
10. TC9:Update Document in OpenSearch Using Postman  
11. TC10:Delete Document from OpenSearch Using Postman  
12. System Load Monitoring During CRUD Operations.  
13. NFR Test Cases  
      
    	

## **Test Environment**

* Ubuntu 24.04  
* Podman and Podman Compose installed  
* OpenSearch running as a container  
* Prometheus collecting metrics from OpenSearch  
* Grafana visualizing OpenSearch metrics  
* Postman for API testing  
* Accessible URLs:  
  * OpenSearch: [`http://localhost:9200`](http://localhost:9200/)  
  * Prometheus: [`http://localhost:9090`](http://localhost:9090/)  
  * Prometheus Exporter: [`http://localhost:9114/metrics`](http://localhost:9114/metrics)  
  * Grafana: [`http://localhost:3000`](http://localhost:3000/)

### 

### 

### **Test Case 1: Valid Request GET to Fetch All Records**

|  Scenario |  | Verify that the OpenSearch container starts successfully without errors. |  |  |  |
| :---- | ----- | :---- | :---- | :---- | :---- |
| **Remarks** |  | **Ensuring OpenSearch initializes properly is critical for further integrations** |  |  |  |
| **Given** |  | **Podman is installed. And The `podman-compose.yml` file is correctly configured.**  |  |  |  |
| **When** |  | **The user runs `podman-compose up -d`.** |  |  |  |
|  **Then**  |  | **The OpenSearch container is running. And Running `podman ps` lists `opensearch` as an active container. And Logs should show no critical errors.** Checked |  |  |  |
| **Test Run** |  | **Date** |  | **Result** |  |

![Images](images/image1.png)


### **Test Case 2: OpenSearch API Accessibility**

| Scenario |  | Verify that OpenSearch is accessible via API calls. |  |  |  |
| :---- | ----- | :---- | :---- | :---- | :---- |
| **Remarks**  |  | **If OpenSearch is not responding, the entire integration fails.** |  |  |  |
| **Given** |  | **OpenSearch is running.** |  |  |  |
| **When** |  | **The user executes `curl -X GET`** `http://localhost:9200`. |  |  |  |
| **Then** |  | **The response includes OpenSearch version details.** Checked |  |  |  |
| **Test Run** |  | **Date** |  | **Result** |  |
![Images](images/image2.png)

### **Test Case 3: Valid Request PUT to Update a Record**

| Scenario |  | Verify that OpenSearch metrics are exposed via Prometheus Exporter |  |  |  |
| :---- | ----- | :---- | :---- | :---- | :---- |
| **Remarks** |  | **Without proper metric exposure, Prometheus cannot collect data.** |  |  |  |
| **Given** |  | **OpenSearch and Prometheus Exporter are running.** |  |  |  |
| **When** |  | **The user accesses** [`http://localhost:9114/metrics`](http://localhost:9114/metrics). |  |  |  |
| **Then** |  | **The page displays OpenSearch metrics in Prometheus format.** Checked |  |  |  |
| **Test Run** |  | **Date** |  | **Result** |  |
![Images](images/image3.png)


### **Test Case 4: Prometheus Integration**

|  Scenario |  | Verify that Prometheus is scraping metrics from OpenSearch Exporter. |  |  |  |
| :---- | ----- | :---- | :---- | :---- | :---- |
| **Remarks** |  | **Prometheus must successfully collect OpenSearch metrics.** |  |  |  |
| **Given** |  | **OpenSearch Exporter is exposing metrics. And Prometheus is running with correct `prometheus.yml` configuration.** |  |  |  |
| **When** |  | **The user accesses** [`http://localhost:9090/targets`](http://localhost:9090/targets). |  |  |  |
| **Then** |  | **The target `opensearch-exporter:9114` is in an `UP` state.**  checked  |  |  |  |
| **Test Run** |  | **Date** |  | **Result** |  |
| ![][image3] |  |  |  |  |  |

### 

### 

### 

### **Test Case 5: Grafana Dashboard Configuration**

| Scenario |  | Verify that OpenSearch metrics appear in Grafana. |  |  |  |
| :---- | ----- | :---- | :---- | :---- | :---- |
| **Remarks** |  | **Grafana must be able to fetch Prometheus data.** |  |  |  |
| **Given** |  | **Grafana is running and accessible at** [`http://localhost:3000`](http://localhost:3000/). **And** **Prometheus data source is correctly added.**  |  |  |  |
| **When** |  | **The user configures a dashboard and adds a panel for OpenSearch metrics.** |  |  |  |
|  **Then** |  | **The graph displays OpenSearch metrics over time.** checked |  |  |  |
| **Test Run** |  | **Date** |  | **Result** |  |
| ![][image4] |  |  |  |  |  |

### **Test Case 6: Create Index in OpenSearch Using Postman**

| Scenario |  | Verify that a new index can be created in OpenSearch using Postman. |  |  |  |
| :---- | ----- | :---- | :---- | :---- | :---- |
| **Remarks** |  | **Creating an index is essential for data storage.** |  |  |  |
| **Given** |  |  **OpenSearch is running.** |  |  |  |
| **When** |  | **Request: Method: PUT URL: `http://localhost:9200/devansh` Headers: `Content-Type: application/json` Body: {   "settings": {     "index": {       "number\_of\_shards": 1,       "number\_of\_replicas": 1     }   },   "mappings": {     "properties": {       "title": { "type": "text" },       "description": { "type": "text" },       "price": { "type": "float" },       "created\_at": {          "type": "date",         "format": "strict\_date\_optional\_time||epoch\_millis",         "null\_value": "2024-02-05T00:00:00Z"       }     }   } }** |  |  |  |
| **Then** |  | **Index `devansh` is created successfully. And Verify with GET `http://localhost:9200/_cat/indices`.** Checked |  |  |  |
| **Test Run** |  | **Date** |  | **Result** |  |
| **![][image5]  ![][image6]**  |  |  |  |  |  |

### **Test Case 7:  Insert Document in OpenSearch Using Postman**

| Scenario |  |  Verify that a document can be inserted into OpenSearch using Postman.  |  |  |  |
| :---- | ----- | :---- | :---- | :---- | :---- |
| **Remarks** |  |  **Data insertion is a core functionality.** |  |  |  |
|  **Given** |  |  **OpenSearch and `devansh` index are available.** |  |  |  |
| **When** |  | **Request: Method: POST URL: `http://localhost:9200/devansh/_doc` Headers: `Content-Type: application/json`           Body: {   "title": "Devansh1",   "description": "My Name is Devansh",   "price": 200.00 }** |  |  |  |
| **Then** |  | **Document is inserted successfully.  And Verified with GET `http://localhost:9200/devansh/_search`.** Checked |  |  |  |
| **Test Run** |  | **Date** |  | **Result** |  |
| ![][image7] |  |  |  |  |  |

### **Test Case 8: Retrieve Document from OpenSearch Using Postman**

| Scenario |  |  Verify that documents can be retrieved from OpenSearch using Postman.  |  |  |  |
| :---- | ----- | :---- | :---- | :---- | :---- |
| **Remarks** |  |  **Retrieving documents ensures data accessibility.** |  |  |  |
| **Given** |  |  **OpenSearch is running with data inserted in `devansh` index.** |  |  |  |
| **When** |  | **Request: Method: GET URL: `http://localhost:9200/devansh/_search`** |  |  |  |
| **Then** |  | **Inserted documents are returned correctly.** Checked |  |  |  |
| **Test Run** |  | **Date** |  | **Result** |  |
| ![][image8] |  |  |  |  |  |

### 

### **Test Case 9:Update Document in OpenSearch Using Postman** {#test-case-9:update-document-in-opensearch-using-postman}

| Scenario |  |  Verify that a document can be updated in OpenSearch using Postman.  |  |  |  |
| :---- | ----- | :---- | :---- | :---- | :---- |
| **Remarks** |  | **Updating documents allows modification of existing data.** |  |  |  |
| **Given** |  |  **OpenSearch is running with the document inserted in `devansh`.** |  |  |  |
| **When** |  |  **Find Document ID: Request: GET `http://localhost:9200/devansh/_search` with query: {   "query": {     "match": { "title": "Devansh1" }   } } Extract the `_id` from the response. Update Document: Request: POST URL: `http://localhost:9200/devansh/_update/{document_id}` Body: {   "doc": {     "price": 129.99   } } Verify Update: GET `http://localhost:9200/devansh/_search`.**  |  |  |  |
| **Then** |  | **Document’s price is updated to `129.99`.** Checked |  |  |  |
| **Test Run** |  | **Date** |  | **Result** |  |
| ![][image9] |  |  |  |  |  |

### **Test Case 10: Delete Document from OpenSearch Using Postman**

| Scenario |  |  Verify that documents can be deleted from OpenSearch using Postman.  |  |  |  |
| :---- | ----- | :---- | :---- | :---- | :---- |
| **Remarks** |  |  **Deleting documents is crucial for data cleanup.** |  |  |  |
| **Given** |  |  **OpenSearch is running with the document inserted in `devansh`.** |  |  |  |
| **When** |  | **Request: POST URL: `http://localhost:9200/devansh/_delete_by_query` Body: {   "query": {     "match": { "title": "Devanshi" }   } }** |  |  |  |
| **Then** |  | **Document is deleted successfully. And Verify with GET `http://localhost:9200/devansh/_search` (returns no results).**  Checked |  |  |  |
| **Test Run** |  | **Date** |  | **Result** |  |
| ![][image10] |  |  |  |  |  |

### 

### **Test Case 11: System Load Monitoring During CRUD Operations.**

| Scenario |  | Verify the Grafana dashboard reflects system load during CRUD operations matching previous test cases.  |  |  |  |
| :---- | ----- | :---- | :---- | :---- | :---- |
| **Remarks** |  | **Consistent data structure ensures comparable metrics.** |  |  |  |
| **Given** |  | **Grafana dashboard is monitoring OpenSearch Prometheus exporter is active** |  |  |  |
| **When** |  | **The user sequentially performs all CRUD operations from Test Cases 6-10: Creates index (TC6) Inserts 100 documents (TC7) Retrieves documents (TC8) Updates documents (TC9) Deletes documents (TC10)**  |  |  |  |
| **Then** |  | **The Grafana dashboard displays: Real-time system metrics during each operation phase CPU/Memory usage proportional to operation intensity Successful completion of all operations Return to baseline metrics after operations complete** |  |  |  |
| **Test Run** |  | **Date** |  | **Result** |  |
| ![][image11] |  |  |  |  |  |

## **NFR Test Cases**

| Scenario |  |  Ensure the system meets non-functional requirements like performance and availability |  |  |  |
| :---- | ----- | :---- | :---- | :---- | :---- |
| **Remarks** |  | **The system should handle multiple queries efficiently.** |  |  |  |
| **Given** |  | **The system is deployed.** |  |  |  |
| **When** |  | **Stress tests are performed (e.g., using Apache JMeter).** |  |  |  |
| **Then** |  | **OpenSearch maintains a reasonable response time.** Checked |  |  |  |
| **Test Run** |  | **Date** |  | **Result** |  |
