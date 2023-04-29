# Nginx Proxy

Run this on fly.io, it should do the proxying.

You can add to the `default.conf` to redirect at certain locations.

## Using on Fly

Update to latest `flyctl`, just because. It changes a lot, in the last month or so, there are important updates.


From the current directory (where the `Dockerfile` lives):

```bash
fly launch
# Choose a name
# Choose an organization (if you have multiple it asks you)
# Choose a region. You can go multi-region later if you'd like
# No pgsql, no redis (I assume)
# Deploy now
```

## Things you need to deal with

1. You'll want to assign this app an IP address, and point DNS to the domain to that IP.
2. You'll want to create an SSL in Fly.io
3. This means that the domain will not longer point to GitHub, so the proxy setting in `default.conf` will need
   to change to point to the correct location (perhaps the GitHub URL they generate? - something like `mikes-page.github.io`?)

Assuming you have a `fly` app created already (after the `fly launch` command):

```bash
# You get IP's assigned to you automatically, however
# the ipv4 is "shared". This is fine normally.

# View your IP addresses:
fly ips list

# If you want have a dedicated IP:
flyctl ips allocate-v4
flyctl ips allocate-v6 # if you want...
```

Take the IPv4 (and optionally the IPv6) and edit the `A` / `AAAA` record for the domain so they point to that IP.

> You'll probably break SSL traffic for the site between the time that DNS is doing it's thing and before Fly can generate an SSL.

```bash
# Once letsencrypt can see the DNS change,
# we can generate certs for that domain on Fly
flyctl certs create "expeditedsecurity.com"
```

## Note

The `fly.toml.example` file is here for reference but you won't need it. The `fly launch` command will generate you a new one
with the correct application name and region filled out.

