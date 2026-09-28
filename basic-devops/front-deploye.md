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


সমস্যাটা dependency install-এর মাঝপথে incomplete হওয়ার কারণে হয়েছে।

`run-p` command আসে **`npm-run-all2`** package থেকে। আপনার `package.json`-এ এটা আছে:

```text
"npm-run-all2": "^9.0.2"
```

কিন্তু `node_modules/.bin/run-p` নেই।

### Step 9 — `run-p` আছে কি না check করি

EC2-তে চালান:

```bash
ls -l node_modules/.bin/run-p
```

যদি দেখায়:

```text
No such file or directory
```

তাহলে আমাকে output দিন। এরপর আমরা dependency installation ঠিকভাবে complete করব।

**এখন `pnpm build` আবার চালাবেন না।**


ঠিক আছে, এখন কারণটা নিশ্চিত। `npm-run-all2` package-এর binary `run-p` install হয়নি।

### Step 10 — শুধু missing package install করি

EC2-তে চালান:

```bash id="f8r3sk"
pnpm add -D npm-run-all2@9.0.2
```

এটা `npm-run-all2` install করবে এবং `node_modules/.bin/run-p` তৈরি করবে।

**গুরুত্বপূর্ণ:** এটা `package.json` এবং `pnpm-lock.yaml`-এ change করতে পারে। আপাতত ঠিক আছে—পরের ধাপে আমরা Git workflow নিয়ে আলাদা করে দেখব।

Command শেষ হলে চালান:

```bash id="1s8j2k"
ls -l node_modules/.bin/run-p
```

Output দিন।


ভালো ✅ `run-p` এখন install হয়েছে ধরে নিচ্ছি।

### Step 11 — আবার frontend build করুন

EC2-তে চালান:

```bash
pnpm build
```

এবার `type-check` এবং `vite build` দুটোই চলবে।

শেষে যদি successful হয়, output-এর শেষ অংশটা আমাকে দিন।


ঠিক আছে। এবার **build process চলছে**, কিন্তু `vite build` এবং `vue-tsc`—দুটোর কোনোটা শেষ হচ্ছে না।

যেহেতু `t3.micro`-তে RAM মাত্র **1 GB**, এই ধরনের TypeScript/Vite build-এ memory pressure হওয়ার সম্ভাবনা আছে।

আগে কিছু kill/change না করে আমরা দেখব server-এর memory অবস্থা কী।

### Step 12 — নতুন EC2 terminal খুলুন

বর্তমান terminal **খোলা রাখুন**।

নতুন SSH session খুলে চালান:

```bash
free -h
```

তারপর:

```bash
ps aux --sort=-%mem | head -10
```

এই দুই command-এর output দিন।

**বাংলা:** এতে আমরা দেখব build-এর সময় EC2-এর RAM কতটা ব্যবহার হচ্ছে এবং কোন process বেশি memory নিচ্ছে। তারপর সেই অনুযায়ী পরের step দেব।


হ্যাঁ, এখানে সমস্যাটা বেশ পরিষ্কার হচ্ছে।

আপনার `t3.micro`-তে **MySQL একাই প্রায় 396 MB RAM** ব্যবহার করছে:

```text
mysql    ...    42.5%    396024 KB
```

আর `t3.micro`-তে মোট RAM প্রায় **1 GB**। Frontend-এর `vue-tsc` + `vite build` একসাথে চালানোর সময় আরও অনেক memory দরকার হতে পারে।

তবে আগে আমরা নিশ্চিত হই **বর্তমানে কত RAM available আছে**।

### Step 13 — এই command চালান

নতুন EC2 terminal-এ:

```bash
free -h
```

শুধু এর output দিন।

তারপর আমরা ঠিক করব build কীভাবে safely complete করা যায়।


Exactly — এখন কারণটা clear। 👍

আপনার server:

```text
Total RAM:       909 MiB
Used:            684 MiB
Available:       225 MiB
```

Frontend build-এর `vue-tsc`/Vite-এর জন্য **225 MiB available RAM খুব কম**। তাই build আটকে থাকার সম্ভাবনা অনেক বেশি।

আমরা এখন **swap memory** যোগ করব। এটা EC2-এর disk-এর কিছু অংশকে temporary memory হিসেবে ব্যবহার করবে। `t3.micro`-এর মতো ছোট server-এ build করার জন্য এটা খুব useful।

### Step 14 — আগে swap আছে কি না check করুন

নতুন terminal-এ শুধু চালান:

```bash id="q1n8xz"
swapon --show
```

যদি কিছু output না আসে, তাহলে বর্তমানে swap নেই।

**Output দিন।** এরপর আমি 2GB swap তৈরি করার একদম step-by-step command দেব।


ঠিক আছে ✅ বর্তমানে **কোনো swap নেই**।

এখন 2GB swap তৈরি করব। এটা আপনার 1GB RAM-এর replacement না, কিন্তু build-এর সময় memory shortage সামলাতে সাহায্য করবে।

### Step 15 — 2GB Swap তৈরি করুন

EC2 terminal-এ চালান:

```bash
sudo fallocate -l 2G /swapfile
```

তারপর:

```bash
sudo chmod 600 /swapfile
```

তারপর:

```bash
sudo mkswap /swapfile
```

তারপর:

```bash
sudo swapon /swapfile
```

শেষে verify:

```bash
free -h
```

`Swap` লাইনে প্রায় **2.0Gi** দেখানোর কথা।

👉 সবগুলো command একসাথে চালাতে পারেন, অথবা একটার পর একটা। শেষে `free -h` এর output আমাকে দিন।



Perfect ✅ **2GB Swap successfully enabled.**

এখন build-এর সময় RAM শেষ হয়ে গেলে server swap ব্যবহার করতে পারবে।

### Step 16 — আবার build চালান

আগের build process যদি এখনও অন্য terminal-এ আটকে থাকে, **ওই terminal-এ `Ctrl + C` দিয়ে stop করার চেষ্টা করুন**।

তারপর frontend directory-তে:

```bash
cd /var/www/frontend-project/emp-management
```

তারপর:

```bash
pnpm build
```

এবার build একটু সময় নিতে পারে—বিশেষ করে `vue-tsc` চলার সময়।

**এখন শুধু build-এর output দেখুন। সফল হলে শেষের output আমাকে দিন।**


এবার আগের চেয়ে অনেক ভালো অবস্থায় আছে। ✅

এই লাইনগুলো গুরুত্বপূর্ণ:

```text
✓ 4623 modules transformed.
```

মানে **Vite successfully 4623 modules process করেছে**। এখন সম্ভবত final bundling/minification হচ্ছে। `t3.micro` + 1GB RAM হওয়ায় এখানে কিছুটা সময় লাগতে পারে।

### এখন কিছু করবেন না

* `Ctrl + C` দেবেন না
* নতুন `pnpm build` চালাবেন না
* একটু অপেক্ষা করুন

যদি সফল হয়, শেষে এরকম কিছু আসবে:

```text
✓ built in ...s
```

এবং `dist/` folder তৈরি হবে।

**আরও কিছুক্ষণ অপেক্ষা করুন। Prompt ফিরে এলে আমাকে শেষের output দিন।**




এখন আর server/memory সমস্যা নেই। ✅ **Build আসলে TypeScript error-এর কারণে fail করেছে।**

৪টা error এসেছে:

1. `DepartmentChart.vue` — `chart.type`
2. `EmployeeChart.vue` — `chart.type`
3. `stores/employee.ts` — `employee.position` possibly `undefined`
4. `views/Employee/Create.vue` — `setFieldError()` type mismatch

আমরা একবারে **একটা error** ঠিক করব।

### Step 17 — প্রথম error-এর code দেখুন

EC2-তে চালান:

```bash
sed -n '65,85p' src/components/DepartmentChart.vue
```

এটার output আমাকে দিন।

**বাংলা:** আগে `DepartmentChart.vue`-এর `chartOptions` অংশটা দেখি। তারপর exact code অনুযায়ী fix করব—অনুমান করে code change করব না।



এখানে `template` ঠিক আছে। Error হচ্ছে `chartOptions` object-এর `chart.type` TypeScript-এর কাছে `string` হিসেবে infer হচ্ছে, কিন্তু ApexCharts নির্দিষ্ট chart type চায়।

এখন exact `chartOptions` declaration দেখি।

### Step 18 — এই command চালান

```bash id="x7y2kp"
sed -n '1,70p' src/components/DepartmentChart.vue
```

Output দিন। তারপর আমরা শুধু এই error-টাই fix করব।



ঠিক আছে। এখন exact issue দেখা যাচ্ছে।

`chartOptions`-এ:

```ts
const chartOptions = {
```

TypeScript এটাকে খুব broad type হিসেবে infer করছে, ফলে:

```ts
type: "donut"
```

কে `"donut"` না ধরে `string` হিসেবে ধরছে। ApexCharts কিন্তু নির্দিষ্ট type চায়।

### Step 19 — `ApexOptions` type ব্যবহার করি

`DepartmentChart.vue`-এর প্রথম লাইনের import পরিবর্তন করুন।

বর্তমানে:

```ts
import VueApexCharts from "vue3-apexcharts";
```

এটা করুন:

```ts
import VueApexCharts from "vue3-apexcharts";
import type { ApexOptions } from "apexcharts";
```

তারপর:

```ts
const chartOptions = {
```

পরিবর্তন করে:

```ts
const chartOptions: ApexOptions = {
```

অর্থাৎ শুরুটা হবে:

```ts
<script setup lang="ts">
import VueApexCharts from "vue3-apexcharts";
import type { ApexOptions } from "apexcharts";

const series = [34, 18, 15, 12, 10, 11];

const chartOptions: ApexOptions = {
```

### কেন?

**English:** `ApexOptions` explicitly tells TypeScript that this object must follow ApexCharts' configuration types.

**বাংলা:** TypeScript তখন বুঝবে `chart.type` শুধু সাধারণ `string` না; এটা ApexCharts-এর valid chart type, যেমন `"donut"`।

এখন **এই একটা file-এ change করুন**। তারপর আমাকে `done` বলুন।



অবশ্যই। EC2 terminal থেকে আমরা `nano` দিয়ে file edit করব।

### Step 19A — File খুলুন

চালান:

```bash
nano src/components/DepartmentChart.vue
```

তারপর keyboard দিয়ে উপরের অংশে যান।

বর্তমানে আছে:

```ts
import VueApexCharts from "vue3-apexcharts";
```

এর **পরের লাইনে** লিখুন:

```ts
import type { ApexOptions } from "apexcharts";
```

তারপর এই line:

```ts
const chartOptions = {
```

পরিবর্তন করে:

```ts
const chartOptions: ApexOptions = {
```

তাহলে শুরুটা এমন হবে:

```ts
<script setup lang="ts">
import VueApexCharts from "vue3-apexcharts";
import type { ApexOptions } from "apexcharts";

const series = [34, 18, 15, 12, 10, 11];

const chartOptions: ApexOptions = {
```

### Save করার নিয়ম

`nano`-তে:

1. **Ctrl + O** → save
2. **Enter** → filename confirm
3. **Ctrl + X** → বের হয়ে আসুন

তারপর আমাকে শুধু `done` বলুন।


ভালো ✅ প্রথম error-এর fix করা হয়েছে।

### Step 20 — এখন আবার build check করি

চালান:

```bash
pnpm build
```

এবার পুরো build শেষ হওয়ার আগেই যদি error আসে, **শেষের error অংশটা পাঠাবেন**।

বিশেষ করে আমরা দেখব `DepartmentChart.vue` error চলে গেছে কি না।


Excellent ✅ প্রথম `DepartmentChart.vue` error চলে গেছে। এখন **৩টা error বাকি**।

আমরা আগের মতো একটার পর একটা করব।

### Step 21 — `EmployeeChart.vue` দেখি

চালান:

```bash id="x8k2qv"
sed -n '1,100p' src/components/EmployeeChart.vue
```

Output দিন।

**বাংলা:** প্রথম chart-এর মতোই এখানে সম্ভবত `ApexOptions` type যোগ করলেই হবে, কিন্তু আগে পুরো relevant code দেখে তারপর exact change করব।


হ্যাঁ, এটাও আগেরটার একই ধরনের TypeScript issue। 👍

এখানেও `chartOptions`-কে `ApexOptions` type দিতে হবে।

### Step 22 — `EmployeeChart.vue` edit করুন

চালান:

```bash id="y9b3tq"
nano src/components/EmployeeChart.vue
```

উপরে:

বর্তমানে:

```ts
import VueApexCharts from "vue3-apexcharts";
```

এর নিচে যোগ করুন:

```ts
import type { ApexOptions } from "apexcharts";
```

তারপর:

```ts
const chartOptions = {
```

পরিবর্তন করে:

```ts
const chartOptions: ApexOptions = {
```

শুরুটা এমন হবে:

```ts
<script setup lang="ts">
import VueApexCharts from "vue3-apexcharts";
import type { ApexOptions } from "apexcharts";

const series = [
  {
    name: "Employees",
    data: [8500, 15500, 14200, 21000, 18500, 28000, 32500],
  },
];

const chartOptions: ApexOptions = {
```

Save:

**Ctrl + O → Enter → Ctrl + X**

তারপর আমাকে `done` বলুন।


then . pnpm build



দারুণ ✅ এখন **Vite build সফল হয়েছে**। শুধু TypeScript `type-check`-এ ২টা error বাকি।

এখন প্রথমটা ঠিক করি:

```text
src/stores/employee.ts:13
employee.position → string | undefined
```

### Step 23 — `employee.ts`-এর code দেখুন

চালান:

```bash
sed -n '1,30p' src/stores/employee.ts
```

Output দিন।

**বাংলা:** আগে দেখি `employee.position` কোথা থেকে আসছে। তারপর সবচেয়ে safe fix করব।


এখানে error-এর কারণ দেখা যাচ্ছে:

```ts
formData.append('position', employee.position)
```

TypeScript বলছে `employee.position` **`string | undefined`**, কিন্তু `FormData.append()` এখানে নিশ্চিত `string` চায়।

এটা ঠিক করার আগে `Employee` type-টা দেখি, কারণ `position` কেন optional (`?`) সেটা জানা দরকার।

### Step 24 — `Employee` type দেখুন

চালান:

```bash
grep -n -A20 -B5 "interface Employee\|type Employee" src/data/employees.ts
```

Output দিন।



এখন কারণটা clear। `Employee` type-এ:

```ts
position?: string
```

মানে `position` **optional**, তাই TypeScript ধরে নিচ্ছে এটা `undefined` হতে পারে।

আমাদের এই function-এ API-তে অবশ্যই string পাঠাতে হবে। Safe fix হিসেবে `undefined` হলে empty string পাঠাতে পারি।

### Step 25 — শুধু এই line পরিবর্তন করুন

ফাইল খুলুন:

```bash
nano src/stores/employee.ts
```

এই line:

```ts
formData.append('position', employee.position)
```

পরিবর্তন করে:

```ts
formData.append('position', employee.position ?? '')
```

### কেন `?? ''`?

**English:** If `employee.position` has a value, it uses that value. If it is `undefined` or `null`, it uses an empty string.

**বাংলা:** `position` থাকলে সেটাই যাবে। না থাকলে `''` (empty string) যাবে। তাই `FormData.append()` আর `undefined` পাবে না।

Save:

**Ctrl + O → Enter → Ctrl + X**

তারপর `done` বলুন।


ভালো ✅

এখন **শেষ TypeScript error** দেখব।

### Step 26 — `Create.vue`-এর relevant code দেখুন

চালান:

```bash
sed -n '45,70p' src/views/Employee/Create.vue
```

Output দিন।


ঠিক আছে। শেষ error-এর কারণ হলো:

```ts
const formField = fieldMap[field] ?? field
```

এখানে `field` যেকোনো `string` হতে পারে। কিন্তু `setFieldError()` নির্দিষ্ট form field name গ্রহণ করছে।

আমরা **fieldMap-এ থাকা known fields-গুলোকে type-safe** করব। তবে আগে `setFieldError` কোন form type থেকে আসছে সেটা দেখা দরকার।

### Step 27 — উপরের অংশটা দেখুন

চালান:

```bash id="j2m8xk"
sed -n '1,45p' src/views/Employee/Create.vue
```

Output দিন।


এখন বুঝতে পারছি। `setFieldError` এসেছে `useForm()` থেকে। আমাদের `useForm` declaration-টা দেখতে হবে, কারণ সেখানেই form fields-এর exact type তৈরি হচ্ছে।

### Step 28 — `useForm` অংশটা খুঁজুন

চালান:

```bash id="8r5q4p"
grep -n -A20 -B5 "useForm" src/views/Employee/Create.vue
```

Output দিন।




ঠিক আছে, এখন exact issue বোঝা যাচ্ছে। `setFieldError()` schema থেকে field names-এর union নিচ্ছে:

```text
"position" | "email" | "department" | "phone" | "status" |
"fullName" | "joinDate..." | "profilePhoto..."
```

কিন্তু `field` হলো সাধারণ `string`।

আমরা backend থেকে আসা error field-কে সরাসরি `setFieldError()`-এ না দিয়ে আগে validate/cast করব।

### Step 29 — `onError`-এর এই অংশ পরিবর্তন করুন

ফাইল খুলুন:

```bash id="g9e5hm"
nano src/views/Employee/Create.vue
```

এই অংশটি খুঁজুন:

```ts id="fgyx4m"
if (errors) {
  Object.entries(errors).forEach(([field, messages]) => {
    const formField = fieldMap[field] ?? field
    setFieldError(formField, messages[0])
  })
  return
}
```

এটা **পুরোপুরি replace** করুন:

```ts id="d5m0vz"
if (errors) {
  Object.entries(errors).forEach(([field, messages]) => {
    const formField = fieldMap[field] ?? field

    if (
      ['fullName', 'email', 'position', 'department', 'phone', 'status', 'joinDate', 'profilePhoto'].includes(formField)
    ) {
      setFieldError(formField, messages[0])
    }
  })
  return
}
```

**কী হচ্ছে এখানে?**

**English:** We check whether the backend field name matches one of our actual form fields before passing it to `setFieldError()`.

**বাংলা:** Backend থেকে `phone`, `email`, `department` ইত্যাদি field আসতে পারে। আমরা আগে check করছি field-টা আমাদের form-এর valid field কি না। Valid হলে তবেই `setFieldError()`-এ পাঠাব।

Save:

**Ctrl + O → Enter → Ctrl + X**

তারপর `done` বলুন।



ঠিক আছে, এখন exact issue বোঝা যাচ্ছে। `setFieldError()` schema থেকে field names-এর union নিচ্ছে:

```text
"position" | "email" | "department" | "phone" | "status" |
"fullName" | "joinDate..." | "profilePhoto..."
```

কিন্তু `field` হলো সাধারণ `string`।

আমরা backend থেকে আসা error field-কে সরাসরি `setFieldError()`-এ না দিয়ে আগে validate/cast করব।

### Step 29 — `onError`-এর এই অংশ পরিবর্তন করুন

ফাইল খুলুন:

```bash id="g9e5hm"
nano src/views/Employee/Create.vue
```

এই অংশটি খুঁজুন:

```ts id="fgyx4m"
if (errors) {
  Object.entries(errors).forEach(([field, messages]) => {
    const formField = fieldMap[field] ?? field
    setFieldError(formField, messages[0])
  })
  return
}
```

এটা **পুরোপুরি replace** করুন:

```ts id="d5m0vz"
if (errors) {
  Object.entries(errors).forEach(([field, messages]) => {
    const formField = fieldMap[field] ?? field

    if (
      ['fullName', 'email', 'position', 'department', 'phone', 'status', 'joinDate', 'profilePhoto'].includes(formField)
    ) {
      setFieldError(formField, messages[0])
    }
  })
  return
}
```

**কী হচ্ছে এখানে?**

**English:** We check whether the backend field name matches one of our actual form fields before passing it to `setFieldError()`.

**বাংলা:** Backend থেকে `phone`, `email`, `department` ইত্যাদি field আসতে পারে। আমরা আগে check করছি field-টা আমাদের form-এর valid field কি না। Valid হলে তবেই `setFieldError()`-এ পাঠাব।

Save:

**Ctrl + O → Enter → Ctrl + X**

তারপর `done` বলুন।



Good news: **Vite build itself succeeded**. এখন শুধু TypeScript-এর **একটা error** বাকি।

Error হচ্ছে:

```text
setFieldError(formField, messages[0])
```

কারণ `formField` এখনো সাধারণ `string`, কিন্তু `setFieldError()` নির্দিষ্ট field name চায়।

### Step 3 — এই অংশটা replace করো

`Create.vue` খুলে এই অংশটি:

```ts
if (errors) {
  Object.entries(errors).forEach(([field, messages]) => {
    const formField = fieldMap[field] ?? field

    if (
      ['fullName', 'email', 'position', 'department', 'phone', 'status', 'joinDate', 'profilePhoto'].includes(formField)
    ) {
      setFieldError(formField, messages[0])
    }
  })
  return
}
```

এর বদলে এটা দাও:

```ts
if (errors) {
  Object.entries(errors).forEach(([field, messages]) => {
    const formField = fieldMap[field] ?? field

    const validFields = [
      'fullName',
      'email',
      'position',
      'department',
      'phone',
      'status',
      'joinDate',
      'profilePhoto',
    ] as const

    if (validFields.includes(formField as typeof validFields[number])) {
      setFieldError(
        formField as typeof validFields[number],
        messages[0]
      )
    }
  })

  return
}
```

**কেন?**
`as const` TypeScript-কে বলে—এই field names-গুলোই valid form fields। তাই `setFieldError()` আর generic `string` নিয়ে complain করবে না।

এটা করার পর শুধু **`pnpm build` আবার চালাও**।



