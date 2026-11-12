# Laravel storage and cache permissions in Docker on a VPS

The first deploy of a containerized Laravel app often ends with "The stream or file storage/logs/laravel.log could not be opened". It is almost always ownership.

## Why it happens

PHP-FPM in the official image runs as `www-data` (UID 33). Files copied into the image during the build are owned by root. If `storage/` or `bootstrap/cache/` is root-owned, FPM cannot write.

## Fix in the Dockerfile

```
COPY --chown=www-data:www-data . /var/www/html
RUN chmod -R ug+rwX storage bootstrap/cache
```

## Fix for volumes

If `storage/` is a named volume, the volume keeps whatever ownership it had when created. Fix it once:

```
docker compose exec -u root app chown -R www-data:www-data storage bootstrap/cache
```

## Do not chmod 777

It works, and it lets any process in the container write executable files to your app directory.

## Related production settings

- Run `php artisan config:cache`, `route:cache` and `view:cache` during the build or at start, as `www-data`.
- Send logs to stderr with `LOG_CHANNEL=stderr` so `docker logs` shows them and the log file problem disappears entirely.

The complete guide to [deploy Laravel on a VPS](https://peon.sh/blogs/deploy-laravel-vps) covers queues and the scheduler too. Written by a contributor to [Peon](https://peon.sh/), an open-source self-hosted PaaS.
