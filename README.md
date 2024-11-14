## Refer Install & Configuration

  https://github.com/groovemonkey/hands_on_linux-self_hosted_wordpress_for_linux_beginners/blob/master/5-basic-phpfpm-and-php-configuration.md

## Creating the Directory Structure

sudo mkdir -p /var/www/your_domain_1/public_html
sudo mkdir -p /var/www/your_domain_2/public_html

## Granting Permissions

    sudo chown -R $USER:$USER /var/www/your_domain_1/public_html
    sudo chown -R $USER:$USER /var/www/your_domain_2/public_html
    sudo chmod -R 755 /var/www

## Creating Default Pages for Each Virtual Host

    nano /var/www/your_domain_1/public_html/index.html

Add the following content:

   <html>
    <head>
      <title>Welcome to your_domain_1!</title>
    </head>
    <body>
      <h1>Success! The your_domain_1 virtual host is working!</h1>
    </body>
  </html>

cp /var/www/your_domain_1/public_html/index.html /var/www/your_domain_2/public_html/index.html

## Creating New Virtual Host Files

    sudo cp /etc/apache2/sites-available/000-default.conf /etc/apache2/sites-available/your_domain_1.conf
    sudo nano /etc/apache2/sites-available/your_domain_1.conf
    
    <VirtualHost *:80>
        ServerName your_domain_1
        ServerAlias your_domain_1
        ServerAdmin webmaster@localhost
        DocumentRoot /var/www/your_domain_1public_html
        ErrorLog /var/www/your_domain_1/error.log
        CustomLog ${APACHE_LOG_DIR}/access.log combined
        <FilesMatch "\.php$">
            SetHandler "proxy:unix:/run/php/php8.X-fpm.sock|fcgi://localhost/"
        </FilesMatch>
    </VirtualHost>
    
## Enabling the New Virtual Host Files

    sudo a2ensite your_domain_1.conf
    sudo a2ensite your_domain_2.conf

## You're done yet...

    sudo systemctl restart apache2
    sudo systemctl restart php8.x-fpm
