# Nginx Documentation


Nginx Configuration Test

```
nginx -t
```



### Nginx http to https Force Redirection

```
server {
        listen       80;
        server_name  some.com;
        return  301 https://$host$request_uri;
}
```



#### Nginx https proxy setup

```
server {
         listen   443 ssl http2;
         listen [::]:443 ssl;

        server_name   some.com;
        ssl_certificate  /etc/letsencrypt/live/some.com/fullchain.pem;
        ssl_certificate_key /etc/letsencrypt/live/some.com/privkey.pem;
    
        location / {
                proxy_pass http://some.com;
        }    
        location /api/v5 {
                proxy_pass http://some.com;
        }
        location /sitemap{
        default_type "text/xml";
        alias /data/gs/bloodsitemap.xml;
        }
        location /sitemap.xml{
        default_type "text/xml";
        alias /data/gs/bloodsitemap.xml;
        }
}
```

