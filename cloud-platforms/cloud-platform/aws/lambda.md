# Lambda

#### **Asserts AWS Exporter**

The [Asserts AWS Exporter](../../../user-guide/integrations/aws.md) is required to export the throttle count metric and the function execution count metric at the function and region level. You can also export request and error metrics from CloudWatch, but it is recommended to use the lambda layer instead. To run it in the development environment

```
docker run --env-file env.properties -p 8010:8010 asserts/aws-cloudwatch-exporter.dev 
```

where env.properties has the AWS credentials

#### Asserts Lambda Layer

The Asserts Lambda layer needs to be added to the Lambda function. Lambda layers are available for [NodeJS](https://github.com/asserts/asserts-aws-lambda-layer-js) and [Python](https://github.com/asserts/aws-lambda-layer-python). Refer to the GitHub project documentation for instructions on how to install the layer and include it in one or more Lambda functions. There is a convenient utility script to add, remove, and upgrade the layer version in multiple Lambda functions in one go.

### Key Performance Indicators (KPIs) and Alerts

#### Request, Errors, and Latency

| **Metric**                                                   | **Key Performance Indicator (KPI)**                                                                                                                                                                                |
| ------------------------------------------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| <p>Request Counter</p><p>aws_lambda_invocations_total</p>    | <p>Request Rate</p><p>rate(aws_lambda_invocations_total[5m])</p>                                                                                                                                                   |
| <p>Error Counter</p><p>aws_lambda_errors_total</p>           | <p>Error Ratio</p><p>rate(aws_lambda_invocations_total[5m])/ rate(aws_lambda_invocations_total[5m])</p>                                                                                                            |
| <p>Latency Histogram</p><p>aws_lambda_duration_seconds</p>   | <p>Latency Average</p><p>rate(aws_lambda_duration_seconds_sum[5m])/ rate(aws_lambda_duration_seconds_count[5m])</p><p>Latency P99</p><p>histogram_quantile(0.99, sum(rate(aws_lambda_duration_seconds_sum[5m])</p> |
| <p>Request Throttle Count</p><p>aws_lambda_throttles_sum</p> | <p>Observe throttle count for Sustained Throttling</p><p>aws_lambda_throttles_sum</p>                                                                                                                              |

#### Resource

| **Metric**                                                                                                                | **Key Performance Indicator (KPI)**                                                                                                                                                                                                                                 |
| ------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| <p>Memory NodeJS</p><p>process_heap_bytes</p><p>Memory Python</p><p>process_resident_memory_bytes</p>                     | <p><strong>NodeJS</strong></p><p>process_heap_bytes / aws_lambda_memory_limit_mb</p><p><strong>Python</strong></p><p>process_resident_memory_bytes / aws_lambda_memory_limit_mb</p>                                                                                 |
| <p>CPU Total Time</p><p>aws_lambda_cpu_total_time_sum</p>                                                                 | <p>CPU Time Usage</p><p>avg_over_time(aws_lambda_cpu_total_time_sum[5m)</p>                                                                                                                                                                                         |
| <p>Network Bytes Received</p><p>aws_lambda_rx_bytes_sum</p><p>Network Bytes Transmitted</p><p>aws_lambda_tx_bytes_sum</p> | <p>Data transfer rate</p><p>avg_over_time(aws_lambda_rx_bytes_sum[5m)</p><p>avg_over_time(aws_lambda_tx_bytes_sum[5m)</p>                                                                                                                                           |
| <p>Concurrent Executions</p><p>aws_lambda_concurrent_executions_avg</p>                                                   | <p>When concurrency is reserved at the function level</p><p>aws_lambda_concurrent_executions_avg / aws_lambda_allocated_concurrency</p><p>For account level</p><p>aws_lambda_concurrent_executions_avg / aws_lambda_account_limit{type="concurrent_executions"}</p> |

**Alerts**

| **KPI**                | **Alert**                                                                                                                   |
| ---------------------- | --------------------------------------------------------------------------------------------------------------------------- |
| Request Rate           | **RequestRateAnomaly**                                                                                                      |
| Error Ratio            | **ErrorRatioBreach** and **ErrorBuildup** based on an availability SLO of 99.9                                              |
| Latency Average        | **LatencyAverageBreach** and **LatencyAverageAnomaly**                                                                      |
| Latency P99            | **LatencyP99ErrorBuildup**                                                                                                  |
| Request Throttle Count | **LambdaFunctionThrottled**                                                                                                 |
| Memory Utilization     | **Saturation** with severity level of **warning** and **critical** when memory utilization exceeds 90% and 95% respectively |
| CPU Time Usage         | **ResourceRateAnomaly**                                                                                                     |
| Network Bytes          | **ResourceRateAnomaly**                                                                                                     |
| Lambda Concurrency     | **Saturation** at Function and Account level                                                                                |

### KPI Dashboard

The Lambda KPI Dashboard shows all the above mentioned KPIs

![](../../../.gitbook/assets/Blog\_KPI\_Dashboard.png)
