---
title: "Nginx"
date: 2019-06-18
description: "Engine-X"
tags: ["Web", "Linux", "Proxy"]
type: post
showTableOfContents: true
---




----------- redirect site to yandex.ru
/etc/nginx/sites-enebled

---
server 
  listen 80;

  server_nane nginx.srwx.net;

  return 301 "http://ya.ru";

----

test config

```bash
nginx -t
```


curl -D - -o /dev/null -s http://nginx.srwx.net


cat etc/nginx/sites-enabled


nginx reload


# Location 
```bash
location /test {
  return 201 "TEST";
}
```
password and user 
```bash
location /restricted {
  auth_basic "Restrictred";
  auth_basic_user_file .htpasswd; 
}
```
proxy to yandex on main domain
```bash
location /admin {
  proxy_pass http://ya.ru;
}
```


balance beetwen servers 

```bash
upstream app {
  server 127.0.0.1:8080;
  server 10.10.21.12:8080;
}
server 
  listen 80 default_server;

  server_nane nginx.srwx.net;

  location /admin {
    proxy_pass http://app;
  }
}
```

Makes the site faster and gives the static css
```bash
upstream app {
  server 127.0.0.1:8080;
  server 10.10.21.12:8080;
}
server 
  listen 80 default_server;

  server_nane nginx.srwx.net;
  location /css {
    root /var/www/css;
  }
  location /admin {
    proxy_pass http://app;
  }
}
```

```bash
upsteam app {
  server app1:80
  server app2:80
}

server {
  listen 80;

  server_name _;

  location / {
    proxy_pass http://app;
  }
}
```



# Check Module

![nginx](images/nginx1.svg)

Install the prerequisites:
```bash
sudo apt install curl gnupg2 ca-certificates lsb-release ubuntu-keyring
```

Import an official nginx signing key so apt could verify the packages authenticity. Fetch the key:
```bash
curl https://nginx.org/keys/nginx_signing.key | gpg --dearmor \
    | sudo tee /usr/share/keyrings/nginx-archive-keyring.gpg >/dev/null
```
Verify that the downloaded file contains the proper key:
```bash
gpg --dry-run --quiet --no-keyring --import --import-options import-show /usr/share/keyrings/nginx-archive-keyring.gpg
```
The output should contain the full fingerprint `573BFD6B3D8FBC641079A6ABABF5BD827BD9BF62` as follows:
```bash
pub   rsa2048 2011-08-19 [SC] [expires: 2024-06-14]
      573BFD6B3D8FBC641079A6ABABF5BD827BD9BF62
uid                      nginx signing key <signing-key@nginx.com>
```
If the fingerprint is different, remove the file.

To set up the apt repository for stable nginx packages, run the following command:

```bash
echo "deb [signed-by=/usr/share/keyrings/nginx-archive-keyring.gpg] \
http://nginx.org/packages/ubuntu `lsb_release -cs` nginx" \
    | sudo tee /etc/apt/sources.list.d/nginx.list
```
If you would like to use mainline nginx packages, run the following command instead:
```bash
echo "deb [signed-by=/usr/share/keyrings/nginx-archive-keyring.gpg] \
http://nginx.org/packages/mainline/ubuntu `lsb_release -cs` nginx" \
    | sudo tee /etc/apt/sources.list.d/nginx.list
```
Set up repository pinning to prefer our packages over distribution-provided ones:
```bash
echo -e "Package: *\nPin: origin nginx.org\nPin: release o=nginx\nPin-Priority: 900\n" \
    | sudo tee /etc/apt/preferences.d/99nginx
```
To install nginx, run the following commands:
```bash
sudo apt update
sudo apt install nginx
```
test
```bash
nginx -v 
```
![nginx](images/nginx1.svg)

```bash
apt-get build-dep nginx
```
Create repository source file for nginx
```bash
nano /etc/apt/sources.list.d/nginx.list
```
```bash
deb-src http://nginx.org/packages/mainline/ ubuntu bionic nginx
```
```bash
apt update
```
```bash
apt-get build-dep nginx
```
```bash
apt-get source nginx
```
```bash
apt install git
```
```bash
git clone https://github.com/yaoweibin/nginx_upstream_check_module
```
```bash
cd nginx_upstream_check_module
```
take last version
```
patch -p < /root/nginx_http_upsteam_check_module/check
```
patch -p < /root/nginx_http_upsteam_check_module/check_1.14.0+.patch

add to 
```bash
nano debian/rules
```
add to end `add-module-/root/nginx_http_upsteam_check_module`
```bash
CFLAGS="" .. .. .. .. .. ...  
.. .. .. .. .. .. --add-module-/root/nginx_http_upsteam_check_module
```

```bash
dpkg-buildpackage -uc -us -b
```

now i can add to repository
```bash
dpkg -i nginx_1.15.11.1-bionic.amd64.deb
```
```bash
cd /etc/nginx/conf.d/
```
```bash
nano default.conf 
```
After config command `nginx reload`


```bash
upstream app {
  server app1.nginx.devops.srwx.net:80
  server app2.nginx.devops.srwx.net:80

  check interval=3000 rise=2 fall=5 timeout=1000 type=http;
  check_http_send "GET / HTTP/1.0\r\n\r\n";
  check_http_expect_alive http_2xx http_3xx;

}

server {
  listen 80;

  server_name _;

  location / {
    proxy_pass http://app;
  } 
}
```

Test for Loadbalance
```bash
curl -D - -s http://lb.nginx.devops.srwx.net
```

on slave server `app1` and `app2`
```bash
server {
  listen 80;

  service_name _;
  
  location / {
    return 200 "app1\n";
  }
}
```




# Nginx Mirroring Module

![nginx2](images/nginx2.svg)

```
upstream appprod {
  server app1.nginx.devops.srwx.net:80
}
upstream appdev {
  server app2.nginx.devops.srwx.net:80
}


server {
  listen 80;

  server_name _;

  location / {
    proxy_pass http://appprod;
    mirror /mirror;
  }
  location /mirror {
    proxy_pass http://appdev$request_url;
  } 
}
```
> dont forget reload nginx

tail -100f /var/log/nginx/access.log 




# Lua Nginx Module

![nginx3](images/nginx3.svg)

```bash
install nginx nginx-extras    
```
```bash
apt install libnginx-mod-http-lua
```
```bash
apt install lua-nginx-redis
```
Restart Nginx
```bash
systemctl restart nginx
```
```
upstream appprod {
  server app1.nginx.devops.srwx.net:80
}
upstream appdev {
  server app2.nginx.devops.srwx.net:80
}


server {
  listen 80;

  server_name _;

  location / {
    default_type 'text/html';
     
    content_by_lua '
      ngx.say("HELLO")
    ';
  }
  location /mirror {
    proxy_pass http://appdev$request_url;
  } 
}
```

```
upstream appprod {
  server app1.nginx.devops.srwx.net:80
}
upstream appdev {
  server app2.nginx.devops.srwx.net:80
}


server {
  listen 80;

  server_name _;

  location / {
    default_type 'text/html';
     
    content_by_lua '
      api_key = ngx.req.get_headers()["X-api-Key"]
      if not api_key then
        ngx.status = 403
        ngx.say("Dont see x-api-key header")
        return ngx.exit(403)
      end
      
      local redis = require "nginx.redis"
      local red = redis:new()
      local ok, err = red:connect(127.0.0.1", 6379)

      local res, err = red:get(api_key)

      if res == ngx.null then
        ngx.status = 403
        ngx.say("Not authorized")
        return ngx.exit(403)
      else
        ngx.exec("/authorized")
      end
    ';
 }
 location /authorized {
   return 200 "Good to go";
 }
}
```
> reload nginx 'nginx reload

**install redis-server**

```bash
install redis-server 
```

redis-cli


```bash
curl -H 'X-api-key: 1234' -D - -s http://lb.nginx.devops.srw.net
````
```bash
md5 lua.conf
```
```bash
redis-cli
set 34534gr324234fwefa client1
OK
get 34534gr324234fwefa
"client1"
```

```bash
curl -H 'X-api-key: 34534gr324234fwefa' -D - -s http://lb.nginx.devops.srw.net
````






## Backend Balancer


```bash
upstream backend {
  server 172.16.1.100;
  server 172.16.1.101;  
}

server {
  listen 80;

  server_name http.srwx.net;

  location / {
    proxy_set_header Host "http.srwx.net";
    proxy_pass http://backend;
  }
}
