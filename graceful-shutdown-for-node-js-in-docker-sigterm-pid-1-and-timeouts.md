# Graceful shutdown for Node.js in Docker: SIGTERM, PID 1 and timeouts

When a deploy replaces a container, Docker sends SIGTERM, waits 10 seconds, then sends SIGKILL. If your Node app ignores SIGTERM, every deploy cuts off requests mid-flight.

## Problem 1: npm swallows the signal

`CMD ["npm", "start"]` makes npm PID 1, and npm does not forward signals reliably. Use `CMD ["node", "server.js"]` instead.

## Problem 2: PID 1 has no default handlers

Node as PID 1 does not exit on SIGTERM unless you handle it. Add a handler, or run with `docker run --init` (or `init: true` in Compose) so tini handles signals.

## Handle it in code

```
process.on('SIGTERM', () => {
  server.close(() => process.exit(0));
  setTimeout(() => process.exit(1), 8000).unref();
});
```

`server.close` stops accepting new connections and waits for open ones. The timer makes sure you exit before Docker's SIGKILL.

## Keep-alive connections

Idle keep-alive sockets can hold `server.close` open. Node 18.2 and later have `server.closeIdleConnections()`. Call it right after `close`.

## Tune the timeout

If requests can legitimately take longer, raise `stop_grace_period` in Compose.

The guide to [deploy a Node.js app with Docker](https://peon.sh/blogs/deploy-nodejs-app-docker) covers the Dockerfile itself. I contribute to [Peon](https://peon.sh/), an open-source self-hosted deployment platform.
