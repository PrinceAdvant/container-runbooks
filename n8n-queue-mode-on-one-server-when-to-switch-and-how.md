# n8n queue mode on one server: when to switch and how

A default n8n install runs everything in one process: the editor, webhooks and workflow executions. That is fine for a while.

## Signs you need queue mode

- The editor becomes slow while heavy workflows run.
- Webhook responses time out during bursts.
- One long workflow blocks others scheduled at the same time.

## What queue mode adds

A Redis instance holds pending executions. The main n8n process accepts webhooks and serves the editor, then hands work to one or more worker processes.

## Minimal setup

- Set `EXECUTIONS_MODE=queue` and the `QUEUE_BULL_REDIS_HOST` variables on the main service and on workers.
- Run workers with the same image and the command `n8n worker`.
- All processes must share the same `N8N_ENCRYPTION_KEY` and Postgres database. SQLite does not work in queue mode.
- Start with two workers and a concurrency of 5 to 10 each, then adjust while watching memory.

## Keep it tidy

Enable execution data pruning, or the executions table grows without limit. Back up Postgres and the encryption key together. Restoring one without the other loses stored credentials.

The guide to [self-host n8n for automation](https://peon.sh/blogs/self-host-n8n-automation) covers the basic install. I work on [Peon](https://peon.sh/), an open-source self-hosted deployment platform.
