হ্যাঁ—এখন তোমার শেখার জন্য **CI/CD setup-এ যাওয়া উচিত**। আর আমার recommendation হলো **এই EC2-তেই CI/CD setup করা**, নতুন EC2 বানিয়ে পুরো process আবার করা দরকার নেই।

তোমার বর্তমান setup-টাই আমাদের **deployment target/server** হিসেবে থাকবে:

```text
GitHub
   │
   │ push to main
   ▼
GitHub Actions
   │
   │ SSH
   ▼
Existing EC2
   ├── Laravel
   ├── Vue
   ├── Nginx
   └── MySQL
```

### কেন নতুন EC2 দরকার নেই?

তুমি ইতিমধ্যে manually শিখেছো:

```text
EC2 setup
→ PHP
→ Composer
→ MySQL
→ Laravel
→ Node/pnpm
→ Vue build
→ Nginx
→ Frontend + API
```

এখন CI/CD-তে আমাদের focus হবে **এই manual কাজগুলো automatic করা**।

যেমন আগে তুমি করেছো:

```bash
git pull
composer install
pnpm install
pnpm build
sudo systemctl reload nginx
```

CI/CD হলে এগুলো GitHub Actions automatically করবে।

---

## আমরা এবার যেভাবে শিখব

আমি তোমাকে সরাসরি complex pipeline দেব না। ধাপে ধাপে করব:

### Phase 1 — GitHub Actions basics

```text
Push code
   ↓
GitHub Actions starts
   ↓
Run a simple command
   ↓
Workflow success
```

### Phase 2 — CI

প্রথমে automated checks:

```text
Push
 ↓
Install dependencies
 ↓
Run tests
 ↓
Build frontend
 ↓
Success / Failure
```

### Phase 3 — CD

তারপর:

```text
GitHub
   ↓
Build
   ↓
SSH into EC2
   ↓
git pull
   ↓
Backend deploy
   ↓
Frontend build
   ↓
Nginx reload
```

### Phase 4 — Proper production-style workflow

শেষে:

```text
Developer
   ↓
feature branch
   ↓
Pull Request
   ↓
GitHub Actions CI
   ↓
Merge to main
   ↓
GitHub Actions CD
   ↓
EC2 deployment
```

---

### একটা গুরুত্বপূর্ণ change

এখন থেকে **EC2-তে manually code edit করব না**।

Code change হবে:

```text
Local Mac
   ↓
GitHub
   ↓
GitHub Actions
   ↓
EC2
```

EC2 হবে শুধু **deployment server**।

আর তোমার `deployment-setup` branch যেটা আমরা বানিয়েছি, সেটাও এখন থাকবে। পরে আমরা decide করব সেটা `main`-এ merge করব নাকি শুধু learning branch হিসেবে রাখব।

**তাই নতুন EC2 create করার দরকার নেই। Existing EC2-তেই CI/CD setup শুরু করি।**

---

## Step 1 — GitHub Actions-এর প্রথম workflow

এখন **local Mac** থেকে কাজ করব, EC2 থেকে নয়।

তোমার project folder:

```text
frontend-project/
```

এর মধ্যে এই folder তৈরি করো:

```text
.github/workflows
```

Terminal-এ project root থেকে:

```bash
mkdir -p .github/workflows
```

তারপর:

```bash
touch .github/workflows/ci.yml
```

এখন file open করো:

```bash
code .github/workflows/ci.yml
```

যদি VS Code `code` command কাজ না করে, বলো—আমি অন্যভাবে দেখাব।

**এখন শুধু file তৈরি করো।** তারপর `done` বলো।


Great 👍 এখন local `main` branch-এ আছো।

### Step 2 — CI workflow তৈরি

`frontend-project/.github/workflows/ci.yml` file-এ এইটা paste করো:

```yaml
name: CI

on:
  push:
    branches:
      - main

  pull_request:
    branches:
      - main

jobs:
  test:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout code
        uses: actions/checkout@v4

      - name: Setup Node.js
        uses: actions/setup-node@v4
        with:
          node-version: 22

      - name: Setup pnpm
        uses: pnpm/action-setup@v4
        with:
          version: 8.14.1

      - name: Install frontend dependencies
        working-directory: emp-management
        run: pnpm install --frozen-lockfile

      - name: Type check frontend
        working-directory: emp-management
        run: pnpm type-check

      - name: Build frontend
        working-directory: emp-management
        run: pnpm build
```

### এই workflow কী করবে?

যখন:

```text
main-এ push
      ↓
GitHub Actions
      ↓
Node + pnpm setup
      ↓
pnpm install
      ↓
TypeScript check
      ↓
Frontend build
```

কোনো step fail করলে GitHub Actions **failed** দেখাবে এবং আমরা বুঝব code deploy করার আগে সমস্যা আছে।

এখন **save করো**, কিন্তু commit/push এখনো করো না।

শুধু `done` বলো।



Commit হয়েছে ✅ তবে একটা **খুব গুরুত্বপূর্ণ বিষয়** দেখলাম:

তোমার commit output বলছে:

```text
create mode 100644 emp-management/.github/workflows/ci.yml
```

মানে workflow file তৈরি হয়েছে:

```text
frontend-project/
└── emp-management/
    └── .github/
        └── workflows/
            └── ci.yml
```

কিন্তু GitHub Actions workflow অবশ্যই repository root-এর:

```text
frontend-project/
└── .github/
    └── workflows/
        └── ci.yml
```

হতে হবে।

তাই push করার আগে location ঠিক করি।

### Step 5 — এখন তুমি কোথায় আছো দেখি

চালাও:

```bash
pwd
```

তারপর:

```bash
git rev-parse --show-toplevel
```

দুইটার output পাঠাও।


Excellent ✅ `main` successfully GitHub-এ push হয়েছে।

### Step 4 — GitHub Actions check করুন

Browser-এ আপনার repository খুলুন:

**GitHub → `mdakashdev/frontend-project` → `Actions`**

তারপর **CI** workflow দেখতে পাবেন।

এখন **CI**-তে click করুন।

**বাংলা:** GitHub এখন আমাদের workflow অনুযায়ী automatically এই কাজগুলো করবে:

1. Code checkout
2. Node.js setup
3. pnpm setup
4. Dependencies install
5. Type check
6. Frontend build

👉 **CI-এর পাশে কী status দেখাচ্ছে** (`In progress`, `Success`, নাকি `Failed`) সেটা আমাকে বলুন।


ঠিক আছে 👍 এটা CI শেখার জন্যই ভালো—এখন আমরা **কেন failure হয়েছে সেটা বের করব**।

### Step 5 — Failure-এর error দেখুন

GitHub-এ:

**Actions → CI → যে failed run হয়েছে → `test` job-এ click করুন**

তারপর নিচে steps দেখতে পাবেন:

* Checkout code
* Setup Node.js
* Setup pnpm
* Install frontend dependencies
* Type check frontend
* Build frontend

যে step-এ **লাল ❌** আছে, সেটাতে click করুন।

👉 ওই step-এর **শেষের error/log কয়েক লাইন এখানে paste করুন**।

**বাংলা:** পুরো log দরকার নেই। শুধু ❌ failed step-এর শেষের error অংশ দিন। আমি সেটা দেখে পরের **একটা step** দেব।


ঠিক আছে, error পরিষ্কার। 👍

CI আপনার **local Mac-এর আগের code** check করছে, যেখানে এই line এখনো ঠিক করা হয়নি:

```ts
formData.append('position', employee.position)
```

`employee.position` এর type হচ্ছে:

```ts
string | undefined
```

কিন্তু `FormData.append()` এখানে `string` চায়।

### Step 6 — Local code fix করুন

আপনার local project-এ এই file খুলুন:

```text
emp-management/src/stores/employee.ts
```

এই line:

```ts
formData.append('position', employee.position)
```

এটা পরিবর্তন করে করুন:

```ts
formData.append('position', employee.position ?? '')
```

**বাংলা:** `position` না থাকলে (`undefined`) empty string `''` পাঠাবে। তাই TypeScript error হবে না।

তারপর শুধু আমাকে বলুন **done**।


Excellent! 🎉 **CI successfully passed.**

এখন পর্যন্ত আমাদের CI flow:

```text
Local Code
   ↓
git push main
   ↓
GitHub
   ↓
GitHub Actions
   ↓
✅ Install dependencies
   ↓
✅ Type check
   ↓
✅ Build
```

### Next: CD — Automatic Deployment 🚀

এখন আমরা CI-এর পরের অংশ শিখব:

```text
GitHub main
     ↓
GitHub Actions
     ↓
SSH
     ↓
Existing EC2
     ↓
Deploy Vue + Laravel
     ↓
Nginx reload
```

আমরা **নতুন EC2 বানাব না** — আপনার existing `my-first-project-server` ব্যবহার করব।

**পরের একটাই step:** GitHub Actions থেকে EC2-তে SSH করার জন্য **GitHub Secrets** তৈরি করা।

আপনি ready হলে শুধু **`next`** বলুন।


Perfect 👍

### Step 10 — SSH Private Key Secret

এবার সবচেয়ে গুরুত্বপূর্ণ secret: **EC2 SSH private key**।

আপনার Mac-এ terminal খুলে আগে key file আছে কি না দেখুন:

```bash
ls ~/aws/
```

আপনার key file হওয়া উচিত:

```text
my-project-key.pem
```

এখন **এখনই GitHub-এ কিছু paste করবেন না।**

শুধু এই command চালান:

```bash
cat ~/aws/my-project-key.pem
```

এতে পুরো private key terminal-এ দেখাবে।

⚠️ **এই key আমাকে পাঠাবেন না।**
শুধু নিজের terminal-এ দেখুন। এরপর আমরা এটাকে GitHub Secret `EC2_SSH_KEY` হিসেবে safely add করব।

Output না পাঠিয়ে শুধু **done** বলুন।



-----END RSA PRIVATE KEY-----%                                                  
sosofsoftsssosofsofsoftzsoftsoftsosossssssossssosofsoftzisoftzino@Msoftzino@Mdsoftzino@Mdssoftzino@Mdsoftzisoftsososssosof
sofsososossosossssssssoftzino@Mds-MacBook-Pro aws % ;2C



Great 👍 এখন key-টা GitHub Secret হিসেবে add করি।

### Step 11 — `EC2_SSH_KEY` Secret

যান:

**GitHub → `frontend-project` → Settings → Secrets and variables → Actions → New repository secret**

**Name:**

```text id="z7q7p6"
EC2_SSH_KEY
```

**Secret:**
আপনার terminal-এ `cat ~/aws/my-project-key.pem` দিয়ে যে **পুরো private key** দেখেছেন, সেটি **পুরোটা copy-paste** করুন।

এটা সাধারণত এমন হবে:

```text
-----BEGIN OPENSSH PRIVATE KEY-----
...
...
-----END OPENSSH PRIVATE KEY-----
```

তারপর **Add secret** চাপুন।

⚠️ **Private key এখানে chat-এ পাঠাবেন না।**

Secret তৈরি হলে **done** বলুন।



হ্যাঁ, আপনার output দেখে মনে হচ্ছে **private key-এর পরে terminal prompt/অন্য text মিশে গেছে**। তাই copy করার সময় সাবধান হতে হবে।

### Step 11A — File-এর শুরু/শেষ check করি

**Private key-এর content আমাকে paste করবেন না।** শুধু এই command চালান:

```bash id="u2b3y8"
head -n 1 ~/aws/my-project-key.pem
tail -n 1 ~/aws/my-project-key.pem
```

Expected:

```text id="0q1d4m"
-----BEGIN RSA PRIVATE KEY-----
-----END RSA PRIVATE KEY-----
```

অথবা নতুন OpenSSH format হলে:

```text id="5b4z1x"
-----BEGIN OPENSSH PRIVATE KEY-----
-----END OPENSSH PRIVATE KEY-----
```

**গুরুত্বপূর্ণ:** `tail`-এর output-এর পরে আপনার Mac prompt দেখা যাবে না—কারণ সেটা file-এর অংশ নয়।

শুধু এই দুই command-এর **output** আমাকে দিন। এতে private key-এর ভিতরের কোনো sensitive content প্রকাশ হবে না।


ঠিক আছে 👍 এখানে শুধু path-এ ছোট ভুল হয়েছে।

আপনি লিখেছেন:

```bash
~my-project-key.pem
```

কিন্তু file ছিল `aws` folder-এর ভিতরে। সঠিক path:

```bash
~/aws/my-project-key.pem
```

### Step 11B — আবার চালান

```bash
head -n 1 ~/aws/my-project-key.pem
tail -n 1 ~/aws/my-project-key.pem
```

Expected হবে এরকম:

```text
-----BEGIN RSA PRIVATE KEY-----
-----END RSA PRIVATE KEY-----
```

অথবা:

```text
-----BEGIN OPENSSH PRIVATE KEY-----
-----END OPENSSH PRIVATE KEY-----
```

👉 শুধু এই দুই লাইনের output দিন। Private key-এর মাঝের কোনো অংশ দেবেন না।


হ্যাঁ, আপনি যদি এখন `aws` folder-এর ভিতরেই থাকেন, তাহলে `~/aws/` দেওয়ার দরকার নেই। 👍

Terminal prompt যদি এমন হয়:

```text
softzino@Mds-MacBook-Pro aws %
```

তাহলে শুধু চালান:

```bash id="f1x0g4"
head -n 1 my-project-key.pem
tail -n 1 my-project-key.pem
```

**বাংলা:** এখানে `my-project-key.pem` সরাসরি current `aws` folder থেকে নেওয়া হবে।

Output-এর শুধু ওই **দুই লাইন** আমাকে দিন।


Perfect ✅ এখন নিশ্চিত হলাম আপনার `.pem` file ঠিক আছে।

```text
-----BEGIN RSA PRIVATE KEY-----
...
-----END RSA PRIVATE KEY-----
```

শেষের `%` এবং `softzino@Mds-MacBook-Pro aws %` **file-এর অংশ নয়**। এগুলো শুধু terminal prompt।

### Step 11C — পুরো key safely copy করুন

যেহেতু আপনার key RSA format, macOS-এ পুরো file clipboard-এ নেওয়ার সবচেয়ে নিরাপদ উপায়:

```bash
pbcopy < my-project-key.pem
```

এখন আপনার **পুরো private key clipboard-এ copy হয়েছে**।

GitHub-এ যান:

**Settings → Secrets and variables → Actions → New repository secret**

দিয়ে:

**Name**

```text
EC2_SSH_KEY
```

**Secret:** `⌘ + V`

তারপর **Add secret**।

⚠️ এখানে chat-এ key paste করবেন না।

Secret add হয়ে গেলে **done** বলুন।


Excellent ✅ এখন আমাদের ৩টা GitHub Secret আছে:

```text
EC2_HOST
EC2_USER
EC2_SSH_KEY
```

### Step 12 — CD workflow তৈরি করি

এখন local project-এর **repository root**-এ যেতে হবে।

আপনি যদি এখনও `aws` folder-এ থাকেন, আগে:

```bash
cd /Users/softzino/Documents/ART4/chat-widget-project/frontend-project
```

তারপর check করুন:

```bash
pwd
```

Expected:

```text
/Users/softzino/Documents/ART4/chat-widget-project/frontend-project
```

**বাংলা:** আমরা এবার repository root থেকে `.github/workflows/cd.yml` তৈরি করব।

শুধু `pwd` চালিয়ে output দিন।


Perfect ✅ আপনি repository root-এ আছেন।

### Step 13 — CD workflow file তৈরি

এখন চালান:

```bash id="c6r8vp"
touch .github/workflows/cd.yml
```

তারপর:

```bash id="n4y2qs"
open -e .github/workflows/cd.yml
```

এতে `cd.yml` TextEdit-এ খুলবে।

**বাংলা:** এই file-টাই পরে GitHub Actions-কে বলবে—`main`-এ code আসলে existing EC2-তে গিয়ে deployment করতে।

File খুললে **এখন কিছু লিখবেন না**। শুধু **done** বলুন।



Perfect 👍 এখন `cd.yml` খোলা আছে।

### Step 14 — CD workflow লিখুন

`cd.yml`-এর মধ্যে **এই পুরো code paste করুন**:

```yaml
name: CD

on:
  push:
    branches:
      - main

jobs:
  deploy:
    runs-on: ubuntu-latest

    steps:
      - name: Deploy to EC2
        uses: appleboy/ssh-action@v1.2.2
        with:
          host: ${{ secrets.EC2_HOST }}
          username: ${{ secrets.EC2_USER }}
          key: ${{ secrets.EC2_SSH_KEY }}
          script: |
            cd /var/www/frontend-project

            git fetch origin
            git reset --hard origin/main

            cd emp-management-api
            composer install --no-dev --optimize-autoloader
            php artisan migrate --force
            php artisan config:cache

            cd ../emp-management
            pnpm install --frozen-lockfile
            pnpm build

            sudo systemctl reload nginx
```

### এই workflow কী করবে?

```text
push main
   ↓
GitHub Actions
   ↓
SSH → EC2
   ↓
git reset → latest main
   ↓
Laravel deploy
   ↓
Vue install + build
   ↓
Nginx reload
```

**বাংলা:** আপাতত আমরা simple CD রাখছি। পরে এটাকে আরও production-style করব—যেমন backup, health check, rollback ইত্যাদি।

এখন code paste করে **Save** করুন।

তারপর শুধু **done** বলুন।



Great 👍

### Step 17 — CD workflow GitHub-এ push করুন

এখন চালান:

```bash id="q5m2x8"
git push origin main
```

এবার push হওয়ার সঙ্গে সঙ্গে **GitHub Actions → CD** workflow automatically trigger হবে। 🚀

**বাংলা:** এবার প্রথমবার আমাদের GitHub Actions চেষ্টা করবে SSH দিয়ে existing EC2-তে ঢুকে deployment করতে।

Push শেষ হলে **Actions → CD** গিয়ে status দেখুন।

👉 `Success` নাকি `Failed`—শুধু সেটা বলুন।


ঠিক আছে — এটা **CD workflow-এর code error না**। GitHub Actions runner আপনার EC2-এর SSH port `22`-এ পৌঁছাতে পারছে না।

Error:

```text
dial tcp ***:22: i/o timeout
```

### কেন হচ্ছে?

আপনার EC2 Security Group-এ SSH `22` বর্তমানে সম্ভবত **শুধু আপনার Mac-এর public IP** থেকে allowed।

কিন্তু GitHub Actions অন্য একটি server/runner থেকে SSH করছে:

```text
GitHub Actions runner
       ↓
      SSH :22
       ↓
     EC2
```

তাই আপনার Mac থেকে SSH কাজ করলেও GitHub Actions থেকে timeout হচ্ছে।

---

### Step 18 — EC2 Security Group খুলুন

AWS Console → **EC2 → Instances**

তারপর:

**`my-first-project-server` → Security tab → Security groups**

আপনার security group:

```text
launch-wizard-1
```

এটাতে click করুন।

তারপর **Inbound rules → Edit inbound rules** এ যান।

⚠️ **এখন কোনো rule change করবেন না।**

শুধু সেখানে বর্তমানে **SSH (22)** rule-টা কী আছে সেটা দেখুন এবং আমাকে বলুন।

বিশেষ করে `Source` কী দেখাচ্ছে, যেমন:

```text
My IP
59.xxx.xxx.xxx/32
```

বা

```text
0.0.0.0/0
```

তারপর আমাকে সেটা বলুন।


ঠিক আছে, এখন সমস্যাটা পরিষ্কার। 👍

আপনার SSH rules:

* `59.153.28.86/32` → আপনার বর্তমান IP
* `103.230.106.3/32` → temporary IP
* EC2 Instance Connect prefix list

কিন্তু **GitHub Actions runner-এর fixed public IP নেই** যেটা আমরা এখানে সহজে allowlist করতে পারি। তাই runner-এর SSH connection timeout হচ্ছে।

### Step 18 — আপাতত শেখার জন্য SSH allow করুন

আমরা প্রথমে CD কাজ করছে কিনা verify করব। পরে এটাকে আরও secure approach-এ নিয়ে যাব।

AWS:

**EC2 → Security Groups → launch-wizard-1 → Inbound rules → Edit inbound rules → Add rule**

দিয়ে:

```text
Type:        SSH
Protocol:    TCP
Port:        22
Source:      Anywhere-IPv4
CIDR:        0.0.0.0/0
Description: GitHub Actions CI/CD
```

তারপর **Save rules**।

⚠️ **Security note:** `0.0.0.0/0` মানে internet-এর যেকোনো IP থেকে port 22 পৌঁছানো যাবে। Production-এ এভাবে রাখা ভালো practice নয়। আমরা CD সফলভাবে শেখার পর SSH access-এর জন্য আরও secure setup করব।

**এখন শুধু এই rule add করে Save করুন। তারপর `done` বলুন।**


Good 👍 এখন আবার **CD workflow manually re-run** করি। নতুন commit করার দরকার নেই।

### Step 19 — GitHub Actions → CD → Re-run

GitHub repository-তে যান:

**Actions → CD → failed run**

তারপর:

**Re-run jobs → Re-run all jobs**

এবার GitHub Actions আবার EC2-তে SSH করার চেষ্টা করবে।

**Expected:** এবার `dial tcp ...:22: i/o timeout` error আর থাকার কথা নয়।

👉 Re-run শেষ হলে শুধু বলুন **Success** অথবা **Failed**।


Good 👍 এখন আবার **CD workflow manually re-run** করি। নতুন commit করার দরকার নেই।

### Step 19 — GitHub Actions → CD → Re-run

GitHub repository-তে যান:

**Actions → CD → failed run**

তারপর:

**Re-run jobs → Re-run all jobs**

এবার GitHub Actions আবার EC2-তে SSH করার চেষ্টা করবে।

**Expected:** এবার `dial tcp ...:22: i/o timeout` error আর থাকার কথা নয়।

👉 Re-run শেষ হলে শুধু বলুন **Success** অথবা **Failed**।


Excellent! 🎉 **আপনার প্রথম automated CD সফল হয়েছে।**

এখন আপনার complete CI/CD flow কাজ করছে:

```text
Developer
   ↓
git push main
   ↓
GitHub
   ↓
┌───────────────┐
│ CI            │
│ ✅ Install    │
│ ✅ Type check │
│ ✅ Build      │
└───────┬───────┘
        ↓
┌───────────────┐
│ CD            │
│ ✅ SSH → EC2  │
│ ✅ Git update │
│ ✅ Laravel    │
│ ✅ Vue build  │
│ ✅ Nginx      │
└───────┬───────┘
        ↓
      EC2 🚀
```

### পরের Step — একটা গুরুত্বপূর্ণ বিষয়

আমাদের CD এখন **`main`-এ push হলেই deploy করে**। কিন্তু production-এ সাধারণত flow হয়:

```text
feature branch
      ↓
Pull Request
      ↓
CI
      ↓
merge → main
      ↓
CD
      ↓
Production EC2
```

আমরা পরের ধাপে **PR-based CI/CD flow** শিখব এবং তারপর deployment-টাকে আরও production-style করব।

**এখন আপনার কাজ শেষ।** ✅


হ্যাঁ 👍 এটা এখন test করার জন্য একদম ভালো। আমরা **একটা ছোট text change → push → CI → CD → browser** পুরো flow দেখব।

### Step 20 — একটা text change করুন

Local project-এ:

```text
emp-management/src/
```

এর মধ্যে আপনার কোনো সহজ UI text খুঁজে পরিবর্তন করুন।

উদাহরণ:

```text
Employee Management
```

থাকলে পরিবর্তন করে:

```text
Employee Management - CI/CD Test
```

করুন।

তারপর **save** করুন।

⚠️ এখনো `git add` / `commit` / `push` করবেন না।

Change save হয়ে গেলে শুধু **done** বলুন।


Great 👍

### Step 23 — `main`-এ push করুন 🚀

```bash id="u7k3pa"
git push origin main
```

এবার automatically:

```text
git push
   ↓
CI ✅
   ↓
CD ✅
   ↓
EC2 update
   ↓
Vue build
   ↓
Nginx reload
```

হবে।

Push শেষ হলে **GitHub → Actions** গিয়ে দেখুন **CI** এবং **CD** দুটোই `Success` হয়েছে কিনা।

তারপর browser refresh করুন:

```text
http://56.10.120.85
```

👉 Text change হয়েছে কিনা দেখুন।

**শেষে আমাকে বলবেন: `changed` অথবা `not changed`।**
