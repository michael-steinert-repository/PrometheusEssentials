# Prometheus
* Prometheus is a Monitoring Tool for highly dynamic Container Environments (but can also be used in bare Server where the Application is directly deployed on it)
* It provides Information about Hardware and Application Level of the System
* It is stand-alone and self-containing, so it works, even other Parts of the Infrastructure are broken
* Prometheus constantly monitor all Services, and alert when they crash, or identify Problems before
* Example Rule: Monitor all Services, and notify if one runs over 70% Memory Usage over one Hour => Automated Monitoring and Alerting

## Prometheus Architecture

<p align="center">
    <img src="https://user-images.githubusercontent.com/29623199/135463008-76017d8f-8e62-44fe-9413-2d1870307750.JPG" alt="Prometheus Architecture" width="75%"/>
</P>

<hr>

### Prometheus Server
* Prometheus Server does the actual Monitoring Work
* It has a Time Series Database, that stores Metrics Data (for Example: CPU Usage or Number of Exceptions from an Application)
* It has a Data Retrieval Worker, that pulls Metrics Data from Applications and push them to  the Database
* It has a HTTP Server, that accepts PomQL Queries for the sored Data and its API is sued to display Data in a Web UI (Prometheus or Grafana)
  * Grafana is a Visualization Tool that uses PromQL to visualize the Metrics
  * "http_requests_total{status!~"4.."}": PromQL that queries all HTTP Status Codes except 4XX ones 
  * "rate(http_requests_total[2m])[42m:]": PromQL that returns the 2-Minute Rate of the http_requests_total Metric for the past 42min 

### Targets and Metrics
* Prometheus monitor Targets (for Example Web Server or Database)
* An Unit, that is monitored of a Target is called Metric (for Example Memory Usage or CPU Status)
* Metrics can be displayed as
  * Counter: How many Times Memory Usage over 70% happened?
  * Gauge: What is the current Value of Memory Usage?
  * Histogram: How long or high was the Memory Usage?
* Prometheus pulls the Metrics from a HTTP Endpoint that is exposed from an Application under the Endpoint "/metrics"
* Exporter are used to fetch Metrics from the Application, convert them to the correct Format and expose them to the Endpoint "/metrics"

### Alert Manager
* The Prometheus Server pushes Alerts the Alert Manager
* The Alert Manager uses the Alert Rules from the Configuration and notifies via Email, Slack, etc.

## Prometheus Configuration
* In the prometheus.yaml is the Configuration of Targets and Intervals to scrape from Prometheus
* __global__: Configuration how often Prometheus will scrape its Targets, and check its Rules
* __rules_files__: Rules for aggregating Metric Values or creating Alerts when their Condition met
* __scrape_configs__: Defining which Targets should be me monitored
