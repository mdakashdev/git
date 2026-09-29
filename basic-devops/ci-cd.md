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
