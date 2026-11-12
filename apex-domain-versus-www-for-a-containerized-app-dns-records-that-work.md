# Apex domain versus www for a containerized app: DNS records that work

Pointing a custom domain at an app on your own server raises the same question every time: apex, www, or both. Both, with one redirecting to the other.

## DNS records

- `example.com` A record to the server IPv4, and an AAAA record if the server has IPv6 and your proxy listens on it.
- `www.example.com` CNAME to `example.com`.

CNAME on the apex is not allowed by the DNS standard. Some providers offer CNAME flattening or ALIAS records, which also work.

## Certificates

Request a certificate that covers both names. With Let's Encrypt HTTP validation, both names must resolve to the server before issuance, so add DNS first and wait for it to propagate.

## Pick a canonical

Choose one, typically the apex, and issue a permanent 301 redirect from the other. Set the same canonical in your app's HTML. Search engines then consolidate signals on one URL.

## Common mistakes

- An AAAA record pointing at an old server, so IPv6 users get a different site or a certificate error.
- Cloudflare proxy enabled before the origin has a certificate, causing redirect loops with Flexible SSL.

The guide to setting up a [custom domain for a Docker app](https://peon.sh/blogs/custom-domain-docker-app) walks through the proxy side. I contribute to [Peon](https://peon.sh/), an open-source self-hosted deployment platform.
