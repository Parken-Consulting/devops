# NGINX Browser Cache like Content Delivery Network - CDN

```sh
# cache directive for all static apps
  index index.html;

  # cache.appcache, your document html and data
  location ~* \.(?:manifest|appcache|html?|xml|json)$ {
    expires -1;
  }

  # Media: images, icons, video, audio, HTC
  location ~* \.(?:jpg|jpeg|gif|png|ico|cur|gz|eot|otf|woff|woff2|ttf|svg|svgz|mp4|ogg|ogv|webm|htc)$ {
    expires 1M;
    access_log off;
    add_header Cache-Control "public";
  }

  # CSS and Javascript
  location ~* \.(?:css|js)$ {
    expires 1y;
    access_log off;
    add_header Cache-Control "public";
  }

  location / {
    #if ($args !~* _v=110917) {
    #  return 301 https://$server_name$uri?_v=110917&$args;
    #}

    try_files $uri $uri/ /index.html;
    expires -1;
  }
```
