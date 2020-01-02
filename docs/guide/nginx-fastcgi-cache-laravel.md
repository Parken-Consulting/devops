# NGINX FastCGI Cache for Laravel 6.2

# NGINX Proxy Cache

#### Create a folder

```sh
mkdir /var/cache/nginx
```

`/etc/nginx/nginx-cache-like-static-page-head.conf`
```sh
fastcgi_cache_path /var/run/nginx-cache levels=1:2 keys_zone=STATICCACHE:100m inactive=60m;
fastcgi_cache_key "$scheme$request_method$host$request_uri";
fastcgi_cache_use_stale error timeout invalid_header http_500;
fastcgi_ignore_headers Cache-Control Expires Set-Cookie;

```

`/etc/nginx/nginx-cache-like-static-page.conf`

```sh
  fastcgi_cache STATICCACHE;
  fastcgi_cache_valid  60m;

```

`/etc/nginx/conf.d/www.example.com.conf`

```sh
include nginx-cache-like-static-page-head.conf;

server {
      ....
      
      location / {
        include nginx-cache-like-static-page.conf;

        ....

      }
 }
```

## References:
- https://www.linuxbabe.com/nginx/setup-nginx-fastcgi-cache
