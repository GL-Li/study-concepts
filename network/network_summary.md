## nginx
see `network-nginx/nginx.html`

### config model xxx.conf

#### Elements

- simple directive 
    `listen 80;`  name value; ends with `;`
- block directive 
    ```
    server {
        listen 80;
        location / {
            root /var/www/site;
        }
    }
    ```
- context: content inside `{...}` of a block directive

#### stock `nginx.conf`

File location: `/etc/nginx`

- understand simple directive `include /path/to/file`
- understand the purpose of two special sub-directories:
    - `conf.d`: for things that are not a site
    - `sites-enabled`: for sites
        - sites are saved in `sites-available`
        - switch a site on/off with a symlink

```
user www-data;
worker_processes auto;

events {
    worker_connections 768;
}

http {
    include       /etc/nginx/mime.types;   # Content-Type by extension
    default_type  application/octet-stream;
    access_log    /var/log/nginx/access.log;
    error_log     /var/log/nginx/error.log;
    gzip on;

    include /etc/nginx/conf.d/*.conf;       # ← your drop-ins land here
    include /etc/nginx/sites-enabled/*;     # ← and the stock site
}
```

### Use nginx

- `sudo systemctl status nginx`: check status
- `sudo systemctl start nginx`: start nginx
- `sudo nginx -t`: check if config file is good
- `sudo systemctl reload nginx`: reload running nginx after config changes
