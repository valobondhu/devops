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



##### Allow Download file size

```
vim uwsgi_params
uwsgi_max_temp_file_size 20480m;
```

##### geoip setup with nginx

Download GeoLite2-Country.mmdb from mmdb db site

```
vim nginx.conf

geoip2 /etc/nginx/GeoLite2-Country_20201103/GeoLite2-Country.mmdb {
  $geoip2_data_country_code country iso_code;
}

map $geoip2_data_country_code $redirect_country {
    default no;
   BD yes;
    AE yes;
    US yes;
    IN yes;

}


## Set variable by matching url then rewrite 

location / {

   if ($args ~ "filename=/usa/(?<id>.*)$") {
set $test U; # if filename location from use then set test variable value U
      }
      
    if ($args ~ "filename=/india/(?<id>.*)$") {
set $test I; If filename location in india set test varibale value I
      }
      
      if ($redirect_country = yes) { # if redirect country = yes then  add variable value with G with test variablble. 
set $test "${test}G";  # যখন redirect country code = yes হবে তখন আগের test variable এর যে value ছিলো সেটারসাথে আরো একটা value add করো। তার পর চেক করো test variable  এর value চেক করে যে দেশে পাঠাতে চাও সেই দেশে পাঠাও। 
     }      
      
 if ($test = UG) {
 rewrite ^ http://usa.some.com/$id? break ;
}

if ($test = IG) {
 rewrite ^ http://india.some.com/$id? break ;
}

বিষয়টা ব্যাখ্যা করি। প্রথমে filename=/usa যখন ছিলো তখন test variable এ value সেট করেছি যে file টি usa  এর । এরপর redirect_country yes কিনা চেক করলাম, redirect_country yes মানে ঐদেশ থেকে request আসলে সেটা redirect হবে , redirect country yes হলে test variable এর আগের value এর সাথে আরো একটা value add করলাম G । G   দিয়ে আমি বুঝালাম global অথাৎ redirect হবে। এখন test variable এর value UG। এর মানে ফাইলটা USA এর এবং সেটা usa থেকে load করাতে চাই। 
```

# test
