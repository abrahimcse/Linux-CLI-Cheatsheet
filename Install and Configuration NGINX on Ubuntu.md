# Install and Configuration `NGINEX` on Ubuntu 20.04 & 22.04

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

```
sudo mkdir -p /var/www/example.com
```
`5.Create a sample index.html page`
```
vim /var/www/example.com/index.html
```
```
<html>
    <head>
        <title>Welcome to  our Server!</title>
    </head>
    <body>
        <h1>Success!  The example.com server block is working!</h1>
    </body>
</html>
```

**OR Store full Project file here** `/var/www/example.com`

`6.Create a server block with the correct directives`
```
sudo vim /etc/nginx/sites-available/example.com
```
`7.Example NGINX Configuration:`
```
server {
    # Bind to the local IP address and the default HTTP port 80
    listen 192.168.1.58:80;

    # Specify the server name (you can use the IP if there's no domain)
    server_name example.com www.example.com 192.168.1.58;

    # Root directory where your site's files are located
    root /var/www/example.com;

    # Default index file
    index index.html index.htm index.php;

    # Logging configuration
    access_log /var/log/nginx/example_access.log;
    error_log /var/log/nginx/example_error.log;

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
sudo ln -s /etc/nginx/sites-available/example.com /etc/nginx/sites-enabled/
```
`9.Test the NGINX configuration to make sure everything is correct:`
```
sudo nginx -t
```
`10.Reload NGINX to apply the changes:`
```
sudo systemctl reload nginx
```

