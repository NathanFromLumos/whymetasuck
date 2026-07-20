# Deploying to the server

The site is fully static — deployment is a git clone plus a vhost.

## 1. Clone on the server

```sh
git clone https://github.com/NathanFromLumos/whymetasuck.git /var/www/whymetasuck
```

## 2. Web server vhost

### nginx

```sh
cp /var/www/whymetasuck/deploy/nginx.conf /etc/nginx/sites-available/whymetasuck
ln -s /etc/nginx/sites-available/whymetasuck /etc/nginx/sites-enabled/
nginx -t && systemctl reload nginx
```

If your Cloudflare SSL mode is **Full**, add the `listen 443 ssl` and certificate
lines from an existing vhost (see the comment at the top of `deploy/nginx.conf`).

### Apache (alternative)

```apache
<VirtualHost *:80>
    ServerName whymetasucks.halfpipelabs.co.uk
    DocumentRoot /var/www/whymetasuck
    <Directory /var/www/whymetasuck>
        Require all granted
    </Directory>
    RedirectMatch 404 /\.git
</VirtualHost>
```

## 3. DNS (Cloudflare)

`whymetasucks.halfpipelabs.co.uk` already resolves via the existing wildcard/
catch-all on halfpipelabs.co.uk (it currently hits the server's default vhost,
which 301s to the apex). If you'd rather have an explicit record, add on
halfpipelabs.co.uk:

| Type | Name | Content | Proxy |
|------|------|---------|-------|
| A | `whymetasucks` | the server's IP | Proxied (orange) |

Either way, once the nginx vhost from step 2 is in place it takes precedence
over the catch-all redirect and the site goes live.

## 4. Updates

```sh
cd /var/www/whymetasuck && git pull
```

New builds of the visualiser change the bundle filename (`index-*.js`), and
`index.html` references it explicitly, so a pull is always cache-safe.
