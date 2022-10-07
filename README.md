# Lucidity

A new take on an Observability platform.

We seek to return to the simplicity of Graphite while also solving Observability problems that still exists in today's world.

## Problem 1: Metrics and logging are still too expensive.

We spend crazy amount of money storing data that's looked at 0.5% of the time.

And the more you use the platform, the more expensive it becomes.

This is not right. Observability users should not be punished the more they use the platform.

## Design Principles

Do these with metrics and logs.

1. Agent-based, obviously. The agent can have HTTP server so that `/metrics` still works.

2. Store everything locally.

3. Detects everything locally.

4. Scatter-gather metrics result from a central location, only when you need to.

5. Push as much query as possible to the agent. Think database's predicate pushdown.

6. Only very few features come from central location, e.g. ping service & global alert evaluator.

7. Real SQL to query. Perhaps SQLite?

8. No nested table. Just a simple and flat SQL table as data format. Which means, data is convertable to CSV.

9. Simple and documented wire format. Maybe ndjson? And graphite+tags to receive metrics.

10. Use websockets as pipes for data and commands to flow.

11. `echo "a.b.c 100 <timestamp> key=value" | nc localhost <port>` must works.

12. `/metrics` must have a legit content-type. Maybe YAML?

13. The only dependency needed should be Nats.

14. Doesn't need it's own UI. Everything must be fetchable using SQL. Which means Grafana should just work.

15. Eventually, the global alert evaluator will integrates with PagerDuty.

16. Eventually, it will integrates with all machine learning libraries in Python because data is CSV/SQL format friendly.