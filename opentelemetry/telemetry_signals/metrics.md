### The Four Common Types of Metrics: Counters, Gauges, Histograms, and Summaries

1. Counter - Cumulative metrics 
     - Ex:- (Total requests served)

2. Gauge - Current state snapshot
     - Ex:- (Current Memory Usages)

3. Histogram - Distribution of values
     - Ex:- Latency metrics (p50, p95, p99 etc.)

4. Summaries - Histograms + Percentiles


---

### Metrics Dimension

`Metrics Dimensions` are also called `labels` and `tags` to add context:

```yaml
http_requests_total{method="GET", endpoint="/api/users", status="200"} = 15420
http_requests_total{method="POST", endpoint="/api/users", status="201"} = 892
http_requests_total{method="GET", endpoint="/api/users", status="404"} = 23

```

### Metrics Features

- Metrics is aggregated value.


   
