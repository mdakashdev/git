# Target 
- EC2 instance create
- Mac Terminal থেকে **SSH connection সফল**
- EC2 server preparaion 
- Deployment - Manually
- Deployment using CI/CD
- docker project


# Done

1. AWS EC2 Server তৈরি
2. Mac Terminal থেকে **SSH connection সফল**
3. EC2 Server Preparation


# Server environment-এর basic foundation:

```text
EC2 Ubuntu          ✅
PHP 8.3.6           ✅
Composer 2.7.1      ✅
Laravel extensions  ✅
Git 2.43.0          ✅
```

✅ MySQL connected
✅ Laravel migrations done
✅ Production mode
✅ Debug disabled
✅ Storage link created
✅ PHP-FPM running
✅ Nginx running
✅ Laravel response = `200 OK`

# Laravel backend deployment milestone complete


| Component         | Status |
| ----------------- | ------ |
| EC2               | ✅      |
| GitHub Clone      | ✅      |
| PHP 8.3           | ✅      |
| Composer          | ✅      |
| Laravel           | ✅      |
| MySQL             | ✅      |
| Database          | ✅      |
| Migrations        | ✅      |
| PHP-FPM           | ✅      |
| Nginx             | ✅      |
| Production `.env` | ✅      |
| Laravel Storage   | ✅      |
| Public Access     | ✅      |


--- 

# Summary

- SSH connect (mac or browser)
- Ubuntu update & upgrade
- Server-এ Laravel চালানোর জন্য `PHP environment` তৈরি করব।
- Install: PHP 8.3, composer, GIT (if didn't install)
- Laravel-এর PHP extensions
- Server-এ project রাখার directory তৈরি
- EC2 theke GitHub-এর জন্য SSH key তৈরি : EC2 -> SSH -> GitHub (authentication ✅)
- Github profile a ssh key add 
- repository clone 
- Laravel Backend Setup - Composer dependencies, .env, APP_KEY, Database setup, mysql login, db create, mysql user create
- Laravel-এর সাথে MySQL connect
- PHP-FPM install
- Nginx install
- Laravel-এর জন্য Nginx config file তৈরি
- Laravel API test
- RUN - EC2 Public IP
---

## EC2-তে SSH করো

Mac Terminal: jei folder my-project-key.pem ta ache sekhan theke 

```bash
ssh -i my-project-key.pem ubuntu@56.10.120.85
```

Or Browser theke connect from AWS

## Ubuntu update

এটা Ubuntu-এর package list update করবে।

```bash
sudo apt update
```

এটা installed packages-এর available updates install করবে।

```bash
sudo apt upgrade -y
```

## PHP environment

version check : 

```bash
php -v
composer --version
git --version
```

— PHP 8.3 & composer install

```bash
sudo apt install php8.3-cli -y
```

```bash
sudo apt install composer -y
```

## Laravel-এর PHP extensions

local project er sathe miliye eigulo install korbo, se jonno age local project `php artisan --version` check korbo. then server sei onujai install korbo.

```bash
sudo apt install php8.3-mysql php8.3-mbstring php8.3-xml php8.3-curl php8.3-zip php8.3-bcmath php8.3-intl -y
```

Install শেষ হলে verify করার জন্য:

```bash
php -m
```

EC2 server-এ চালাও:

```bash id="r6v8kg"
php -m | grep -E 'mysql|mbstring|xml|curl|zip|bcmath|intl'
```

## Server-এ project রাখার directory তৈরি

```bash
sudo mkdir -p /var/www
```

তারপর:

```bash
sudo chown -R ubuntu:ubuntu /var/www
```

```bash
ls -ld /var/www
```

## EC2 theke GitHub-এর জন্য SSH key তৈরি

```bash
ssh-keygen -t ed25519 -C "ec2-github-deploy"
```

এখানে **শুধু Enter** চাপবে। sobjaigia enter then get 2 files

mane, EC2-এর জন্য GitHub SSH key তৈরি হয়ে গেছে।

> Public key দেখো: 

```bash id="xq0j7h"
cat ~/.ssh/id_ed25519.pub
```

> তারপর GitHub-এ profile a যাও:

**GitHub → Profile picture → Settings → SSH and GPG keys → New SSH key**

then public key ta copy kore, ekhene bosiye diye, তারপর **Add SSH key**।

> GitHub connection test:

```bash id="t8m3p2"
ssh -T git@github.com
```

then yes dile, emon output pabe.

```text id="e3x8pr"
Hi YOUR-GITHUB-USERNAME! You've successfully authenticated, but GitHub does not provide shell access.
```

> এখন তোমার **Mac-এর Laravel project folder**-এ যাও।

```bash id="0f4y6w"
git remote -v
```

> Repository Clone

```bash
cd /var/www
git clone git@github.com:mdakashdev/frontend-project.git
ls
```

## Laravel Backend Setup

```bash id="3g8c4n"
cd /var/www/frontend-project/emp-management-api
ls
```

> Composer dependencies install

```bash id="5o8n2k"
composer install --no-dev --optimize-autoloader
```

> Check `.env.example`

```bash
ls -la | grep .env
```

```bash
cp .env.example .env
```

> Generate Laravel APP_KEY

```bash
php artisan key:generate
```

> Database setup

```bash
sudo apt install mysql-server -y
mysql --version
sudo systemctl status mysql
```

> MySQL-এ login

```bash
sudo mysql
CREATE DATABASE emp_management;
SHOW DATABASES;
```

> Laravel-এর জন্য MySQL user তৈরি

```sql
CREATE USER 'emp_user'@'localhost' IDENTIFIED BY 'Emp@2026Secure!';
GRANT ALL PRIVILEGES ON emp_management.* TO 'emp_user'@'localhost';
FLUSH PRIVILEGES;
```

```sql id="n9g4yq"
EXIT;
```

> Laravel-এর সাথে MySQL connect

```bash
grep -A 8 '^DB_' .env
grep '^APP_ENV=' .env
grep '^APP_DEBUG=' .env
```

```bash
sed -i 's/^DB_DATABASE=.*/DB_DATABASE=emp_management/' .env
sed -i 's/^DB_USERNAME=.*/DB_USERNAME=emp_user/' .env
sed -i 's/^DB_PASSWORD=.*/DB_PASSWORD=Emp@2026Secure!/' .env

sed -i 's/^APP_ENV=.*/APP_ENV=production/' .env

sed -i 's/^APP_DEBUG=.*/APP_DEBUG=false/' .env
```

```bash
php artisan migrate
php artisan migrate:status
```

> Laravel application-এর basic production check

```bash
php artisan about
```

```bash
php artisan config:cache
```

```bash id="s8f3k2"
php artisan storage:link
```

## PHP-FPM install

```bash id="f5x2k8"
sudo apt install php8.3-fpm -y
sudo systemctl status php8.3-fpm
```

## Nginx install


```bash
sudo apt install nginx -y
sudo systemctl status nginx
```

## Laravel-এর জন্য Nginx config file তৈরি

> config file তৈরি করুন:

```bash id="n3c4m7"
sudo nano /etc/nginx/sites-available/emp-management-api
```

```nginx id="8p7q2r"
server {
    listen 80;
    listen [::]:80;

    server_name _;

    root /var/www/frontend-project/emp-management-api/public;

    index index.php index.html;

    location / {
        try_files $uri $uri/ /index.php?$query_string;
    }

    location ~ \.php$ {
        include snippets/fastcgi-php.conf;
        fastcgi_pass unix:/run/php/php8.3-fpm.sock;
    }

    location ~ /\.ht {
        deny all;
    }
}
```

> Config syntax test

```bash id="n6r8w2"
sudo nginx -t
```

> Laravel Nginx site enable

```bash id="z9q3kw"
sudo ln -s /etc/nginx/sites-available/emp-management-api /etc/nginx/sites-enabled/emp-management-api
```

তারপর default Nginx site disable করি, যাতে default welcome page না আসে:

```bash id="k2m6vp"
sudo rm /etc/nginx/sites-enabled/default
```

তারপর আবার config test:

```bash id="p7v4cx"
sudo nginx -t
```

সবশেষে Nginx reload:

```bash id="m3j8qa"
sudo systemctl reload nginx
```

## Laravel API test

```bash id="w4r6t2"
curl -I http://127.0.0.1
```

error asle check korbo :

> Laravel log দেখুন

```bash
tail -n 30 storage/logs/laravel.log
```

> Permission ঠিক করি

```bash id="9f2xka"
sudo chown -R www-data:www-data storage bootstrap/cache
sudo chmod -R 775 storage bootstrap/cache
```

then again test: 

```bash
curl -I http://127.0.0.1
```

## Run

আপনার EC2 Public IP:

```text
56.10.120.85
```