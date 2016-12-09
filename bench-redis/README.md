# Fluentd benchmark - forward

This benchmarks following architecture scenario:

```
  Benchmark Node
  +-----------------------------------+
  | +-----------+      +-----------+  |
  | |           |      |           |  |
  | | Log File  +----->|  Fluentd  |  |
  | |           |      |           |  |
  | +-----------+  in_tail --redis--  |
  |                         |         |
  |                         +-- flowcounter_simple (just to estimate events per second)
  +-------------------------+---------+
```

## Setup Fluentd

Assume ruby is installed

```
git clone https://github.com/cosmo0920/fluent-plugins-bench
cd fluent-plugins-bench/bench_map
bundle
bundle exec fluentd -c redis.conf -p /path/to/plugin-redis
```

## Run benchmark tool and measure

Run at Fluentd agent server.

This tool outputs logs to `dummy.log`, and Fluentd agent reads it and process data with fluent-plugin-map.

```
cd fluent-plugins-bench/bench_map
bundle exec dummer -c dummer.conf
```

You may increase the rate (messages/sec) of log generation by -r option to benchmark.

```
bundle exec dummer -c dummer.conf -r 40000
```

You should see an output on Fluentd receiver as followings. This tells you the performance of fluentd processing.
