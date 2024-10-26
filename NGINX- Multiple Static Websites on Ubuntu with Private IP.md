# Configuring Multiple Static Websites with `NGINX` on Ubuntu 20.04 & 22.04: Using `Private IPs` and `Custom Port` Numbers

## Installation and Configuration
    
`1.Installation Steps:`

```
sudo apt update
sudo apt install nginx
```

`2. Service Management:`
```
sudo systemctl start nginx
sudo systemctl status nginx
sudo systemctl enable nginx
```
`3.Check HTTP Headers and Response Body`
```
curl -i ip_address 
#example 
curl -i 192.168.1.58
```

`4.Setting Up Server application root.`

You’ll need to create separate directories for each website.
```
sudo mkdir -p /var/www/site1
sudo mkdir -p /var/www/site2

```
`5.Create a sample index.html page`

**For `site-1`**
```
vim /var/www/site1/index.html
```
```
<html>
    <head>
        <title>Welcome to Site 1!</title>
    </head>
    <body>
        <h1>This is Site 1, served on port 8081!</h1>
    </body>
</html>

```
**For `site-2`**

```
vim /var/www/site2/index.html
```
```
<html>
    <head>
        <title>Welcome to Site 2!</title>
    </head>
    <body>
        <h1>This is Site 2, served on port 8082!</h1>
    </body>
</html>
```


**OR Store full Project file here** 
`/var/www/site1`
and
`/var/www/site2`


`6.Create a server block with the correct directives`

**For `site1` (using port `8081`):**
```
sudo vim /etc/nginx/sites-available/site1
```
`7.Example NGINX Configuration:`
```
server {
    # Bind to the local IP address and the default HTTP port 80
    listen 192.168.1.58:8081;

    # Specify the server name (you can use the IP if there's no domain)
    server_name 192.168.1.58;

    # Root directory where your site's files are located
    root /var/www/site1;

    # Default index file
    index index.html index.htm index.php;

    # Logging configuration
    access_log /var/log/nginx/site1_access.log;
    error_log /var/log/nginx/site1_error.log;

    # Location block to serve requests
    location / {
        try_files $uri $uri/ =404;
    }

    # Optional: Configure PHP if needed
    location ~ \.php$ {
        include snippets/fastcgi-php.conf;
        fastcgi_pass unix:/var/run/php/php7.4-fpm.sock; # Adjust the PHP version if necessary
    }

    # Optional: Handle static files
    location ~* \.(jpg|jpeg|png|gif|ico|css|js)$ {
        expires 365d;
    }
}
```
**For `site2` (using port `8082`):**
```
sudo vim /etc/nginx/sites-available/site2
```
`7.Example NGINX Configuration:`
```
server {
    # Bind to the local IP address and the default HTTP port 80
    listen 192.168.1.58:8082;

    # Specify the server name (you can use the IP if there's no domain)
    server_name 192.168.1.58;

    # Root directory where your site's files are located
    root /var/www/site2;

    # Default index file
    index index.html index.htm index.php;

    # Logging configuration
    access_log /var/log/nginx/site2_access.log;
    error_log /var/log/nginx/site2_error.log;

    # Location block to serve requests
    location / {
        try_files $uri $uri/ =404;
    }

    # Optional: Configure PHP if needed
    location ~ \.php$ {
        include snippets/fastcgi-php.conf;
        fastcgi_pass unix:/var/run/php/php7.4-fpm.sock; # Adjust the PHP version if necessary
    }

    # Optional: Handle static files
    location ~* \.(jpg|jpeg|png|gif|ico|css|js)$ {
        expires 365d;
    }
}
```

`8.Enable the site by creating a symbolic link to sites-enabled`
```
sudo ln -s /etc/nginx/sites-available/site1 /etc/nginx/sites-enabled/
sudo ln -s /etc/nginx/sites-available/site2 /etc/nginx/sites-enabled/

```
`9.Test the NGINX configuration to make sure everything is correct:`
```
sudo nginx -t
```
`10.Reload NGINX to apply the changes:`
```
sudo systemctl reload nginx
```

