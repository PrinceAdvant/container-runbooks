# Five Kubernetes features small teams think they need, and the simpler equivalent

Kubernetes solves real problems, mostly at a scale small teams do not have yet. Here are five features that often drive the decision, with simpler options.

1. **Self-healing.** Docker restart policies (`restart: unless-stopped`) plus health checks restart crashed containers. An uptime monitor tells you when that is not enough.
2. **Rolling updates.** A reverse proxy that switches traffic to a new container only after its health check passes gives zero-downtime deploys on one host.
3. **Service discovery.** Compose networks resolve service names out of the box. `db` and `redis` just work.
4. **Secrets.** Environment variables stored in a platform or a root-only env file, injected at runtime, cover most needs. Add a secrets manager when compliance asks for it.
5. **Scaling.** `docker compose up --scale worker=3` scales stateless workers. For web traffic, a bigger server is usually cheaper than a cluster until you outgrow the largest reasonable VPS.

## When Kubernetes earns its cost

Many services owned by different teams, strict multi-region requirements, or workloads that genuinely autoscale by the hour. If none apply, the operational overhead buys little.

A longer comparison of [Docker Compose vs Kubernetes](https://peon.sh/blogs/docker-compose-vs-kubernetes) goes deeper. Written by a contributor to [Peon](https://peon.sh/), an open-source self-hosted PaaS.
