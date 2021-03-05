# Install magento2 on ubuntu


Magento 2 Requirement
- PHP >= 7.2 such as php-fpm, ...
- Composer 1.x.x
- Mysql 5.7
- ElasticSearch
- Nginx
- Redis (Optionals)


## update ubuntu
sudo apt update

## Install php 7.2

```bash
sudo apt install php7.2-common php7.2-cli php7.2-fpm php7.2-opcache php7.2-gd php7.2-mysql php7.2-curl php7.2-intl php7.2-xsl php7.2-mbstring php7.2-zip php7.2-bcmath php7.2-soap
```

## check status php7.2
sudo service php7.2-fpm status

## Install composer

```bash
curl -o composer.phar https://getcomposer.org/download/1.10.20/composer.phar

sudo mv composer.phar /usr/local/bin/composer

sudo chmod +x /usr/local/bin/composer

```

## Install mysql service

sudo apt install mysql-server

sudo service mysql status

sudo mysql -u root

database create magento;

Change password for root user: 123456

## Install magento 

composer create-project --repository-url=https://repo.magento.com/ magento/project-community-edition=2.3.5 magento235

Auth - user: cbb842dfe5c6b13a5e82ee3736e43f22 -pass: a3cd6560594ddfb6302579621b75ff88

## Install magento

```bash
bin/magento setup:install \
--base-url=http://magento2.local/ \
--db-host=localhost \
--db-name=magento \
--db-user=root \
--db-password=123456 \
--admin-firstname=admin \
--admin-lastname=admin \
--admin-email=admin@admin.com \
--admin-user=admin \
--admin-password=admin123 \
--language=en_US \
--currency=USD \
--timezone=America/Chicago \
--use-rewrites=1
```

## Setup nginx

sudo vim /etc/nginx/site-availables/magento2.local

Paste this:
  
```bash
upstream fastcgi_backend {
        server  unix:/var/run/php/php7.2-fpm.sock;
}

server {
        listen                  80;
        server_name             ofm22-mdc.local;
        set $MAGE_ROOT          /var/www/ofm22-mdc;
        set $MAGE_MODE          developer;
        include                 /var/www/ofm22-mdc/nginx.conf.sample;
}

```

Save this.

Symlink file

sudo ln -s /etc/nginx/sites-available/magento2.local /etc/nginx/sites-enabled/

### Check config 

sudo nginx -t

sudo service nginx restart

## Change hosts

sudo vim hosts

127.0.0.1 magento2.local

## Change user nginx to your user

sudo vim /etc/nginx/nginx.conf

user <your user name>;

## Change user for php-fpm

sudo vim /etc/php/7.2/fpm/pool.d/www.conf

user = <your user name>
group = <your user name>

listen.owner = <your user name>
listen.group = <your user name>


## Open browser

http://magento2.local

Thanks
