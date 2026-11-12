# Structured JSON logs from containers: a minimal convention

Once a stack has more than three services, free-text logs become hard to search. A small shared convention fixes most of it.

## Write JSON to stdout

One object per line. Docker captures stdout and stderr, so there is no file to rotate inside the container.

## Agree on a few fields

- `ts`: ISO 8601 timestamp in UTC.
- `level`: one of debug, info, warn, error.
- `service`: the service name, same as in Compose.
- `msg`: a short, human-readable message.
- `request_id`: passed between services in a header, so one request can be traced across the stack.
- `err`: the error message and stack, only on errors.

Everything else is optional context.

## Limit volume at the Docker level

Set log rotation in `/etc/docker/daemon.json`:

```
{"log-driver":"json-file","log-opts":{"max-size":"10m","max-file":"5"}}
```

Without it, a chatty container can fill the disk.

## Do not log secrets

Redact authorization headers, cookies and connection strings in the logger, not in each call.

## Read them

`docker logs app | jq 'select(.level=="error")'` goes a long way before you need a log platform.

More in [Docker logs best practices](https://peon.sh/blogs/docker-logs-best-practices). Written by a contributor to [Peon](https://peon.sh/), an open-source self-hosted PaaS.
