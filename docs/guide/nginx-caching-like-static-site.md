
#### Create a folder

```sh
mkdir /var/cache/nginx
```

`/etc/nginx/nginx-cache-like-static-page.conf`

```sh
      # enable caching
      proxy_cache assets_zone;

      # adds a "HIT/MISS" cache header
      add_header X-Proxy-Cache $upstream_cache_status;

      # ignore these headers, lets effing cache everything
      proxy_ignore_headers Cache-Control Set-Cookie Expires;

      # http status codes to cache for 14 days
     proxy_cache_valid 200 301 14d;

      # we can invalidate a page by sending "my-secret-header:true"
      proxy_cache_bypass $http_my_secret_header;

      include proxy_params;

```

`/etc/nginx/conf.d/www.example.com.conf`

```sh
proxy_headers_hash_max_size 512;
proxy_headers_hash_bucket_size 128;
proxy_cache_path /var/cache/nginx levels=1:2 keys_zone=assets_zone:10m inactive=60m;
proxy_cache_key "$scheme$request_method$host$request_uri";

location / {
  include teak_proxy_cache.conf;
  
  ....
  
}
```
