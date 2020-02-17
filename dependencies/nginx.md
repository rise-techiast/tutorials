# Nginx Setup Guideline
This is a step-by-step guide to install Nginx.

## Install Nginx
```sh
sudo apt update
sudo apt install nginx
```
## Adjusting the Firewall
List the application configurations that `ufw` knows how to work with by typing:
```sh
sudo ufw app list
```
## Basic Usage
Default html directory:
```
/var/www/html
```
Check web server status:
```sh
systemctl status nginx
```
Reload on configuration changes:
```sh
sudo systemctl reload nginx
```
Stop web server:
```sh
sudo systemctl stop nginx
```
Start web server:
```sh
sudo systemctl start nginx
```
Restart web server:
```sh
sudo systemctl restart nginx
```
## Troubleshooting
#### Enable CORS on Nginx:
```sh
sudo nano /etc/nginx/nginx.conf
```
```
server {
    listen 80;
    #
    # Wide-open CORS config for nginx
    #
    location / {
    if ($request_method = 'OPTIONS') {
            add_header 'Access-Control-Allow-Origin' '*';
            add_header 'Access-Control-Allow-Methods' 'GET, POST, OPTIONS';
            #
            # Custom headers and headers various browsers *should* be OK with but aren't
            #
            add_header 'Access-Control-Allow-Headers' 'DNT,User-Agent,X-Requested-With,If-Modified-Since,Cache-Control,Content-Type,Range';
            #
            # Tell client that this pre-flight info is valid for 20 days
            #
            add_header 'Access-Control-Max-Age' 1728000;
            add_header 'Content-Type' 'text/plain; charset=utf-8';
            add_header 'Content-Length' 0;
            return 204;
        }
        if ($request_method = 'POST') {
            add_header 'Access-Control-Allow-Origin' '*';
            add_header 'Access-Control-Allow-Methods' 'GET, POST, OPTIONS';
            add_header 'Access-Control-Allow-Headers' 'DNT,User-Agent,X-Requested-With,If-Modified-Since,Cache-Control,Content-Type,Range';
            add_header 'Access-Control-Expose-Headers' 'Content-Length,Content-Range';
        }
        if ($request_method = 'GET') {
            add_header 'Access-Control-Allow-Origin' '*';
            add_header 'Access-Control-Allow-Methods' 'GET, POST, OPTIONS';
            add_header 'Access-Control-Allow-Headers' 'DNT,User-Agent,X-Requested-With,If-Modified-Since,Cache-Control,Content-Type,Range';
            add_header 'Access-Control-Expose-Headers' 'Content-Length,Content-Range';
        }
    }
}
```