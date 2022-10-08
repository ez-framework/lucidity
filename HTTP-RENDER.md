This docs explores the format of HTTP rendered metrics.

## Prometheus

Prometheus `/metrics` is the most famous example.

But it is annoyingly a custom format. It is neither JSON nor TOML nor YAML.

Scraping it requires custom libraries. Why?


## CSV

If we go with events input format, the endpoint can be as simple as:

```
/<table-name>.csv
<timestamp>,12,4,6

# and it can be accompanied with schema definition
/<table-name>/schema.json
{
  "columns": [
    {"name": "time", "type": "unix-timestamp"},
    {"name': "user.online.incr", "type": "int64"},
    ...
  ]
}
```

## Graphite

```
/metrics.json
{
  "aaa.bbb.ccc": {"value": 1, "time": <most-recent-timestamp>, "tags": {}},
  "xxx.yyy.zzz": {"value": 1, "time": <most-recent-timestamp>, "tags": {}}
}
```
