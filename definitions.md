## math

### rate

A comparison of two related quantities.

Often the second quantity is time (per second, per hour, etc) but it can be anything.

### quantiles

 a quantile is where a sample is divided into equal-sized, adjacent, subgroups (that's why it's sometimes called a “fractile“). It can also refer to dividing a probability distribution into areas of equal probability.

### median

is quantile that split the date into groups that contains the same number of data points.
this is called 0.5 quantile

### percentile

Percentiles are given as percent values, values such as 95%, 40%, or 27%. Quantiles are given as decimal values, values such as 0.95, 0.4, and 0.27. The 0.95 quantile point is exactly the same as the 95th percentile point.

### why 0.95 quantile matters ?

The 95th percentile basically says that 95 percent of the time your usage is at or below it, and the other 5 percent of the time it exceeds it.
-> <https://opmantek.com/what-is-the-95th-percentile-and-why-does-it-matter/>

## metric && time series

In layperson terms, metrics are nuquantilesmeric measurements. Time series means that changes are recorded over time. What users want to measure differs from application to application. For a web server it might be request times, for a database it might be number of active connections or number of active queries etc.

## pull model vs push model

There are two different methods when thinking about how metrics are ingested into a monitoring system: either the metrics get pushed (usually via UDP) into the system or they get pulled (usually via HTTP).

### pull based

The Pull-based monitoring system, as the name implies, is a monitoring system that actively obtains indicators, and the objects that require monitoring need to have the capability to be accessed remotely.

![](uploads/2022-07-21-21-51-48.png)

advantages of pull based model:

- easy to use
- service discovery
- centrilized configuration
- easy to encrypt traffic
- tcp pull based make sure that you have a minimum data loss

### push based

Push-based monitoring systems do not actively obtain data, but the monitored objects actively push indicators.

![](uploads/2022-07-21-21-52-11.png)

advantages of push based model:

- for a complex networking environnement, this strategy is the best suited . (no need to open ports to pull metrics)
- less cost than pull
- easier to manage when you have multiple ingesters

## multi demensional metric

as systems have become more and more complex, the concept of dimensional metrics, that is, metrics that also include a set of tags or labels (i.e., the dimensions) to provide additional context, emerged. Monitoring systems that support dimensional metrics allow engineers to easily aggregate and analyze a metric across multiple components and dimensions by querying for a specific metric name and filtering and grouping by label.
![](uploads/2022-07-22-01-07-53.png)
-> <https://www.timescale.com/blog/four-types-prometheus-metrics-to-collect/>

## Metric cardinality

Metric cardinality is another important concept, and refers to the number of unique combinations of labels that can be generated from a particular metric.

if you have a metric `server_responses` that will have two labels:

- env with cardinality of 2 (two possible values)
- satuts_code with cardinality of 5

the metric cardinality will be: 5x2 = 10

![](uploads/2022-07-22-17-18-09.png)

> you always make sure that your cardinality is not too high to avoid performance issues (the maximum recomanded cardinality is 100 000 )

-> <https://promcon.io/2019-munich/slides/containing-your-cardinality.pdf>
-><https://grafana.com/blog/2022/02/15/what-are-cardinality-spikes-and-why-do-they-matter/#:~:text=Cardinality%20spike%3A%20Basic%20diagram%20of,the%20metric%20server_responses%20is%2010>.

## traces and spans?

A trace is a collection of operations that represents a unique transaction handled by an application and its constituent services. A span represents a single operation within a trace. Each span stores the start time, duration, and parent_id. Spans without a parent_id are rendered as root spans.

A distributed trace connects calling relationships among events in distributed services. For example, Service A calls Service B, which makes a database query and then hits a third-party API.

## events && logs

- Logs are human-readable flat text files that are used by developers to capture useful data. Logs messages occur at a single point in time (though not necessarily at every point in time).
- Events are structured logs. They follow a standardized format (JSON),

# sla, slo, sli

## sla

An SLA (service level agreement) is an agreement between provider and client about measurable metrics like uptime, responsiveness, and responsibilities.

## slo

An SLO (service level objective) is an agreement within an SLA about a specific metric like uptime or response time

## sli

An SLI (service level indicator) measures compliance with an SLO (service level objective). So, for example, if your SLA specifies that your systems will be available 99.95% of the time, your SLO is likely 99.95% uptime and your SLI is the actual measurement of your uptime.
several approaches for this:

    RED\: Rate, Errors, Duration - introduced by @tom_wilkie
    USE\: Utilization, Saturation, and Errors - introduced by @brendangreggs

###

-> <https://www.alibabacloud.com/blog/pull-or-push-how-to-select-monitoring-systems_599007>
-> <https://giedrius.blog/2019/05/11/push-vs-pull-in-monitoring-systems/>
