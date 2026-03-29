# Prometheus 
-  A pull based monitoring tool used to collect time series metrics and data from the target systems.
- These collected metrics and data is further used to analyse the system status and performance.
- Also, provide alert system to make the target system work efficiently.
- It doesn't help with collecting any logs and traces as other  tools do.

## Prometheus Architecture

`Prometheus Architecture Diagram`

 ![images](/workspaces/monitoring_tools_sre/prometheus/Prometheus_architecture.png)

Copyright@ KodeKloud

## Prometheus Components

1. Prometheus server

2. Exporter 

3. TSDB (Time Series Database)

4. Alert Manager

5. Service Discovery mechanism

6. Query



### Prometheus Configuration 

1. Define a Job with name of nodes
2. Metrics should be scraped  every 30s
3. Timeout of a scrape is 3 s
4. For Scheme use HTTPS instead of HTTP 
5. Scrape path should be changed from default/metrics to /stats/metrics (you can customize the path from where you scrape the metrics)
6. Must specify the Scrape targets with ports
   - IP Address:port_no
   - IP Address:oprt_no

```sh 
  job_name: 'node'
  scrape_interval: 15s     # Config for scrape job. Takes precedenc over global config
  scrape_timeout: 5s
  sample_limit: 1000
  static_config:
    - targets: [ ]    # Sets of targets to be scraped

```

#### scrape configs

```sh
scrape_config:
  job_name: 'node'
  scrape_interval: 15s     # Config for scrape job. Takes precedenc over global config
  scrape_timeout: 5s
  sample_limit: 1000
  static_config:
    - targets: [ ]    # Sets of targets to be scraped
  
  basic_auth:    # This section allow to config authentication
     
```