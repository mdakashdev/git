# Step 1 — EC2-তে SSH করো

Mac Terminal:

```bash
ssh -i my-project-key.pem ubuntu@56.10.120.85
```

সফল হলে এমন কিছু দেখাবে:

```text
ubuntu@ip-172-31-5-244:~$
```

## Step 2 — Ubuntu update

Server-এর ভিতরে এই command চালাও:

```bash
sudo apt update
```

এটা Ubuntu-এর package list update করবে।

শেষে সাধারণত এমন কিছু দেখতে পারো:

```text
Reading package lists... Done
Building dependency tree... Done
```

## Step 3 — Installed packages upgrade

তারপর:

```bash
sudo apt upgrade -y
```

এটা installed packages-এর available updates install করবে।

---

### আজকের concept

**`apt update`**
→ নতুন package/update-এর information নেয়।

**`apt upgrade`**
→ available updates install করে।

---

# এখন আমরা server-এ Laravel চালানোর জন্য **PHP environment** তৈরি করব।

## Step 2 — PHP install করার আগে version check

Ubuntu server-এ এই command চালাও:

```bash
php -v
```

## Step 2 — PHP 8.3 install

Server-এ চালাও:

```bash
sudo apt install php8.3-cli -y
```


## কেন `php8.3-cli`?

`CLI` = **Command Line Interface**

Laravel-এর Artisan command চালানোর জন্য PHP CLI দরকার:

```bash
php artisan ...
```


## Step 3 — Composer আছে কিনা check করি

Server-এ চালাও:

```bash
composer --version
```

## Step 3 — Composer Install

Composer হলো PHP-এর **dependency manager**।

Laravel project-এ `vendor/` এবং project-এর PHP packages install করার জন্য Composer ব্যবহার হয়।

### Server-এ চালাও:

```bash id="6q5v7m"
sudo apt install composer -y
```
---

এখন পর্যন্ত server:

```text
Ubuntu
  ↓
PHP 8.3.6       ✅
  ↓
Composer 2.7.1  ✅
```

# Step 4 — Laravel-এর PHP extensions

Laravel শুধু PHP + Composer দিয়ে পুরোপুরি run করবে না। কিছু **PHP extensions** প্রয়োজন হবে।

তবে আমরা আগে তোমার **local Laravel project-এর version** অনুযায়ী exact requirements মিলিয়ে নেব।

তোমার Mac-এ, **Laravel project folder-এর ভিতরে** এই command চালাও:

```bash id="r8q5mp"
php artisan --version
```

এতে এমন output আসবে:

```text id="9zj1qk"
Laravel Framework 12.x.x
```

অথবা 11.x/10.x ইত্যাদি।


## Step 4 — Laravel-এর required PHP extensions

Laravel 13-এর জন্য আমাদের PHP 8.3-এর সাথে প্রয়োজনীয় extensions install করতে হবে।

EC2 server-এ (যেখানে `ubuntu@...` prompt আছে) চালাও:

```bash
sudo apt install php8.3-mysql php8.3-mbstring php8.3-xml php8.3-curl php8.3-zip php8.3-bcmath php8.3-intl -y
```

### এগুলো কেন লাগবে?

| Extension  | কাজ                               |
| ---------- | --------------------------------- |
| `mysql`    | Laravel → MySQL connection        |
| `mbstring` | String/multibyte text handling    |
| `xml`      | XML-related operations            |
| `curl`     | HTTP/API requests                 |
| `zip`      | ZIP/composer packages             |
| `bcmath`   | Precise mathematical calculations |
| `intl`     | Internationalization              |

Install শেষ হলে verify করার জন্য:

```bash
php -m
```


## Step 4.1 — PHP extensions verify

EC2 server-এ চালাও:

```bash id="r6v8kg"
php -m | grep -E 'mysql|mbstring|xml|curl|zip|bcmath|intl'
```

Expected output-এর মধ্যে এগুলো দেখতে পাবে:

```text id="1h9j3u"
bcmath
curl
intl
mbstring
mysqli
mysqlnd
pdo_mysql
xml
zip
```

# Step 5 — Git install/check

এখন আমাদের Git দিয়ে GitHub থেকে Laravel project server-এ আনতে হবে।

প্রথমে check করি Git আগে থেকেই আছে কিনা।

EC2 server-এ চালাও:

```bash id="6j8s2n"
git --version
```

Excellent 👍 **Git 2.43.0 already installed.** ✅

---



# এখন আমাদের server environment-এর basic foundation complete:

```text
EC2 Ubuntu          ✅
PHP 8.3.6           ✅
Composer 2.7.1      ✅
Laravel extensions  ✅
Git 2.43.0          ✅
```

# Step 6 — Server-এ project রাখার directory তৈরি

এখন আমরা Laravel project রাখার জন্য একটা standard directory তৈরি করব।

Server-এ চালাও:

```bash
sudo mkdir -p /var/www
```

তারপর:

```bash
sudo chown -R ubuntu:ubuntu /var/www
```

### কেন?

`/var/www` সাধারণত web applications রাখার জন্য ব্যবহৃত হয়।

আমরা পরে structure এমন করতে পারি:

```text
/var/www/
└── my-laravel-project/
    ├── app/
    ├── routes/
    ├── public/
    ├── storage/
    ├── vendor/
    └── .env
```

আর `chown` দিয়ে `/var/www`-এর ownership তোমার `ubuntu` user-এর কাছে দিচ্ছি, যাতে project clone/install করার সময় permission problem না হয়।

### এখন শুধু এই দুই command চালাও:

```bash
sudo mkdir -p /var/www
sudo chown -R ubuntu:ubuntu /var/www
```

তারপর:

```bash
ls -ld /var/www
```

Outputটা পাঠাও।


# Manual Deployment
