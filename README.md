# Container runbooks

Short runbooks for running web apps, databases and automation tools in containers on your own servers. Written by a contributor to [Peon](https://peon.sh/), an open-source self-hosted deployment platform.

- [Laravel storage and cache permissions in Docker on a VPS](laravel-storage-and-cache-permissions-in-docker-on-a-vps.md): Fixing the classic storage and bootstrap/cache permission errors when Laravel runs in a container.
- [n8n queue mode on one server: when to switch and how](n8n-queue-mode-on-one-server-when-to-switch-and-how.md): When a self-hosted n8n instance should move from regular mode to queue mode with Redis and workers.
- [Five Kubernetes features small teams think they need, and the simpler equivalent](five-kubernetes-features-small-teams-think-they-need-and-the-simpler-e.md): Common reasons small teams reach for Kubernetes, paired with lighter tools that solve the same problem.
- [Graceful shutdown for Node.js in Docker: SIGTERM, PID 1 and timeouts](graceful-shutdown-for-node-js-in-docker-sigterm-pid-1-and-timeouts.md): Making a Node.js container stop cleanly so deploys do not drop in-flight requests.
- [Preview environments and databases: three strategies and their trade-offs](preview-environments-and-databases-three-strategies-and-their-trade-of.md): How to give each pull request preview a database without leaking production data or blowing up costs.
- [PHP-FPM pool sizing for Laravel containers](php-fpm-pool-sizing-for-laravel-containers.md): A simple formula for pm.max_children and related settings so Laravel containers neither waste memory nor queue requests.
- [Structured JSON logs from containers: a minimal convention](structured-json-logs-from-containers-a-minimal-convention.md): A small set of fields every service should log so container logs stay searchable across a whole stack.
- [Apex domain versus www for a containerized app: DNS records that work](apex-domain-versus-www-for-a-containerized-app-dns-records-that-work.md): How to point both the apex domain and www at a self-hosted app with correct certificates and one canonical URL.

Licensed CC BY 4.0.
