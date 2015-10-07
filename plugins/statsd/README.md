# Telegraf Service Plugin: statsd

#### Plugin arguments:

- **service_address** string: Address to listen for statsd UDP packets on
- **delete_gauges** boolean: Delete gauges on every collection interval
- **delete_counters** boolean: Delete counters on every collection interval
- **delete_sets** boolean: Delete set counters on every collection interval
- **allowed_pending_messages** integer: Number of messages allowed to queue up
on the UDP listener before the next flush. NOTE: gauge, counter, and set
measurements are aggregated as they arrive, so this is not a straight counter of
the number of total messages that the listener can handle between flushes.
- **templates** []string: Templates for transforming statsd buckets into influx
measurements and tags.

#### Statsd bucket -> InfluxDB Templates

TODO [summarize this](https://github.com/influxdb/influxdb/tree/master/services/graphite#templates)

#### Description

The statsd plugin is a special type of plugin which runs a backgrounded statsd
listener service while telegraf is running.

The format of the statsd messages was based on the format described in the
original [etsy statsd](https://github.com/etsy/statsd/blob/master/docs/metric_types.md)
implementation. In short, the telegraf statsd listener will accept:

- Gauges
    - `users.current.den001.myapp:32|g` <- standard
    - `users.current.den001.myapp:+10|g` <- additive
    - `users.current.den001.myapp:-10|g`
- Counters
    - `deploys.test.myservice:1|c` <- increments by 1
    - `deploys.test.myservice:101|c` <- increments by 101
    - `deploys.test.myservice:1|c|@0.1` <- sample rate, increments by 10
- Sets
    - `users.unique:101|s`
    - `users.unique:101|s`
    - `users.unique:102|s` <- would result in a count of 2 for `users.unique`
- Timings
    - TODO
