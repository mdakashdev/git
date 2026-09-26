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



Perfect 👍 এটা একদম normal।

বর্তমানে:

```text
~/.ssh/
└── authorized_keys
```

`authorized_keys` হলো **EC2-তে তোমার SSH login-এর জন্য**। GitHub-এর জন্য আলাদা key তৈরি করব।

## Step 7.2 — EC2 → GitHub-এর জন্য SSH key তৈরি

EC2 server-এ চালাও:

```bash
ssh-keygen -t ed25519 -C "ec2-github-deploy"
```

তারপর কয়েকটা প্রশ্ন আসবে।

### 1️⃣ প্রথমে আসবে:

```text
Enter file in which to save the key (/home/ubuntu/.ssh/id_ed25519):
```

👉 এখানে **শুধু Enter** চাপবে।

### 2️⃣ তারপর:

```text
Enter passphrase (empty for no passphrase):
```

👉 আপাতত **Enter** চাপবে।

### 3️⃣ আবার:

```text
Enter same passphrase again:
```

👉 আবার **Enter** চাপবে।

শেষে এরকম দেখাবে:

```text
Your identification has been saved in /home/ubuntu/.ssh/id_ed25519
Your public key has been saved in /home/ubuntu/.ssh/id_ed25519.pub
```

### ⚠️ গুরুত্বপূর্ণ

দুইটা file তৈরি হবে:

```text
~/.ssh/id_ed25519       ← PRIVATE KEY 🔴
~/.ssh/id_ed25519.pub   ← PUBLIC KEY 🟢
```

**Private key কখনো GitHub-এ add করবে না বা আমাকে পাঠাবে না।**

আমরা শুধু `.pub` file-এর content GitHub-এ দেব।

👉 এখন commandটা চালাও:

```bash id="7d3h1k"
ssh-keygen -t ed25519 -C "ec2-github-deploy"
```

শেষ হলে শুধু `done` বলো।


দারুণ 👍 EC2-এর জন্য GitHub SSH key তৈরি হয়ে গেছে। ✅

এখন **public key** বের করব। এটা নিরাপদভাবে GitHub-এ add করা যাবে।

### Step 7.3 — Public key দেখো

EC2 terminal-এ চালাও:

```bash id="xq0j7h"
cat ~/.ssh/id_ed25519.pub
```

Output এমন হবে:

```text
ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAI... ec2-github-deploy
```

### ⚠️ Important

এখানে:

* `id_ed25519.pub` ✅ **Public key**
* `id_ed25519` 🔴 **Private key — কখনো share করবে না**

এখন `.pub` output-টা copy করো।

তারপর GitHub-এ যাও:

**GitHub → Profile picture → Settings → SSH and GPG keys → New SSH key**

তারপর:

```text
Title:
EC2 - my-first-project-server

Key type:
Authentication Key

Key:
[এখানে পুরো cat output paste করবে]
```

তারপর **Add SSH key**।

👉 আগে EC2-তে শুধু এটা চালাও:

```bash
cat ~/.ssh/id_ed25519.pub
```

তারপর GitHub-এ add করো। Add হয়ে গেলে **done** বলো।




দারুণ 👍 এখন **EC2 → GitHub authentication** test করব। Project clone করার আগে এটা verify করা ভালো।

## Step 7.4 — GitHub connection test

EC2 terminal-এ চালাও:

```bash id="t8m3p2"
ssh -T git@github.com
```

প্রথমবার এই ধরনের message আসতে পারে:

```text id="k6q2wd"
The authenticity of host 'github.com' can't be established.
Are you sure you want to continue connecting (yes/no/[fingerprint])?
```

👉 লিখবে:

```text
yes
```

তারপর সফল হলে সাধারণত:

```text id="e3x8pr"
Hi YOUR-GITHUB-USERNAME! You've successfully authenticated, but GitHub does not provide shell access.
```

এটা **success** ✅

> `does not provide shell access` দেখে ভয় পাওয়ার কিছু নেই। GitHub SSH দিয়ে repository access দেয়, কিন্তু GitHub server-এর shell access দেয় না।

### এখন শুধু চালাও:

```bash id="zq7v1c"
ssh -T git@github.com
```

Outputটা পাঠাও।


Excellent! 🎉 **EC2 → GitHub SSH authentication সফল।** ✅

এই line-টাই সবচেয়ে important:

```text
Hi mdakashdev! You've successfully authenticated
```

মানে এখন:

```text id="2y9q8r"
EC2
 │
 │ SSH
 ▼
GitHub
 │
 └── Authentication ✅
```

এখন আমরা project clone করতে পারব।

---

# Step 7.5 — GitHub repository URL বের করি

এখন তোমার **Mac-এর Laravel project folder**-এ যাও।

চালাও:

```bash id="0f4y6w"
git remote -v
```

তোমার output সম্ভবত এরকম হবে:

```text id="9r4w2e"
origin  git@github.com:mdakashdev/my-laravel-project.git (fetch)
origin  git@github.com:mdakashdev/my-laravel-project.git (push)
```

আমাদের **SSH URL** দরকার, যেমন:

```text id="3rj6pw"
git@github.com:mdakashdev/my-laravel-project.git
```

### কেন SSH URL?

কারণ আমরা EC2-তে SSH key configure করেছি:

```text id="8p7s2m"
EC2 SSH Key
      ↓
GitHub
      ↓
Private repository access
```

তাই GitHub username/password দিতে হবে না।

---

👉 এখন **Mac-এর Laravel project folder থেকে**:

```bash id="7q4m1n"
git remote -v
```

চালিয়ে output পাঠাও।

**এখনো EC2-তে `git clone` চালাবে না।** প্রথমে repository URL confirm করি।





# Manual Deployment

আহা, এটা **খুব গুরুত্বপূর্ণ information** 👍 তাহলে structureটা আগে বুঝে নিই। এখনই clone করা ঠিক হবে না।

তোমার repository:

```text
frontend-project
```

এর ভিতরে:

```text
frontend-project/
├── frontend/
└── backend/
```

অর্থাৎ এটা **monorepo-style structure** — একই Git repository-র মধ্যে frontend এবং backend দুটো project।

আর তোমার Git remote:

```text
git@github-local:mdakashdev/frontend-project.git
```

এটা তোমার **Mac-এর custom SSH host alias** (`github-local`)। EC2-তে আমরা এটা সরাসরি ব্যবহার করব না।

EC2 থেকে সাধারণত ব্যবহার করব:

```text
git@github.com:mdakashdev/frontend-project.git
```

### Deployment architecture

আমরা পরে EC2-তে এমন structure রাখতে পারি:

```text
/var/www/frontend-project/
│
├── frontend/       ← Nuxt/Vue
│
└── backend/        ← Laravel
```

তারপর:

```text
                    AWS EC2
                      │
          ┌───────────┴───────────┐
          │                       │
       frontend                backend
       Nuxt/Vue                Laravel
          │                       │
        Nginx                  PHP-FPM
                                  │
                                MySQL
```

এটা তোমার project-এর জন্য অনেক বেশি appropriate হবে।

---

## এখন একটা জিনিস জানতে হবে

Repository-র ভিতরের actual structure জানতে চাই।

তোমার **Mac-এর `frontend-project` root folder** থেকে চালাও:

```bash id="7f3n8m"
ls
```

যদি output হয়:

```text
frontend
backend
```

তাহলে perfect।

আর যদি অন্য কিছু থাকে, সেটাও পাঠাও।

👉 শুধু `ls` outputটা দাও। তারপর আমরা **একটা repository clone করে frontend/backend দুটো কীভাবে deploy করব** সেটা step-by-step করব।


Perfect 👍 এখন একদম clear।

তোমার repository আসলে:

```text id="3f5n8a"
frontend-project/
│
├── emp-management/          ← Frontend
│   ├── src/
│   ├── public/
│   ├── package.json
│   ├── pnpm-lock.yaml
│   └── vite.config.ts
│
└── emp-management-api/      ← Laravel Backend
    ├── app/
    ├── bootstrap/
    ├── config/
    ├── database/
    ├── routes/
    ├── artisan
    ├── composer.json
    └── composer.lock
```

একটা গুরুত্বপূর্ণ correction: **এটা Nuxt project নয়, Vite-based frontend**—কারণ `vite.config.ts`, `src/`, `index.html` আছে এবং Nuxt-এর typical `nuxt.config.*`/`.nuxt` structure নেই।

---

# এখন Deployment Plan

আমরা এই repository **একবারই EC2-তে clone** করব:

```text
GitHub
   │
   │ git clone
   ▼
/var/www/frontend-project/
│
├── emp-management/
│
└── emp-management-api/
```

তারপর দুইটা আলাদা deployment:

### Frontend

```text
emp-management
      ↓
pnpm install
      ↓
pnpm build
      ↓
dist/
      ↓
Nginx
```

### Laravel API

```text
emp-management-api
      ↓
composer install
      ↓
.env
      ↓
MySQL
      ↓
PHP-FPM
      ↓
Nginx
```

শেষে:

```text
Browser
   │
   ├── Frontend → Nginx → Vite build
   │
   └── API → Nginx → PHP-FPM → Laravel → MySQL
```

এটাই আমরা ধাপে ধাপে বানাব। 🚀

---

## Step 7.7 — এবার Repository Clone

EC2 server-এ ফিরে যাও।

প্রথমে নিশ্চিত হও:

```bash
cd /var/www
```

তারপর:

```bash
git clone git@github.com:mdakashdev/frontend-project.git
```

Clone শেষ হলে:

```bash
ls
```

Expected:

```text
frontend-project
```

তারপর:

```bash
ls frontend-project
```

Expected:

```text
emp-management
emp-management-api
project-note.md
sketch
working.md
```

### এখন শুধু এই 3টা command চালাও:

```bash
cd /var/www
git clone git@github.com:mdakashdev/frontend-project.git
ls
```

**Clone successful হলে থামবে।** তারপর আমাকে output দাও। এরপর আমরা আগে **Laravel backend deployment** করব।


Perfect! 🎉 **GitHub → EC2 clone successfully complete.** ✅

এখন আমাদের server-এ actual project আছে:

```text
/var/www/frontend-project/
│
├── emp-management/       ← Frontend
└── emp-management-api/   ← Laravel Backend
```

এটা আমাদের **Manual Deployment-এর first major milestone**।

---

# Step 8 — Laravel Backend Setup

আমরা আগে **backend/API** deploy করব। Frontend পরে।

Laravel project-এ ঢুকো:

```bash id="3g8c4n"
cd /var/www/frontend-project/emp-management-api
```

তারপর check করো:

```bash id="w5g3xq"
ls
```

তোমার আগের structure অনুযায়ী এখানে থাকবে:

```text
app
artisan
bootstrap
composer.json
config
database
public
resources
routes
storage
...
```

### Step 8.1 — Composer dependencies

যেহেতু repository-তে `composer.lock` already আছে, production deployment-এ আমরা:

```bash id="5o8n2k"
composer install --no-dev --optimize-autoloader
```

ব্যবহার করব।

**কিন্তু একটা বিষয়:** তোমার project-এ `vendor/` already Git repository-তে দেখা গেছে। সেটা দেখে বোঝা যাচ্ছে local machine থেকে `vendor` folder repository-তে committed হতে পারে। সাধারণত Laravel project-এ `vendor/` Git-এ commit করা হয় না।

তাই আগে check করি:

```bash id="4z0g6p"
git status
```

👉 এই command চালাও:

```bash id="j5x8q1"
cd /var/www/frontend-project/emp-management-api
git status
```

Outputটা পাঠাও। **এখনো `composer install` চালাবে না।**


Perfect 👍 `git status` clean। এখন আরও ভালোভাবে বুঝলাম—`vendor/` server-এ নেই। আগের local listing-এ `vendor` ছিল, কিন্তু Git clone-এ আসেনি, অর্থাৎ সম্ভবত `.gitignore`-এ আছে। **এটাই expected Laravel setup।** ✅

## Step 8.2 — Composer dependencies install

আমরা এখন Laravel backend-এর PHP packages install করব।

তুমি এখন already আছো:

```text
/var/www/frontend-project/emp-management-api
```

এই command চালাও:

```bash
composer install --no-dev --optimize-autoloader
```

### এটা কী করবে?

```text
composer.json
      +
composer.lock
      ↓
   composer install
      ↓
vendor/
```

অর্থাৎ Laravel-এর প্রয়োজনীয় packages `vendor/` folder-এ install হবে।

`--no-dev`
→ development-only packages install করবে না।

`--optimize-autoloader`
→ production-এর জন্য Composer autoloader optimize করবে।

### Expected

শেষের দিকে এরকম কিছু দেখতে পারো:

```text
Installing dependencies from lock file
...
Generating optimized autoload files
```

তারপর check করব:

```bash
ls -la vendor
```

👉 এখন শুধু চালাও:

```bash
composer install --no-dev --optimize-autoloader
```

শেষ হলে outputটা পাঠাও।


Perfect ✅ `vendor/` successfully created. That means:

**English:** Composer installation is complete, and Laravel dependencies are ready.

**বাংলা:** `vendor/` তৈরি হয়েছে, অর্থাৎ Composer successfully সব Laravel dependencies install করেছে। 👍

### Next Step — Check `.env.example`

Run:

```bash
ls -la | grep .env
```

**Expected:**

```text
.env.example
```

এটা run করে output আমাকে দিন। তারপর আমরা **Laravel `.env` configuration** করব।


Perfect ✅ `.env.example` আছে।

### Step 2 — `.env` তৈরি করুন

**English:** Laravel production server needs its own `.env` file. We will copy `.env.example` first, then configure the database and app settings.

**বাংলা:** Laravel server-এ `.env` file দরকার। আগে `.env.example` থেকে `.env` তৈরি করব, পরে database settings configure করব।

Run:

```bash
cp .env.example .env
```

তারপর check করুন:

```bash
ls -la | grep .env
```

Expected:

```text
.env
.env.example
```

এটা হয়ে গেলে শুধু **`done`** বলুন।


Perfect ✅ `.env` successfully তৈরি হয়েছে।

### Step 3 — Generate Laravel APP_KEY

**English:** Laravel needs a unique encryption key in `APP_KEY`. The `key:generate` command will create it automatically and save it inside `.env`.

**বাংলা:** Laravel-এর encryption-এর জন্য একটি `APP_KEY` দরকার। এই command নিজে থেকেই key তৈরি করে `.env`-এ save করবে।

Run:

```bash
php artisan key:generate
```

**Expected output:**

```text
INFO  Application key set successfully.
```

এটা run করে output দিন।


Great ✅ `APP_KEY` set হয়ে গেছে।

### Step 4 — Database setup

এখন Laravel-এর জন্য **MySQL database** লাগবে।

**English:** Before installing/configuring MySQL, we need to decide where the database will live:

* **MySQL on the same EC2** → easier for learning and our first deployment.
* **AWS RDS MySQL** → more production-like and recommended for real-world architecture.

**বাংলা:** এখন Laravel-এর database লাগবে। দুইটা option আছে:

* **একই EC2 server-এ MySQL** → শেখার জন্য সহজ।
* **AWS RDS MySQL** → production-এর জন্য বেশি standard architecture।

আমাদের **প্রথম manual deployment শেখার জন্য**, আমি আগে **EC2-এর ভিতর MySQL** দিয়ে শুরু করার পরামর্শ দেব। পরে চাইলে একই project-কে **RDS-এ migrate** করব।

যদি এটা ঠিক থাকে, next step হবে **EC2-তে MySQL install করা**।





# Step 4 — Database setup

এখন Laravel-এর জন্য **MySQL database** লাগবে।

**English:** Before installing/configuring MySQL, we need to decide where the database will live:

* **MySQL on the same EC2** → easier for learning and our first deployment.
* **AWS RDS MySQL** → more production-like and recommended for real-world architecture.

**বাংলা:** এখন Laravel-এর database লাগবে। দুইটা option আছে:

* **একই EC2 server-এ MySQL** → শেখার জন্য সহজ।
* **AWS RDS MySQL** → production-এর জন্য বেশি standard architecture।

আমাদের **প্রথম manual deployment শেখার জন্য**, আমি আগে **EC2-এর ভিতর MySQL** দিয়ে শুরু করার পরামর্শ দেব। পরে চাইলে একই project-কে **RDS-এ migrate** করব।

যদি এটা ঠিক থাকে, next step হবে **EC2-তে MySQL install করা**।


ঠিক আছে 👍

## Step 4.1 — MySQL install

**English:** We’ll install MySQL Server on the EC2 machine.

**বাংলা:** এখন EC2 server-এর ভিতরে MySQL Server install করব।

Run:

```bash
sudo apt install mysql-server -y
```

Install শেষ হলে check করুন:

```bash
mysql --version
```

Expected এরকম হবে:

```text
mysql  Ver 8.0.x ...
```

শুধু এই দুইটা command run করে **output দিন**। তারপর পরের একটাই step করব।


হ্যাঁ, **এই path থেকেই command চালাতে পারবেন**। তবে একটা important বিষয়:

**MySQL install করার জন্য specific folder-এর দরকার নেই।** `apt install` পুরো Ubuntu server-এ MySQL install করবে, current directory যেটাই হোক না কেন।

আপনি এখন আছেন:

```bash
/var/www/frontend-project/emp-management-api
```

এখান থেকেই চালান:

```bash
sudo apt install mysql-server -y
```

তারপর:

```bash
mysql --version
```

**বাংলা:** Current path নিয়ে চিন্তা করার দরকার নেই। MySQL system-level software হিসেবে পুরো EC2 server-এ install হবে, Laravel project folder-এর ভিতরে নয়।


Perfect ✅ MySQL install হয়ে গেছে।

### Step 4.2 — MySQL service চলছে কিনা check করি

**English:** First, let's verify that MySQL Server is running.

**বাংলা:** আগে নিশ্চিত করি MySQL server ঠিকভাবে running আছে কিনা।

Run:

```bash
sudo systemctl status mysql
```

Expected:

```text
Active: active (running)
```

⚠️ এখনো কিছু configure করবেন না। শুধু এই command-এর output দেখুন এবং আমাকে দিন।


Perfect ✅ MySQL ঠিকভাবে চলছে।

আপনার output-এর গুরুত্বপূর্ণ অংশ:

```text
Active: active (running)
Status: "Server is operational"
```

মানে **MySQL server ready**. 👍

### Step 4.3 — MySQL-এ login

এখন আমরা MySQL-এর ভিতরে ঢুকে database এবং user তৈরি করব।

Run:

```bash
sudo mysql
```

Expected:

```text
Welcome to the MySQL monitor...
mysql>
```

যখন `mysql>` দেখবেন, **ওখানেই থামুন** এবং আমাকে output দিন।


Perfect ✅ আপনি MySQL-এর ভিতরে আছেন এবং এখনো কোনো application database তৈরি হয়নি।

### Step 4.4 — Laravel database তৈরি করি

**English:** We'll create a separate database for your Laravel application.

**বাংলা:** আপনার Laravel application-এর জন্য আলাদা একটি database তৈরি করব।

MySQL prompt-এ:

```sql
CREATE DATABASE emp_management;
```

তারপর verify করুন:

```sql
SHOW DATABASES;
```

Expected list-এ দেখতে পাবেন:

```text
emp_management
```

⚠️ এখন শুধু এই দুইটা command চালান। `mysql>` prompt-এ আছেন, তাই এখানেই চালাবেন।

Output দিন, তারপর **database user** তৈরি করব।



Great ✅ `emp_management` database তৈরি হয়ে গেছে।

### Step 4.5 — Laravel-এর জন্য MySQL user তৈরি

**English:** We should use a dedicated database user for Laravel instead of using the MySQL `root` user.

**বাংলা:** Laravel-এর জন্য `root` user ব্যবহার না করে আলাদা database user তৈরি করা ভালো practice।

MySQL prompt-এ এই command চালান:

```sql
CREATE USER 'emp_user'@'localhost' IDENTIFIED BY 'Emp@2026Secure!';
```

তারপর এই user-কে শুধু `emp_management` database-এর permission দেব:

```sql
GRANT ALL PRIVILEGES ON emp_management.* TO 'emp_user'@'localhost';
```

তারপর:

```sql
FLUSH PRIVILEGES;
```

⚠️ **Important:** এই password শুধু শেখার server-এর জন্য ব্যবহার করছি। Production-এ আরও strong/random secret ব্যবহার করব এবং GitHub-এ কখনো commit করব না।

এই ৩টা command চালিয়ে **done** বলুন।



Perfect ✅ Database user এবং permission তৈরি হয়ে গেছে।

### Step 4.6 — MySQL থেকে বের হই

এখন MySQL prompt-এ লিখুন:

```sql id="n9g4yq"
EXIT;
```

Expected:

```text
Bye
```

এরপর আপনি আবার এই ধরনের terminal prompt-এ ফিরে আসবেন:

```text
ubuntu@ip-172-31-5-244:/var/www/frontend-project/emp-management-api$
```

**বাংলা:** এখন MySQL configuration শেষ। Laravel-এর `.env`-এ database connection configure করার আগে MySQL থেকে বের হয়ে আসছি।

এটা করে **done** বলুন।


Perfect ✅ এখন Laravel-এর সাথে MySQL connect করব।

### Step 4.7 — `.env` database configuration

আপনি এখন এই folder-এ আছেন:

```text
/var/www/frontend-project/emp-management-api
```

প্রথমে `.env`-এর database অংশটা দেখুন:

```bash
grep -A 8 '^DB_' .env
```

এটার output আমাকে দিন।

**English:** We’ll first inspect the current DB settings before changing anything.

**বাংলা:** আগে `.env`-এ বর্তমানে কী database configuration আছে সেটা দেখি। তারপর প্রয়োজনীয় values পরিবর্তন করব।



হ্যাঁ, configuration দেখা যাচ্ছে। 👍

এখন আমাদের শুধু এই ৩টা value পরিবর্তন করতে হবে:

```text
DB_DATABASE=emp_management
DB_USERNAME=emp_user
DB_PASSWORD=Emp@2026Secure!
```

`DB_CONNECTION`, `DB_HOST`, `DB_PORT` ঠিক আছে।

### Step 4.8 — `.env` update

সবচেয়ে সহজভাবে এই command চালান:

```bash
sed -i 's/^DB_DATABASE=.*/DB_DATABASE=emp_management/' .env
sed -i 's/^DB_USERNAME=.*/DB_USERNAME=emp_user/' .env
sed -i 's/^DB_PASSWORD=.*/DB_PASSWORD=Emp@2026Secure!/' .env
```

তারপর verify করুন:

```bash
grep -A 5 '^DB_' .env
```

Expected:

```text
DB_CONNECTION=mysql
DB_HOST=127.0.0.1
DB_PORT=3306
DB_DATABASE=emp_management
DB_USERNAME=emp_user
DB_PASSWORD=Emp@2026Secure!
```

⚠️ **Note:** এই password chat/GitHub-এ publish করবেন না। `.env` সাধারণত Git-এ commit করা হয় না।

এগুলো চালিয়ে output দিন।



Perfect ✅ `.env`-এ database configuration ঠিকভাবে update হয়েছে।

### Step 4.9 — Laravel → MySQL connection test

এখন আমরা **কোনো migration চালাব না**। আগে শুধু Laravel MySQL-এ connect করতে পারছে কিনা test করব।

Run:

```bash
php artisan migrate:status
```

### Expected

যদি connection ঠিক থাকে, এরকম output আসবে:

```text
Migration name .................................................... Batch / Status
0001_01_01_000000_create_users_table ............................. Pending
...
```

অথবা migration list দেখাবে।

**বাংলা:** যদি `migrate:status` database connection successfully করতে পারে, তাহলে বুঝব:

```text
Laravel
   ↓
.env
   ↓
MySQL
   ↓
emp_management
```

সব ঠিক আছে। এখন command-টা চালিয়ে **পুরো output দিন**।



এটা **error হলেও connection-এর সমস্যা নয়**। 👍

```text
ERROR  Migration table not found.
```

এর অর্থ Laravel MySQL database-এ successfully পৌঁছাতে পেরেছে, কিন্তু `migrations` table এখনো তৈরি হয়নি। কারণ আমরা এখনো migration চালাইনি।

### Step 4.10 — Laravel migrations run করি

এখন চালান:

```bash
php artisan migrate
```

Laravel আপনাকে migration চালানোর confirmation চাইতে পারে, বিশেষ করে production environment detect করলে।

যদি এমন আসে:

```text
APPLICATION IN PRODUCTION.
Do you really wish to run this command?
```

তাহলে:

```text
yes
```

দেবেন।

**বাংলা:** এই command Laravel-এর প্রয়োজনীয় tables যেমন `users`, `cache`, `jobs`, `sessions` ইত্যাদি আপনার `emp_management` database-এ তৈরি করবে—আপনার project's migration files অনুযায়ী।

শুধু:

```bash
php artisan migrate
```

চালান এবং output দিন।




Excellent ✅ এখন সব migration successfully run হয়েছে।

আপনার output:

```text
create_users_table                 [1] Ran
create_cache_table                 [1] Ran
create_jobs_table                  [1] Ran
create_employees_table             [1] Ran
create_personal_access_tokens_table [1] Ran
```

এর মানে:

```text
Laravel
   ↓
.env
   ↓
MySQL
   ↓
emp_management
   ↓
All migrations ✅
```

### পরের Step — Laravel application-এর basic production check

এখন আমরা Nginx/PHP-FPM install করার আগে Laravel নিজে ঠিকভাবে কাজ করছে কিনা check করব।

Run:

```bash
php artisan about
```

এটা Laravel application-এর configuration/environment সম্পর্কে summary দেখাবে।

**এখন শুধু এই command চালান এবং output দিন।**
