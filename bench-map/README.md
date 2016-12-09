# Fluentd benchmark - forward

This benchmarks following architecture scenario:

```
  Benchmark Node
  +-----------------------------------+
  | +-----------+      +-----------+  |
  | |           |      |           |  |
  | | Log File  +----->|  Fluentd  |  |
  | |           |      |           |  |
  | +-----------+  in_tail --map-- flowcounter_simple
  +-----------------------------------+
```

## Setup Fluentd

Assume ruby is installed

```
git clone https://github.com/cosmo0920/fluent-plugins-bench
cd fluent-plugins-bench/bench_map
bundle
bundle exec fluentd -c map.conf -p /path/to/plugin-map
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

```
<snip>
2016-12-08 17:53:22 +0900 [info]: plugin:out_flowcounter_simple	count:38549	indicator:num	unit:second
2016-12-08 17:53:22 +0900 [info]: plugin:out_flowcounter_simple	count:37534	indicator:num	unit:second
2016-12-08 17:53:23 +0900 [info]: plugin:out_flowcounter_simple	count:39562	indicator:num	unit:second
2016-12-08 17:53:23 +0900 [info]: plugin:out_flowcounter_simple	count:37534	indicator:num	unit:second
2016-12-08 17:53:24 +0900 [info]: plugin:out_flowcounter_simple	count:36520	indicator:num	unit:second
```
