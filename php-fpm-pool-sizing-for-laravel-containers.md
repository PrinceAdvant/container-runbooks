# PHP-FPM pool sizing for Laravel containers

The default PHP-FPM pool settings are a guess. For a Laravel container with a memory limit, it is better to calculate them.

## Measure a worker

Under realistic load, check average memory per FPM process:

```
ps -ylC php-fpm --sort:rss | awk '{sum+=$8; n++} END {print sum/n/1024 " MB"}'
```

Laravel apps commonly land between 40 and 80 MB per worker.

## Calculate max_children

Take the container memory limit, subtract about 100 MB for the master process and OPcache, and divide by per-worker memory. A 1 GB container with 60 MB workers gives roughly 15.

## Pick a process manager

- `pm = static` with that number for steady traffic. Predictable memory.
- `pm = dynamic` with `pm.start_servers` around a third of max and sensible spare limits for uneven traffic.
- `pm.max_requests = 500` recycles workers and contains slow memory leaks.

## Watch the slow log

Set `request_slowlog_timeout = 5s` and a `slowlog` path. Slow requests hold workers and cause queuing long before CPU looks busy.

## OPcache

Enable it with `opcache.validate_timestamps=0` in production images, since code does not change inside a running container.

A comparison of the [best Docker hosting for Laravel](https://peon.sh/blogs/best-docker-hosting-for-laravel) covers where to run it. I work on [Peon](https://peon.sh/), an open-source self-hosted deployment platform.
