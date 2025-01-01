nginx


Nginx یک سرور پروکسی معکوس منبع باز برای پروتکل های HTTP، HTTPS، SMTP، POP3 و IMAP، و همچنین یک متعادل کننده بار، کش HTTP و یک وب سرور (سرور اصلی) است.

وب سایت استاتیک
فایل: docker-compose.yml

nginx:
  image: nginx:alpine
  ports:
    - "80:80"
  volumes:
    - ./data/conf.d:/etc/nginx/conf.d
    - ./data/html:/usr/share/nginx/html
  restart: unless-stopped
پروکسی معکوس
فایل: docker-compose.yml

nginx:
  image: nginx:alpine
  volumes:
    - ./data/conf.d:/etc/nginx/conf.d
    - ./data/ssl:/etc/nginx/ssl
    - ./data/htpasswd:/etc/nginx/htpasswd
  net: host
  restart: unless-stopped
فایل رمز عبور را می توان توسط:

echo "username:$(openssl passwd -apr1 password)" >> data/htpasswd

فایل: پیش فرض

server {
    listen 80 default;
    server_name _;
    return 301 http://blog.foobar.site/;
}

server {
    listen 80;
    server_name blog.foobar.site blog.easypi.info;
    location / {
        proxy_pass http://127.0.0.1:6109;
    }
}

server {
    listen 80;
    server_name wiki.foobar.site wiki.easypi.info;
    location / {
        auth_basic restricted;
        auth_basic_user_file /etc/nginx/htpasswd;
        proxy_pass http://127.0.0.1:8000;
    }
}

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
فایل: rtmp

rtmp {
    server {
        listen 1935;
        application live {
            live on;
        }
    }
}
