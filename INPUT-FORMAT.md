This docs explores the possibility of input format.

## DogStatsD

https://docs.datadoghq.com/developers/dogstatsd/datagram_shell/?tab=metrics

```
<METRIC_NAME>:<VALUE>|<TYPE>|@<SAMPLE_RATE>|#<TAG_KEY_1>:<TAG_VALUE_1>,<TAG_2>

# examples
page.views:1|c
fuel.level:0.5|g
song.length:240|h|@0.5
users.uniques:1234|s
users.online:1|c|#country:china
```

## JSON Events

```
{"table": "users", "time":<timestamp>, "fuel":0.5, "user.online.incr": 1, "user.online.decr": 0}
```

## Custom Events

```
table=users time=<timestamp> fuel=0.5 user.online.incr=1 user.online.decr=0

# time is auto injected if missing
# In the future, if string columns are added, values can be wrapped with quotes
# Easy to type for sysadmins.
```

I am starting to like events data. There's no mental burden to remember.

Example of a mental burden: 

* in Graphite do you auto reset the count data? or do you keep it growing forever? Why is this even a problem?

Coincidentally, events format works with structured logging like zerolog.
