node -v
Command 'node' not found, but can be installed with:
sudo apt install nodejs


ঠিক আছে। EC2-তে Node.js নেই। যেহেতু আপনার project **Node 22.18+** চায়, আমরা Ubuntu-এর পুরোনো default package না নিয়ে **Node.js 22 LTS** install করব।

### Step 3 — Node.js 22 install করুন

EC2-তে চালান:

```bash
curl -fsSL https://deb.nodesource.com/setup_22.x | sudo -E bash -
```

তারপর:

```bash
sudo apt install -y nodejs
```

শেষে version check:

```bash
node -v
npm -v
```

**Expected:**

```text
v22.x.x
10.x.x
```

**বাংলা:** প্রথম command Node.js 22-এর repository যোগ করবে, দ্বিতীয়টি Node.js install করবে। তারপর `node -v` দিয়ে version যাচাই করব।

শুধু এই step-এর output দিন, তারপর **pnpm install** করব।



Perfect ✅ Node.js **v22.23.3** আপনার project's requirement `^22.18.0` পূরণ করছে।

### Step 4 — pnpm install করুন

আপনার project-এ `pnpm-lock.yaml` আছে, তাই pnpm ব্যবহার করব।

EC2-তে চালান:

```bash
sudo npm install -g pnpm
```

তারপর verify করুন:

```bash
pnpm -v
```

**বাংলা:** `pnpm` globally install করছি যাতে project-এর dependency install এবং build করতে পারি।

Output দিন, তারপর আমরা **frontend dependencies install** করব।



Perfect ✅

* Node.js: `v22.23.3` ✅
* npm: `10.9.9` ✅
* pnpm: `12.6.0` ✅

### Step 6 — Frontend dependencies install

আপনি এখনো এই directory-তে থাকলে:

```bash
/var/www/frontend-project/emp-management
```

চালান:

```bash
pnpm install --frozen-lockfile
```

**বাংলা:** এটা `pnpm-lock.yaml` অনুযায়ী exact dependency versions install করবে। এতে `node_modules` তৈরি হবে।

শেষে যদি `Done` / successful installation দেখায়, output দিন। তারপর আমরা **frontend build (`pnpm build`)** করব।



ঠিক আছে—এখানে সমস্যা পরিষ্কার। **`pnpm-lock.yaml` পুরোনো format-এর (`lockfileVersion: 6.0`)**, কিন্তু আমাদের EC2-তে **pnpm 12.6.0** আছে, যা এই lockfile format support করছে না।

এখনই `pnpm install` দিয়ে lockfile পরিবর্তন করব না। আগে দেখি আপনার **local Mac-এ কোন pnpm version** দিয়ে এই project চলছে।

### Step 6A — Local Mac-এ pnpm version check

Mac-এ project-এর `emp-management` folder-এ গিয়ে চালান:

```bash
pnpm -v
```

**Output আমাকে দিন।**

**বাংলা:** আগে local environment-এর pnpm version জানতে হবে। তারপর ঠিক করব lockfile update করব, নাকি EC2-তে compatible pnpm version install করব।





এখন বিষয়টা clear ✅

আপনার local project চলছে:

```text
Node.js  v22.22.3
pnpm     8.14.1
```

আর EC2-তে আছে:

```text
Node.js  v22.23.3
pnpm     12.6.0
```

আপনার `pnpm-lock.yaml` **pnpm 8-এর lockfile**, তাই আমাদের EC2-তে **pnpm 8.14.1** ব্যবহার করাই সবচেয়ে safe। এতে lockfile পরিবর্তন হবে না।

### Step 6B — EC2-তে pnpm 8.14.1 install করুন

EC2-তে চালান:

```bash
sudo npm install -g pnpm@8.14.1
```

তারপর:

```bash
pnpm -v
```

Expected:

```text
8.14.1
```

এটা হলে আমাকে output দিন। এরপর আবার:

```bash
pnpm install --frozen-lockfile
```

করব।



ঠিক আছে। যদি `Ctrl+C`-তেও কিছু না হয়, তাহলে সম্ভবত terminal process পুরোপুরি hang করেছে।

### এখন Step 1 — নতুন EC2 terminal/session খুলুন

আপনার বর্তমান terminal **বন্ধ করবেন না**।

আপনি যেভাবে আগে EC2-তে SSH করেছিলেন, **আরেকটি নতুন terminal window/tab খুলে আবার EC2-তে SSH করুন**।

তারপর নতুন session-এ চালান:

```bash
ps aux | grep '[p]npm install'
```

এটার output আমাকে দিন।

**বাংলা:** আমরা নতুন session দিয়ে দেখব পুরোনো `pnpm install` process সত্যিই চলছে কি না। তারপর প্রয়োজন হলে সেটাকে safely stop করব।


এটা গুরুত্বপূর্ণ ✅

কোনো output আসেনি, অর্থাৎ **`pnpm install` process এখন আর চলছে না**। সম্ভবত আগের session-এর process terminate হয়ে গেছে, শুধু terminal prompt ফিরেনি।

এখন নতুন session-এ একই folder-এ আছেন:

```text
/var/www/frontend-project/emp-management
```

### Step 2 — Installation complete হয়েছে কি না check করি

চালান:

```bash
ls -ld node_modules
```

তারপর:

```bash
ls node_modules/.pnpm | wc -l
```

এই দুইটার output দিন। তারপর বুঝব dependency installation কতটুকু হয়েছে এবং আবার `pnpm install` চালানো দরকার কি না।


Excellent ✅ Dependency installation **complete হয়েছে**।

`378` packages আছে, এবং `node_modules` তৈরি হয়েছে। তাই আবার `pnpm install` করার দরকার নেই।

### Step 8 — এখন Frontend build করি

একই directory-তে থেকে চালান:

```bash
pnpm build
```

এটা আপনার `package.json`-এর:

```text
build → type-check + vite build
```

দুটো কাজ করবে এবং সফল হলে `dist/` folder তৈরি হবে।

**Expected শেষের দিকে কিছু এমন দেখবেন:**

```text
✓ built in ...
```

এবং:

```text
dist/
```

তৈরি হবে।

👉 এখন শুধু `pnpm build` চালান এবং **পুরো output দিন**।

