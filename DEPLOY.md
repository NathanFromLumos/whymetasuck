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
    ServerName whymetasuck.nathanknowsnothing.co.uk
    DocumentRoot /var/www/whymetasuck
    <Directory /var/www/whymetasuck>
        Require all granted
    </Directory>
    RedirectMatch 404 /\.git
</VirtualHost>
```

## 3. DNS (Cloudflare)

Add a record on nathanknowsnothing.co.uk:

| Type | Name | Content | Proxy |
|------|------|---------|-------|
| A | `whymetasuck` | your server's IP | Proxied (orange) |

## 4. Updates

```sh
cd /var/www/whymetasuck && git pull
```

New builds of the visualiser change the bundle filename (`index-*.js`), and
`index.html` references it explicitly, so a pull is always cache-safe.
