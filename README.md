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

2. Store everything locally. Average Kubernetes pods have a lot of storage that sits idle. This is wasteful, what if we use some for storing metrics?

3. Detects everything locally. If each agent can detect a problem, then it can initiate a longer-term data retention policy automatically. And when the problem is solved, it can switch back to much shorter data retention policy. That means: You pay storage cost only when it matters!

4. Scatter-gather metrics result from a central location, only when you need to. This mechanism is needed because data lives on the agent.

5. Push as much query as possible to the agent. Think database's predicate pushdown.

6. Only very few features come from central location, e.g. ping service & global alert evaluator.

7. Use a real SQL to query data. Perhaps SQLite?

8. No nested table. Just a simple and flat SQL table as data format. Which means, data is convertable to CSV.

9. Simple and documented wire format. Maybe ndjson? And graphite+tags to receive metrics.

10. Use websockets as pipes for data and commands to flow.

11. `echo "a.b.c 100 <timestamp> key=value" | nc localhost <port>` must works.

12. `/metrics` must have a legit content-type. Maybe YAML? Parsing this should not require a custom library.

13. The only dependency needed should be Nats.

14. Doesn't need it's own UI. Everything must be fetchable using SQL. Which means Grafana should just work.

15. When the machine is gone, do we lose the data there? Yes! How many times do you look at 1 year old infra metrics data? Data should be stored long term only if it's eventful (e.g. an outage).

16. Eventually, the global alert evaluator will integrates with PagerDuty.

17. Eventually, it will integrates with all machine learning libraries in Python because data is CSV/SQL format friendly.

## Prior Art

* People still love tailing logs directly. [Stern](https://github.com/wercker/stern) is widely popular even though the company have Splunk. For immediate debugging, being close to the pods helps a lot. A central system like Splunk is weighed down by their own data.
