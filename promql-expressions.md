size of all filesystems mounted under /run but not /run/lock:
![](uploads/2022-07-23-23-12-50.png)

```promql
node_filesystem_files{job='node',mountpoint=~"/run.*",mountpoint!~"/run/lock"}
```

![](up>loads/2022-07-23-23-13-37.png)

the average number of file descriptors across all your jobs :

![](uploads/2022-07-24-13-34-33.png)

```promql
avg without(instance,job) (process_open_fds)
```

![](uploads/2022-07-24-13-36-13.png)

- the amount of traffic received per second

```promql
rate(node_network_receive_bytes_total[5m])
```

- the total bytes received per machine

```promql
sum without(device) (rate(node_network_receive_bytes_total[5m]))
```

![](uploads/2022-07-24-13-46-50.png)

- explain request
  ![](uploads/2022-07-24-13-54-12.png)

  -> total per second prometheus http request rate.

- average request duration in the past 5 minutes

```promql
sum without(handler) (rate(prometheus_http_request_duration_seconds_sum[5m]))
/
sum without(handler) (rate(prometheus_http_request_duration_seconds_count[5m]))
```

- explain :

![](uploads/2022-07-24-15-31-02.png)

-> 0.9 quantiles latency of prometheus's compaction latency over the past day.

- count the number of CPUs per instance
![](uploads/2022-07-24-19-09-19.png)

```promql
count by(instance)(count without(mode) (node_cpu_seconds_total))
```

![](uploads/2022-07-24-19-09-59.png)
> for the inner count you can also use sum since we don't carry about the value

- get the number of processes for each job that have more than 10 open file descriptors
![](uploads/2022-07-25-01-34-38.png)

```promql
sum without(instance)(process_open_fds > bool 10)
```

![](uploads/2022-07-25-01-36-39.png)

the inner query will output 1 if the process have more than 10 fds or 0 if less. we will than just sum the processes with an aggregation on instance.

- explain
![](uploads/2022-07-25-02-17-07.png)

-> get all the instances that have there open file descriptors more the 0.0001 of the max file descriptors.

## notes

- when using counter, tou must use rate befor agregating
- when calculating average, it is important that you aggregate up the sum and count, and only on the last step perform a division otherwise you could end up averaging averages, which is not statistically valid.
- it's not possible to aggregate the quantiles of a summary
- quantile cannot be aggregated or have arithmetic performed
- always rate buckets exposed by histogram metric since histogram_quantile needs gauges to work
