# kameni-wordpress
ssh -i /path/to/your-key.pem ec2-user@your-ec2-instance-ip
Update your system:


sudo yum update -y
Install and configure MariaDB:


sudo yum install mariadb-server -y
sudo systemctl start mariadb
sudo systemctl enable mariadb
sudo mysql_secure_installation
Follow the prompts to set a secure root password and secure the database.

Install PHP 7 and related modules:


sudo amazon-linux-extras enable php7.2
sudo yum install php php-mysqlnd php-fpm -y
Configure PHP-FPM:
Edit the PHP-FPM configuration file:



sudo nano /etc/php-fpm.d/www.conf
Find the user and group settings and change them to the following:



user = apache
group = apache
Save the file and exit.

Start PHP-FPM and enable it to start on boot:



sudo systemctl start php-fpm
sudo systemctl enable php-fpm
Install and configure WordPress:
Download and extract WordPress to your web server's document root:

#Active and enable https.service

sudo systemctl restart httpd.service
sudo systemctl enable httpd.service


sudo yum install wget -y
cd /var/www/html
sudo wget https://wordpress.org/latest.tar.gz
sudo tar -xvzf latest.tar.gz
sudo mv wordpress/* .
sudo chown -R apache:apache /var/www/html/
Create a MySQL database and user for WordPress:
Log in to MySQL:



mysql -u root -p
Create a new database and user, and grant privileges:

sql

CREATE DATABASE wordpress;
CREATE USER 'wordpressuser'@'localhost' IDENTIFIED BY 'your_password';
GRANT ALL PRIVILEGES ON wordpress.* TO 'wordpressuser'@'localhost';
FLUSH PRIVILEGES;
EXIT;
Configure WordPress:
Create a wp-config.php file:



cp wp-config-sample.php wp-config.php
nano wp-config.php
Edit the wp-config.php file with your database settings:

php

define('DB_NAME', 'wordpress');
define('DB_USER', 'wordpressuser');
define('DB_PASSWORD', 'your_password');
define('DB_HOST', 'localhost');
define('DB_CHARSET', 'utf8mb4');
define('DB_COLLATE', '');
Save the file and exit.




