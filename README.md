Nginx pre-baked config
======================

Checkout this repo at `/etc/nginx/`, then install nginx.

```bash
sudo apt-get install nginx
```

Using PHP
---------

Install PHP5 Fast Process Manager

```bash
sudo apt-get install php5-fpm
sudo mkdir -p /var/run/php5-fpm/
sudo nano /etc/php5/fpm/pool.d/www.conf
```

Replace `listen = 127.0.0.1:9000` with `listen = /var/run/php5-fpm/www.sock`

Adding a website
----------------

Add a server config file to `sites-available` (e.g. `sites-available/com.example.www`). To enable the site, link to the config file in `sites-enabled`.

Use the global config files defined in `/global` to add extra functions on the server. 
But when you do, read the included config file: there may be additional instructions in the comments.
E.g.: when including `/global/php-fcgi.conf` you may need to edit `/etc/php5/fpm/php.ini`.

Example site config:
```conf
server {
  listen 127.0.0.1:80;

  server_name hostname.com;

  root /path/to/server/root/;
  autoindex on;

  access_log  /var/log/nginx/yourhost_access.log;
  error_log   /var/log/nginx/yourhost_error.log;

  include global/restrictions.conf;
  include global/php-fcgi.conf;
}
```

Don't forget to get the config loaded:
```bash
sudo service nginx reload
```
