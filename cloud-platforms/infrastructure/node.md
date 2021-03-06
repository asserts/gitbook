# Node

### Setup

The [Node Exporter](https://github.com/prometheus/node\_exporter) needs to be installed for monitoring nodes.

### Metrics and Key Performance Indicators (KPI)s

| **Metric**                                                                                                                                                                                                                                                                                                                                                                                        | **KPI**                                                                                                                                                                                                                                                                                 |
| ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| <p>CPU</p><p>node_cpu_seconds_total</p>                                                                                                                                                                                                                                                                                                                                                           | 1 - avg by(instance, job)(rate(node\_cpu\_seconds\_total{mode="idle"}\[5m]))                                                                                                                                                                                                            |
| <p>Memory</p><p>node_memory_MemTotal_bytes</p><p>node_memory_Buffers_bytes</p><p>node_memory_Cached_bytes</p><p>node_memory_MemFree_bytes</p><p>node_memory_Slab_bytes</p><p>node_vmstat_pgmajfault</p>                                                                                                                                                                                           | <p>Memory Utilization</p><p>1 - (buffer + cached + free + slab)/total</p><p>Page Fault Rate</p><p>rate(node_vmstat_pgmajfault[1m])</p>                                                                                                                                                  |
| <p>Network Bytes</p><p>node_network_receive_bytes_total</p><p>node_network_transmit_bytes_total</p>                                                                                                                                                                                                                                                                                               | <p>Network Byte Rate</p><p>rate(node_network_receive_bytes_total[5m])</p><p>rate(node_network_transmit_bytes_total[5m])</p>                                                                                                                                                             |
| <p>Disk</p><p>node_filesystem_avail_bytes</p><p>node_filesystem_size_bytes</p><p>Read/Write byte rate</p><p>node_disk_read_bytes_total</p><p>node_disk_written_bytes_total</p><p>Read Time and Count</p><p>node_disk_read_time_seconds_total</p><p>node_disk_reads_completed_total</p><p>Write Time and Count</p><p>node_disk_write_time_seconds_total</p><p>node_disk_writes_completed_total</p> | <p>Disk Utilization</p><p>1 - available bytes / size bytes</p><p>Disk IO Rate</p><p>rate(node_disk_read_bytes_total[5m])</p><p>rate(node_disk_written_bytes_total[5m])</p><p>Disk Average Latency</p><p>rate(...time_seconds_total[5m])</p><p>/</p><p>rate(..._completed_total[5m])</p> |

### Alerts

| **KPI**                                                 | **Alert**                                                                                                                                       |
| ------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------- |
| <p>Memory Utilization</p><p>High Memory Page Faults</p> | <p><strong>Saturation</strong> with resource_type=memory:utilization</p><p><strong>Saturation</strong> with resource_type=memory:page_fault</p> |
| CPU Utilization                                         | **Saturation**                                                                                                                                  |
| Network Bytes Rate                                      | **ResourceRateAnomaly**                                                                                                                         |
| Disk Utilization                                        | **Saturation**                                                                                                                                  |
| Disk Read/Write Rate                                    | **ResourceRateAnomaly**                                                                                                                         |
| Disk Read/Write Latency Average                         | **Saturation** when latency average breaches 100ms                                                                                              |

### Dashboard

The following KPIs are shown in the Node Dashboard

![](<../../.gitbook/assets/image (21) (1).png>)
