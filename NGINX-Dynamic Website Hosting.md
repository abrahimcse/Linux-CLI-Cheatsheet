To clone This `project` from `Github` directly into the server and deploy it using `NGINX` (assuming your project includes an `Angular frontend`, a `Spring Boot backend`, and a `MySQL database`), follow the steps below.

#
# Step-by-Step Guide

## GitHub Repo Link: `https://shorturl.at/5iiI0`

## 1. Install Required Tools on the Server

Before cloning your project from GitHub, ensure all necessary tools are installed on the server:

`a. Install Git`
```
sudo apt update
sudo apt install git -y
```
`b. Install Java (for Spring Boot)`
```
sudo apt install openjdk-17-jdk -y
```
`c. Install Maven (for Spring Boot build)`

```
sudo apt install maven -y
```
`d. Install Node.js and NPM (for Angular)`
```
sudo apt install nodejs npm -y
```
`e. Install Angular CLI`
```
sudo npm install -g @angular/cli
```
`f. Install MySQL Server`
```
sudo apt install mysql-server -y
```
`g. Secure the MySQL installation:`
```
sudo mysql_secure_installation    
```
`Example Walkthrough`

Here's an example of the prompts and answers you might encounter:
```
Enter current password for root (enter for none): [Press Enter if no password is set]

Set root password? [Y/n] Y
New password: [Enter a new strong password]
Re-enter new password: [Re-enter the new password]

Remove anonymous users? [Y/n] Y
Disallow root login remotely? [Y/n] Y
Remove test database and access to it? [Y/n] Y
Reload privilege tables now? [Y/n] Y
```

## 2. Clone Your GitHub Project

On your server, navigate to a directory where you want to clone your project, e.g., `/opt/myproject`.
```
cd /opt
sudo git clone https://github.com/abrahimcse/sims-dynamic-web.git
```

This will download your project into the `yourproject` folder.

## 3. Set Up the MySQL Database
`1. Log in to MySQL:`
```
sudo mysql -u root -p
```
`2. Create a Database and User:`

```
CREATE DATABASE student_db;
CREATE USER 'stdn_user'@'%' IDENTIFIED BY 'Admin@123';
GRANT ALL PRIVILEGES ON student_db.* TO 'stdn_user'@'%';
FLUSH PRIVILEGES;
```
`3. Exit MySQL:`
```
EXIT;
```
## 4. Build and Deploy Spring Boot Backend
Navigate to the backend folder in the cloned project:
```
cd /opt/sims-dynamic-web/student/backend
```

`a. Update the Spring Boot Database Configuration:`

In the `application.properties` or `application.yml`  update the MySQL database connection settings to match your MySQL configuration.
```
sudo vim src/main/resources/application.properties
```

`For example:`
```
spring.datasource.url=jdbc:mysql://localhost:3306/student_db
spring.datasource.username=stdn_user
spring.datasource.password=Admin@123
```
`b. Build the Spring Boot Application:`
```
cd /opt/sims-dynamic-web/student/backend
sudo mvn clean install
```
This will create a `student-0.0.1-SNAPSHOT.jar` file inside the `/opt/sims-dynamic-web/student/backend/target` directory.


`1. Copy the JAR to a directory:`

The Spring Boot JAR file, copy the built JAR, and run the application as a service.

```
sudo cp /opt/sims-dynamic-web/student/backend/target/student-0.0.1-SNAPSHOT.jar /opt/sims-dynamic-web/student/backend/
```

`2. Create a systemd service to run the backend:`
```
sudo vim /etc/systemd/system/student-backend.service
```
`Add the following content:`

```
[Unit]
Description=Spring Boot Backend

[Service]
User=www-data
ExecStart=/usr/bin/java -jar /opt/sims-dynamic-web/student/backend/student-0.0.1-SNAPSHOT.jar
SuccessExitStatus=143
Restart=always
RestartSec=5

[Install]
WantedBy=multi-user.target
```

`3. Reload systemd and start the service:`

```
sudo systemctl daemon-reload
sudo systemctl start student-backend.service
sudo systemctl enable student-backend.service
```
`4. Check if the backend is running:`
```
sudo systemctl status student-backend.service
```
## 5. Build and Deploy Angular Frontend

`Here API Connect fontend to backend`

Make sure your `environment.prod.ts` or `environment.ts`  but ourfile name file `student.service.ts` is configured correctly, pointing to your backend API.

Here Replace with your Local IP
```
cd /opt/sims-dynamic-web/student/frontend/src/app/service
sudo vim student.service.ts
```
- `Ensure the node_modules directory exists:`
```
cd /opt/sims-dynamic-web/student/frontend/
```
- `Set Up Node.js (Version 18.x)`
```
curl -fsSL https://deb.nodesource.com/setup_18.x | sudo -E bash -
sudo npm install -g n
sudo n stable
sudo n 18.19.0
node -v
```
- `Install Angular Dependencies`
```
sudo npm install
```
- `Check and Fix Security Issues`
```
sudo npm audit
sudo npm audit fix
sudo npm fund
```

`a. Build the Angular Application:`
- `If using Angular CLI 12 or later:`
```
sudo ng build --configuration production
```
- `If using an older Angular version, you may try:`
```
ng build --prod  # (But this might show an "Unknown argument: prod" error)
```
`b. Prepare for Deployment Create and Copy`
```
sudo mkdir -p /var/www/student-info/
cd /opt/sims-dynamic-web/student/frontend/
sudo cp -r dist/* /var/www/student-info/
```

# 6. Configure NGINX as a Reverse Proxy
`a. NGINX Server Install and Configuration for Angular and Spring Boot:`
`Installation Steps:`

```
sudo apt update
sudo apt install nginx -y
```

`Service Management:`
```
sudo systemctl start nginx
sudo systemctl status nginx
sudo systemctl enable nginx
```
`Check HTTP Headers and Response Body`
```
curl -i ip_address 
#example 
curl -i 192.168.1.93
```
Create an NGINX configuration file for your app:

```
sudo vim /etc/nginx/sites-available/student-info
```
`Add the following configuration:`

```
server {
    listen 80;

    server_name 192.168.1.93;  # Replace with your domain or IP

    root /var/www/student-info/student-fontend;
    index index.html;

    location / {
        try_files $uri $uri/ /index.html;
    }

    location /api/ {
        proxy_pass http://localhost:8080/;  # Proxying to Spring Boot Backend
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }

    error_page 404 /404.html;
    location = /404.html {
        internal;
    }
}

```
`b. Enable the NGINX Configuration:`

Create a symbolic link to enable the NGINX site:

```
sudo ln -s /etc/nginx/sites-available/student-info /etc/nginx/sites-enabled/
sudo nginx -t
sudo systemctl reload nginx

```

` Check Spring Boot Application Logs`
```
sudo journalctl -u student-be.service -f
```
```
sudo tail -f /var/log/nginx/error.log
```
## Conclusion
Successfully deploying a dynamic web application involves setting up the right environment, configuring dependencies, and following best practices. With this guide, you should be able to replicate the deployment process on your own server. Feel free to reach out if you have any questions or need further assistance!

