# Phase 1 — AWS EC2 Server তৈরি

## 🎯 এই Phase-এর লক্ষ্য

শেষে তোমার কাছে থাকবে:

```text
AWS
 └── EC2
      └── Ubuntu Server
           └── Public IP
```

এখনো Laravel, MySQL বা Nuxt install করব না।

---

## Step 1 — AWS Console-এ যাও

Mac-এ Chrome খুলে যাও:

👉 [https://aws.amazon.com/](https://aws.amazon.com/)

উপরে **Sign in to Console** button থাকবে।

ওখানে click করো।

তারপর তোমার AWS account দিয়ে login করো।

---

## Step 2 — AWS Region select করো

Login করার পর AWS Console-এর উপরের দিকে একটি **Region** দেখা যাবে।

যেমন:

```text
N. Virginia
```

বা

```text
Asia Pacific (Singapore)
```

তুমি আপাতত **Singapore (`ap-southeast-1`)** select করতে পারো।

### কেন Region?

**English:**
Region means the physical geographical location where AWS keeps your server.

**বাংলা:**
Region মানে AWS-এর কোন geographical location-এ তোমার server থাকবে।

---

## Step 3 — EC2 service খুঁজে বের করো

AWS Console-এর উপরে **Search** box থাকবে।

সেখানে লিখো:

```text
EC2
```

Search result থেকে:

**EC2 — Virtual Servers in the Cloud**

এটাতে click করো।

---

## Step 4 — EC2 Dashboard

এখন তুমি EC2 Dashboard-এ যাবে।

বাম পাশে বা dashboard-এ দেখতে পারবে:

```text
Instances
```

এখানে click করো।

তারপর:

**Launch instances**

button-এ click করো।

---

## Step 5 — Instance Name দাও

একটা field থাকবে:

**Name**

এখানে লিখো:

```text
my-project-server
```

এটা শুধু server চিনতে সুবিধার জন্য।

---

## Step 6 — Operating System নির্বাচন

নিচে **Application and OS Images (AMI)** section থাকবে।

এখানে:

**Ubuntu**

select করো।

তারপর Ubuntu version হিসেবে:

```text
Ubuntu Server 24.04 LTS
```

নাও।

### এটা কী?

**English:**
Ubuntu is the Linux operating system that will run on our AWS server.

**বাংলা:**
Ubuntu হলো আমাদের AWS server-এর operating system।

আমরা পরে এই Ubuntu server-এর ভিতরে গিয়ে Laravel, Nginx, PHP ইত্যাদি install করব।

---

## Step 7 — Instance Type

এরপর আসবে:

**Instance type**

এখানে AWS-এর available **Free tier eligible** option থাকলে সেটি select করো।

AWS console-এ যেটাকে বর্তমানে **Free tier eligible** হিসেবে দেখাচ্ছে, সেটাই নাও।

⚠️ এখানে আমি নির্দিষ্ট instance name ধরে দিচ্ছি না, কারণ AWS-এর free-tier eligibility/account অনুযায়ী option পরিবর্তিত হতে পারে।

---

## Step 8 — Key Pair তৈরি

এটা খুব গুরুত্বপূর্ণ।

Section:

**Key pair (login)**

এখানে:

**Create new key pair**

click করো।

একটা popup আসবে।

### Key pair name

লিখো:

```text
my-project-key
```

### Key pair type

```text
RSA
```

select করো।

### Private key file format

Mac-এর জন্য:

```text
.pem
```

select করো।

তারপর:

**Create key pair**

click করো।

---

## ⚠️ খুব গুরুত্বপূর্ণ

Click করার পর `.pem` file তোমার Mac-এ download হবে।

যেমন:

```text
my-project-key.pem
```

এই file:

* কাউকে দেবে না
* GitHub-এ upload করবে না
* project-এর public folder-এ রাখবে না
* হারাবে না

কারণ এই key দিয়ে আমরা পরে AWS server-এ SSH করে ঢুকব।

---

## Step 9 — Network settings

এখন নিচে আসো:

**Network settings**

এখানে **Create security group** select করা থাকতে পারে।

Security group হচ্ছে আমাদের server-এর **firewall**।

আমরা কোন port দিয়ে server-এ ঢুকতে পারব সেটা এখানে define করব।

---

## SSH

Inbound security rule-এ থাকবে:

```text
Type: SSH
Port: 22
Source: My IP
```

### কেন My IP?

SSH হলো server-এ administrator হিসেবে ঢোকার রাস্তা।

তাই পুরো internet-এর জন্য:

```text
0.0.0.0/0
```

দেওয়ার বদলে আপাতত:

```text
My IP
```

দেব।

---

## HTTP

আরেকটি rule:

```text
Type: HTTP
Port: 80
Source: Anywhere
```

কারণ পরে website internet থেকে access করব।

---

## HTTPS

আরেকটি:

```text
Type: HTTPS
Port: 443
Source: Anywhere
```

পরে SSL/HTTPS-এর জন্য এটা ব্যবহার করব।

---

তাহলে মোটামুটি এমন হবে:

| Type  | Port | Source   |
| ----- | ---: | -------- |
| SSH   |   22 | My IP    |
| HTTP  |   80 | Anywhere |
| HTTPS |  443 | Anywhere |

---

## Step 10 — Storage

এরপর:

**Configure storage**

section আসবে।

Default storage আপাতত পরিবর্তন না করলেও হবে।

আমাদের শেখার জন্য default configuration দিয়েই শুরু করব।

---

## Step 11 — Launch Instance

এখন ডান পাশে বা নিচে:

**Launch instance**

button থাকবে।

Click করো।

কিছুক্ষণ পরে AWS দেখাবে:

```text
Successfully initiated launch of instance
```

এবং একটি Instance ID থাকবে।

যেমন:

```text
i-0123456789abcdef
```

🎉 এখন তোমার AWS server তৈরি হয়েছে।

---

## Step 12 — Instance দেখতে যাও

Click করো:

**View all instances**

অথবা:

**EC2 → Instances → Instances**

এখন তোমার server দেখতে পাবে:

```text
Name
my-project-server
```

Status:

```text
Running
```

এটা খুব গুরুত্বপূর্ণ।

### যদি দেখো:

🟢 **Running**

তাহলে server চালু আছে।

---

# Step 13 — Public IPv4 address খুঁজে বের করো

`my-project-server`-এর উপর click করো।

নিচের **Details** section-এ খুঁজো:

```text
Public IPv4 address
```

যেমন:

```text
13.xxx.xxx.xxx
```

এটাই তোমার server-এর public IP।

এখন এই IP দিয়ে internet থেকে server-এ পৌঁছানো যাবে।

---

## আজ এখানেই থামব

আজ আমাদের লক্ষ্য ছিল শুধু:

```text
AWS Account
     ↓
EC2
     ↓
Ubuntu Server
     ↓
Security Group
     ↓
SSH Key
     ↓
Public IP
```
