# Nginx Configuration with Docker

**Nginx** is an open-source reverse proxy server for HTTP, HTTPS, SMTP, POP3, and IMAP protocols. It can also function as a load balancer, HTTP cache, and web server (origin server).

---

## Static Website Setup

### File: `docker-compose.yml`
```yaml
nginx:
  image: nginx:alpine
  ports:
    - "80:80"
  volumes:
    - ./data/conf.d:/etc/nginx/conf.d
    - ./data/html:/usr/share/nginx/html
  restart: unless-stopped
```

---

## Reverse Proxy Setup

### File: `docker-compose.yml`
```yaml
nginx:
  image: nginx:alpine
  volumes:
    - ./data/conf.d:/etc/nginx/conf.d
    - ./data/ssl:/etc/nginx/ssl
    - ./data/htpasswd:/etc/nginx/htpasswd
  net: host
  restart: unless-stopped
```

---

## Generating Password File

To generate a password file, use the following command:

```bash
echo "username:$(openssl passwd -apr1 password)" >> data/htpasswd
```

---

## Nginx Server Configuration Examples

### Default Redirection
```nginx
server {
    listen 80 default;
    server_name _;
    return 301 http://blog.foobar.site/;
}
```

### Reverse Proxy for Blog
```nginx
server {
    listen 80;
    server_name blog.foobar.site blog.easypi.info;

    location / {
        proxy_pass http://127.0.0.1:6109;
    }
}
```

### Reverse Proxy with Authentication
```nginx
server {
    listen 80;
    server_name wiki.foobar.site wiki.easypi.info;

    location / {
        auth_basic "restricted";
        auth_basic_user_file /etc/nginx/htpasswd;
        proxy_pass http://127.0.0.1:8000;
    }
}
```

### IoT Proxy Configuration
```nginx
server {
    listen 80;
    server_name iot.foobar.site iot.easypi.info;

    location / {
        proxy_pass http://127.0.0.1:1880;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection "upgrade";
    }
}
```

---

## RTMP Configuration

### File: `rtmp`
```nginx
rtmp {
    server {
        listen 1935;

        application live {
            live on;
        }
    }
}
```
---
