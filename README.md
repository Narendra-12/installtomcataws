# Install Tomcat 8.x in AWS EC2 Instance (Amazon Linux AMI)

## Software to install
* Java 8
* Tomcat 8.x

## Check YUM Updates
```
sudo su
yum list installed
yum update
```

## Install Java 8
```
yum install java-1.8.0
yum remove java-1.7.0-openjdk
```

## Install Tomcat 8.x
```
yum install tomcat8 tomcat8-webapps tomcat8-admin-webapps tomcat8-docs-webapp
```

## Start Tomcat Service
```
service tomcat8 start
```

## Tomcat Paths
1 Edit the tomcat-users file
```
cd  /usr/share/tomcat8
vim /usr/share/tomcat8/conf/tomcat-users.xml
```
2 Add this line according your desired configuration (vi commands: "i" for insert mode, "ESC" key to escape the inserting mode, ":wq" for write an quit)
```
<user name="admin" password="YOURPASSWORD" roles="admin,manager,admin-gui,admin-script,manager-gui,manager-script,manager-jmx,manager-status" />
```

## Check Tomcat Installation
* fuser: to display the process id(PID) of every process using the specified files
```
fuser -v -n tcp 8080
```
* netstat: to list out all the network (socket) connections on a system
```
netstat -na | grep 8080
```

## Set Auto Start for Tomcat Service
```
sudo chkconfig --list tomcat8
sudo chkconfig tomcat8 on
```

## Install nginx
```
yum install nginx
```

## Add Nginx domain binding
```
vi /etc/nginx/conf.d/shenhe.org.conf
```

```
server {
    listen       80;
    listen       [::]:80;
    server_name  shenhe.org;
    root         /usr/share/nginx/html;
	location / {
    	proxy_connect_timeout 300;
        proxy_send_timeout 300;
        proxy_read_timeout 300;
        proxy_pass http://localhost:8080;
        }
}
```
You can add more binding, it’s similar.
If you just want nginx to handle static files(like images, javascript, css etc), you just do it like this:
```
server {
    listen       80;
    listen       [::]:80;
    server_name  static.shenhe.org;
    root         /usr/share/nginx/html;
}
```
## Start nginx
```
service nginx start
```
## Set Auto Star for Nginx Service
```
sudo chkconfig --list nginx
sudo chkconfig nginx on
```

## Install PostgreSQL
```
sudo yum install postgresql postgresql-server
```

## Initialize postgresql DB
```
sudo service postgresql initdb
```

## Start PostgreSQL
```
sudo service postgresql start
```

## Login in with postgre
```
sudo -u postgres psql
```

## Change password
```
\password postgres
```

## Quit
```
\q
```
## Enable remote access
```
sudo vi /var/lib/pgsql9/data/pg_hba.conf
```
Add you IP to trust IP
```
host all all 125.69.29.0/24     trust
```
Also change this line
From
```
host    all             all             127.0.0.1/32            ident
```
To
```
host    all             all             127.0.0.1/32            md5
```

```
sudo vi /var/lib/pgsql9/data/postgresql.conf
```
Change 
```
#listen_addresses = 'localhost'
```
To:
```
listen_addresses = '*'
```
## Auto Start Postgresql
```
sudo chkconfig --list postgresql
sudo chkconfig postgresql on
```