# Lucidity

A new take on an Observability platform.

We seek to return to the simplicity of Graphite while also solving Observability problems that still exists in today's world.

## Problem 1: Metrics and logging are still too expensive.

We spend crazy amount of money storing data that's looked at 0.5% of the time.

And the more you use the platform, the more expensive it becomes.

This is not right. Observability users should not be punished the more they use the platform.

## Design Principles

Do these with metrics and logs. Has to be open source, obviously.

1. Agent-based. The agent can have HTTP server so that `/metrics` still works.

  * It can be a side car on each pod or a dedicated pod or running on K8S node. Either way, it should work. The data format and the networking should facilitate these.

2. Store everything locally. Average Kubernetes pods have a lot of storage that sits idle. This is wasteful, what if we use some for storing metrics?

  * `sqlite-zstd` extension can shrink the disk consumption by a lot. https://hackaday.com/2022/08/01/never-too-rich-or-thin-compress-sqlite-80/

3. Detects everything locally. If each agent can detect a problem, then it can initiate a longer-term data retention policy automatically. And when the problem is solved, it can switch back to much shorter data retention policy. That means: You pay storage cost only when it matters!

4. Scatter-gather metrics result from a central location, only when you need to. This mechanism is needed because data lives on the agent.

  * Can I use benthos's higher level processors to shuffle/filter/group by intermediate results?

5. Push as much query as possible to the agent. Think database's predicate pushdown.

6. Only very few features come from central location, e.g. ping service & global alert evaluator.

  * For the ultimate ease of installation, central server should not have a central DB like PostgreSQL.

  * The only dependency needed should be Nats.

7. Use a real SQL to query data. Perhaps SQLite?

8. No nested table. Just a simple and flat SQL table as data format. Which means, data is convertable to CSV.

9. Simple and documented wire format. Maybe ndjson? And graphite+tags to receive metrics.

10. Use websockets as pipes for data and commands to flow.

  * `Central Server -- [commands] --> agents`: This is for activating/deactivating features.

  * `Agents -- [data] --> Central Server`: This is for streaming the timeseries results.

11. `echo "a.b.c 100 <timestamp> key=value" | nc localhost <port>` must works.

  * Question: What if it's better if the format is an event: `{"time": timestamp, "column_a": 12, "column_b": 9001}`?

    * If this is chosen, stick with numeric types for now. String type requires fancy indexing like bitmap or full text inverted index.

12. Auto aggregation, this is 1 of Druid's shining feature. Data is automatically aggregated per specification, e.g. hourly or daily or monthly.

13. `/metrics` must have a legit content-type. Maybe YAML? Parsing this should not require a custom library.

14. Doesn't need it's own UI. Everything must be fetchable using SQL. Which means Grafana should just work.

15. When the machine is gone, do we lose the data there? 

  * Yes! How many times do you look at 1 year old infra metrics data? Data should be stored long term only if it's eventful (e.g. an outage).

  * A snapshot summary can be generated and broadcasted to all nodes for long term persistence.

  * But do we really need it though? How many times do people look back at past root cause analysis?

16. Eventually, the global alert evaluator will integrates with PagerDuty.

17. Eventually, it will integrates with all machine learning libraries in Python because data is CSV/SQL format friendly.

18. Don't try to solve distributed tracing for now because I couldn't come up with an architecture that requires a central storage.

## Prior Art

* People still love tailing logs directly. [Stern](https://github.com/wercker/stern) is widely popular even though the company have Splunk. For immediate debugging, being close to the pods helps a lot. A central system like Splunk is weighed down by their own data.

* Popular metrics SaaS like NR or DD are frequently impacted by the sheer weight of their own data. Not to mention they are extremely expensive when you have a lot of data. Do you really need to pay that much for 0.5% data you looked at?