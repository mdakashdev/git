# Writing a good CLAUDE.md

https://www.humanlayer.dev/blog/writing-a-good-claude-md

**একটি ভালো CLAUDE.md কীভাবে লিখবেন**

**#agents**
**#claudecode**
**#best-practices**

আপনার কোডবেস সম্পর্কে মডেলটি শুধুমাত্র সেই তথ্যই জানে, যা আপনি টোকেন (tokens) আকারে তাকে প্রদান করেন।


**Claude Code**-এর মতো কোডিং এজেন্ট বা অন্যান্য এজেন্ট হারনেস (agent harnesses) সাধারণত এজেন্টের **মেমরি (memory)** আপনাকেই স্পষ্টভাবে পরিচালনা করতে হয়। 

**CLAUDE.md** হলো সেই ফাইল, যা ডিফল্টভাবে এজেন্টের সঙ্গে আপনার প্রতিটি নতুন কথোপকথনে অন্তর্ভুক্ত হয়।

এর তিনটি গুরুত্বপূর্ণ অর্থ রয়েছে:

* প্রতিটি নতুন সেশন শুরু হওয়ার সময় কোডিং এজেন্ট আপনার কোডবেস সম্পর্কে **কোনো তথ্যই জানে না**।
* আপনার কোডবেস সম্পর্কে যেসব তথ্য গুরুত্বপূর্ণ, **প্রতিবার নতুন সেশন শুরু করলে সেগুলো এজেন্টকে জানাতে হবে**।
* এই কাজটি করার জন্য **CLAUDE.md**-ই সবচেয়ে উপযুক্ত এবং সুপারিশকৃত উপায়।


# CLAUDE.md ক্লডকে (Claude) আপনার কোডবেসের সঙ্গে পরিচিত করায়

যেহেতু প্রতিটি নতুন সেশনের শুরুতে Claude আপনার কোডবেস সম্পর্কে কিছুই জানে না, তাই **CLAUDE.md** ব্যবহার করে তাকে আপনার কোডবেস সম্পর্কে প্রাথমিক ধারণা দিতে হবে।


সংক্ষেপে, এতে নিচের বিষয়গুলো থাকা উচিত:

### **WHAT (কী)**

Claude-কে আপনার ব্যবহৃত প্রযুক্তি (Tech Stack), প্রজেক্টের স্ট্রাকচার এবং কোডবেসের একটি সামগ্রিক মানচিত্র সম্পর্কে জানান। 

এটি বিশেষ করে **মনোরেপো (Monorepo)**-র ক্ষেত্রে খুবই গুরুত্বপূর্ণ। কোন অ্যাপ কী কাজ করে, কোন শেয়ার্ড প্যাকেজ রয়েছে, এবং রিপোজিটরির প্রতিটি অংশের উদ্দেশ্য কী—এসব উল্লেখ করুন, যাতে Claude সহজেই বুঝতে পারে কোথায় কী খুঁজতে হবে।

### **WHY (কেন)**

প্রজেক্টটির উদ্দেশ্য কী এবং রিপোজিটরির বিভিন্ন অংশ কী কাজ করে, তা ব্যাখ্যা করুন। অর্থাৎ, প্রজেক্টটি কেন তৈরি হয়েছে এবং প্রতিটি কম্পোনেন্ট বা মডিউলের ভূমিকা কী, সে সম্পর্কে ধারণা দিন।

### **HOW (কীভাবে)**

Claude-কে জানান কীভাবে এই প্রজেক্টে কাজ করতে হবে। উদাহরণস্বরূপ, আপনি যদি Node-এর পরিবর্তে **Bun** ব্যবহার করেন, তাহলে সেটি উল্লেখ করুন। এছাড়াও এমন তথ্য দিন, যা Claude-কে কার্যকরভাবে কাজ করতে সাহায্য করবে। যেমন, পরিবর্তন করার পর কীভাবে টেস্ট, টাইপচেক বা বিল্ড চালিয়ে তার কাজ যাচাই করা যায়।

**তবে এটি লেখার পদ্ধতিটাও গুরুত্বপূর্ণ।**

CLAUDE.md-তে Claude-এর প্রয়োজন হতে পারে এমন প্রতিটি কমান্ড গুঁজে দেওয়ার চেষ্টা করবেন না। এতে বরং ফলাফল ভালো হবে না। বরং প্রয়োজনীয়, প্রাসঙ্গিক এবং সংক্ষিপ্ত তথ্য দিন, যাতে Claude দ্রুত কোডবেস বুঝে কার্যকরভাবে কাজ শুরু করতে পারে।


# Claude প্রায়ই `CLAUDE.md` উপেক্ষা করে

আপনি যে মডেলই ব্যবহার করুন না কেন, লক্ষ্য করতে পারেন যে Claude অনেক সময় **`CLAUDE.md`**-এর নির্দেশনাগুলো উপেক্ষা করে।

Claude Code এজেন্টের কাছে `CLAUDE.md`-এর বিষয়বস্তুর সঙ্গে নিচের সিস্টেম রিমাইন্ডারটি পাঠায়:

```text
<system-reminder>
IMPORTANT: this context may or may not be relevant to your tasks.
You should not respond to this context unless it is highly relevant to your task.
</system-reminder>
```

এর ফলে, যদি Claude মনে করে যে **`CLAUDE.md`**-এর তথ্য বর্তমান কাজের জন্য প্রাসঙ্গিক নয়, তাহলে সে সেটি উপেক্ষা করবে।

আপনার `CLAUDE.md`-এ যত বেশি এমন তথ্য থাকবে যা সব ধরনের কাজের ক্ষেত্রে প্রযোজ্য নয়, তত বেশি সম্ভাবনা থাকবে যে Claude ফাইলটির নির্দেশনাগুলো উপেক্ষা করবে।



## Anthropic এমন ব্যবস্থা কেন করেছে?

নিশ্চিতভাবে বলা কঠিন, তবে কিছুটা অনুমান করা যায়।

আমরা যে অধিকাংশ `CLAUDE.md` ফাইল দেখেছি, সেগুলোতে এমন অনেক নির্দেশনা থাকে যা সব কাজের জন্য প্রযোজ্য নয়। 

অনেক ব্যবহারকারী Claude-এর কোনো আচরণ পছন্দ না হলে সেটি "হটফিক্স" করার উদ্দেশ্যে `CLAUDE.md`-তে একের পর এক নির্দেশনা যোগ করেন, যদিও সেগুলো সব পরিস্থিতিতে প্রাসঙ্গিক নয়।

সম্ভবত Claude Code টিম দেখেছে যে Claude-কে অপ্রাসঙ্গিক বা নিম্নমানের নির্দেশনা উপেক্ষা করতে বললে, সামগ্রিকভাবে Claude আরও ভালো ফলাফল দেয়।

অর্থাৎ, `CLAUDE.md`-এ যত সংক্ষিপ্ত, পরিষ্কার এবং সর্বজনীনভাবে প্রযোজ্য নির্দেশনা থাকবে, Claude সেগুলো অনুসরণ করার সম্ভাবনাও তত বেশি হবে।


## একটি ভালো `CLAUDE.md` ফাইল তৈরি করা

নিচের অংশে **context engineering best practices** অনুসরণ করে একটি ভালো `CLAUDE.md` ফাইল কীভাবে লেখা যায়, সে সম্পর্কে কিছু সুপারিশ দেওয়া হয়েছে।

আপনার ব্যবহারের ক্ষেত্রে ফলাফল ভিন্ন হতে পারে। সব ধরনের প্রজেক্ট বা সেটআপের জন্য এই নিয়মগুলোর প্রতিটি সর্বোত্তম হবে—এমন নয়।

অন্য যেকোনো নিয়মের মতো, এগুলোও প্রয়োজন হলে ভাঙতে পারেন...

তবে শর্ত হলো:

* আপনি বুঝতে হবে **কখন এবং কেন** এই নিয়ম ভাঙা ঠিক হবে।
* নিয়ম ভাঙার জন্য আপনার কাছে একটি **যৌক্তিক কারণ** থাকতে হবে।

অর্থাৎ, ভালো `CLAUDE.md` লেখার উদ্দেশ্য হলো অন্ধভাবে সব নিয়ম অনুসরণ করা নয়; বরং আপনার নির্দিষ্ট প্রজেক্টের জন্য কোন নির্দেশনা Claude-কে সবচেয়ে বেশি সাহায্য করবে, সেটি বুঝে ব্যবহার করা।







## কম (নির্দেশনা) মানেই বেশি কার্যকর

অনেক সময় মনে হতে পারে যে `CLAUDE.md` ফাইলে Claude-এর প্রয়োজন হতে পারে এমন প্রতিটি কমান্ড, আপনার কোড স্ট্যান্ডার্ড এবং স্টাইল গাইডলাইন একসাথে যোগ করে দেওয়া ভালো হবে। কিন্তু আমরা এটি করার পরামর্শ দিই না।

যদিও এই বিষয়টি নিয়ে এখনো খুব কঠোর বা বিস্তৃত গবেষণা হয়নি, কিছু গবেষণা থেকে নিচের বিষয়গুলো জানা গেছে:

* **Frontier-level thinking LLM** (যেমন বড় ও উন্নত reasoning model) প্রায় **১৫০–২০০টি নির্দেশনা** মোটামুটি ভালোভাবে অনুসরণ করতে পারে।

* ছোট মডেলগুলো বড় মডেলের তুলনায় কম সংখ্যক নির্দেশনার প্রতি মনোযোগ দিতে পারে।

* যেসব মডেল reasoning বা "thinking" ব্যবহার করে না, তারা thinking model-এর তুলনায় আরও কম নির্দেশনা কার্যকরভাবে অনুসরণ করতে পারে।

* **ছোট মডেলগুলো খুব দ্রুত খারাপ ফল দিতে শুরু করে।** বিশেষ করে, নির্দেশনার সংখ্যা বাড়ার সঙ্গে সঙ্গে ছোট মডেলের instruction-following ক্ষমতা **exponential হারে কমে যায়**।

* অন্যদিকে, বড় frontier thinking model-গুলোতে এই ক্ষমতা তুলনামূলকভাবে **linear হারে কমে**।

* এই কারণেই আমরা ছোট মডেল ব্যবহার করে অনেক ধাপের কাজ বা জটিল implementation plan তৈরি করার পরামর্শ দিই না।

* LLM সাধারণত prompt-এর **প্রান্তের (periphery) নির্দেশনাগুলোর দিকে বেশি ঝুঁকে থাকে**:

    * একদম শুরুতে থাকা নির্দেশনা (যেমন Claude Code system message এবং `CLAUDE.md`)
    * একদম শেষে থাকা নির্দেশনা (সর্বশেষ user message)

* নির্দেশনার সংখ্যা যত বাড়ে, নির্দেশনা অনুসরণ করার মান তত কমে যায়।

এর অর্থ হলো, আপনি যখন LLM-কে অনেক বেশি নির্দেশনা দেন, তখন এটি শুধু নতুন বা নিচের দিকে থাকা ("ফাইলের শেষের দিকে থাকা") নির্দেশনাগুলো উপেক্ষা করে না; বরং ধীরে ধীরে **সব নির্দেশনাই সমানভাবে কম গুরুত্ব দিয়ে অনুসরণ করতে শুরু করে।**

---

### সহজ ভাষায়:

`CLAUDE.md`-এ এমন লিখবেন না:

❌

```md
Run this command before every change.
Run this command after every change.
Always use this naming style.
Never use this syntax.
Always write comments.
Never write comments.
Check this folder.
Check that folder.
Follow this pattern.
Follow that pattern.
...
```

বরং লিখুন:

✅

```md
# Project Rules

- Use existing components before creating new ones.
- Follow the current project structure.
- Run tests before finishing changes.
- Keep changes small and focused.
```

অর্থাৎ, **CLAUDE.md যত ছোট, পরিষ্কার এবং প্রজেক্ট-সম্পর্কিত হবে, Claude তত ভালোভাবে সেটি ব্যবহার করতে পারবে।**


আমাদের **Claude Code harness** বিশ্লেষণ থেকে দেখা গেছে যে, Claude Code-এর **system prompt**-এর মধ্যেই প্রায় **৫০টি আলাদা নির্দেশনা** (individual instructions) থাকে।

আপনি যে মডেল ব্যবহার করছেন তার ওপর নির্ভর করে, এটি ইতোমধ্যেই আপনার এজেন্টের নির্ভরযোগ্যভাবে অনুসরণ করার সক্ষমতার প্রায় **এক-তৃতীয়াংশ নির্দেশনা** ব্যবহার করে ফেলতে পারে।

এবং এটি হচ্ছে **rules, plugins, skills বা user messages যোগ করার আগেই**।

এর অর্থ হলো:

আপনার `CLAUDE.md` ফাইলে যতটা সম্ভব **কম সংখ্যক নির্দেশনা রাখা উচিত**।

আদর্শভাবে, সেখানে শুধুমাত্র এমন নির্দেশনা থাকা উচিত যেগুলো:

* আপনার সব ধরনের কাজের ক্ষেত্রে প্রযোজ্য।
* প্রজেক্টের জন্য সত্যিই গুরুত্বপূর্ণ।
* Claude-এর কাজ করার পদ্ধতিকে ধারাবাহিক ও উন্নত করে।

অর্থাৎ, `CLAUDE.md`-এ প্রতিটি ছোটখাটো নিয়ম, অস্থায়ী workaround, বা নির্দিষ্ট কোনো একবারের কাজের নির্দেশনা যোগ না করে, শুধুমাত্র **সর্বজনীনভাবে প্রযোজ্য এবং দীর্ঘমেয়াদি গুরুত্বপূর্ণ নির্দেশনাগুলো** রাখা উচিত।





## CLAUDE.md কীভাবে Claude Code-কে আপনার কোডবেস বুঝতে সাহায্য করে (এবং কেন Claude কখনো কখনো এটি উপেক্ষা করে)

প্রতিটি নতুন session-এর শুরুতে Claude আপনার codebase সম্পর্কে কিছুই জানে না। তাই `CLAUDE.md` ব্যবহার করে তাকে আপনার project সম্পর্কে প্রয়োজনীয় context দেওয়া যায়।

একটি ভালো `CLAUDE.md` সাধারণত ৩টি বিষয় কভার করে:

### WHAT (কী)

Claude-কে জানান:

* আপনার Tech Stack কী
* Project structure কেমন
* কোন folder বা package কী কাজ করে
* Monorepo হলে কোন app এবং shared package-এর দায়িত্ব কী

এতে Claude সহজে বুঝতে পারে কোথায় কোন code খুঁজতে হবে।

---

### WHY (কেন)

Project-এর উদ্দেশ্য এবং বিভিন্ন অংশের ভূমিকা ব্যাখ্যা করুন।

যেমন:

* এই application কেন তৈরি করা হয়েছে
* কোন module কী দায়িত্ব পালন করে
* কোন component বা service কেন আছে

এতে Claude শুধু code edit না করে project-এর context বুঝে কাজ করতে পারে।

---

### HOW (কীভাবে)

Claude-কে জানান কীভাবে project-এ কাজ করতে হবে।

যেমন:

* Node-এর পরিবর্তে Bun ব্যবহার করলে সেটি উল্লেখ করুন
* কোন command দিয়ে test চালাতে হবে
* কীভাবে lint, type-check বা build verify করতে হবে

তবে একটি বিষয় গুরুত্বপূর্ণ:

`CLAUDE.md`-এ সব সম্ভাব্য command বা প্রতিটি coding rule ঢুকিয়ে দেওয়ার চেষ্টা করবেন না।

কম কিন্তু গুরুত্বপূর্ণ instruction সাধারণত বেশি কার্যকর।

---

## Claude কি সবসময় CLAUDE.md follow করে?

না।

অনেক সময় দেখা যায় Claude `CLAUDE.md`-এর কিছু instruction ignore করে।

কারণ Claude Code context-এর সাথে একটি reminder যোগ করে:

```
<system-reminder>
IMPORTANT: this context may or may not be relevant to your tasks.
You should not respond to this context unless it is highly relevant to your task.
</system-reminder>
```

এর মানে হলো:

Claude প্রথমে বিচার করে এই context বর্তমান কাজের জন্য relevant কিনা।

যদি `CLAUDE.md`-এ এমন অনেক instruction থাকে যা সব task-এর জন্য প্রযোজ্য নয়, তাহলে সেগুলো ignore হওয়ার সম্ভাবনা বাড়ে।

---

## একটি ছোট experiment করতে পারেন

আপনার Vue project-এর `CLAUDE.md`-এ যোগ করুন:

```md
## Rules

- Every response must start with: 🚀 Vue Project
- Every response must end with: I followed CLAUDE.md
```

তারপর Claude Code session শুরু করে বিভিন্ন প্রশ্ন করুন।

যদি Claude এই text ব্যবহার করে, তাহলে বুঝবেন সে আপনার `CLAUDE.md` load করেছে।

তবে মনে রাখবেন, এটি শুধু একটি test। বাস্তব project-এ `CLAUDE.md`-এ এমন instruction রাখাই ভালো যেগুলো সত্যিই development workflow এবং codebase understanding-এর জন্য গুরুত্বপূর্ণ।

---

মূল শিক্ষা:

**CLAUDE.md হলো Claude-কে control করার জায়গা নয়; এটি Claude-কে আপনার project বুঝতে সাহায্য করার context layer।**

কম, পরিষ্কার এবং project-specific instruction দিলে Claude সবচেয়ে ভালো ফল দেয়।


# August - 3

নিচে বাংলায় অনুবাদ করা হলো:

---

### **কম নির্দেশনা (Instructions) দেওয়াই ভালো**

অনেক সময় মনে হতে পারে যে `CLAUDE.md` ফাইলে Claude-এর প্রয়োজন হতে পারে এমন সব কমান্ড, কোডিং স্ট্যান্ডার্ড এবং স্টাইল গাইডলাইন একসাথে লিখে রাখা উচিত। কিন্তু আমরা এটি করার পরামর্শ দিই না।

যদিও এ বিষয়ে খুব গভীর গবেষণা হয়নি, তবুও কিছু গবেষণা থেকে নিম্নলিখিত বিষয়গুলো জানা গেছে:

* আধুনিক (Frontier) **thinking LLM** সাধারণত প্রায় **১৫০–২০০টি নির্দেশনা** যথেষ্ট ধারাবাহিকভাবে অনুসরণ করতে পারে।
* **ছোট মডেল** বড় মডেলের তুলনায় অনেক কম নির্দেশনা কার্যকরভাবে অনুসরণ করতে পারে। একইভাবে, **non-thinking model**-এর সক্ষমতাও thinking model-এর চেয়ে কম।
* নির্দেশনার সংখ্যা বাড়ার সঙ্গে সঙ্গে **ছোট মডেলের কর্মক্ষমতা খুব দ্রুত (প্রায় সূচকীয় হারে) কমে যায়**। অন্যদিকে, বড় Frontier Thinking Model-এর ক্ষেত্রে এই অবনতি তুলনামূলকভাবে **রৈখিক (linear)** হয়। তাই বহু ধাপের কাজ বা জটিল implementation plan-এর জন্য ছোট মডেল ব্যবহার না করার পরামর্শ দেওয়া হয়।
* LLM সাধারণত **prompt-এর একেবারে শুরু** (যেমন Claude Code-এর system prompt এবং `CLAUDE.md`) এবং **একেবারে শেষের অংশ** (সর্বশেষ user message)-এর নির্দেশনাগুলোকে বেশি গুরুত্ব দেয়।
* নির্দেশনার সংখ্যা যত বাড়ে, **সব নির্দেশনা অনুসরণ করার মান সমানভাবে কমতে থাকে**। অর্থাৎ, শুধু ফাইলের নিচের দিকের নির্দেশনাগুলোই উপেক্ষা হয় না; বরং ধীরে ধীরে **সব নির্দেশনাই সমানভাবে কম গুরুত্ব পেতে শুরু করে**।

### **Instruction Following**

আমাদের বিশ্লেষণ অনুযায়ী, Claude Code-এর **system prompt**-এ প্রায় **৫০টি আলাদা নির্দেশনা** আগে থেকেই থাকে।

আপনি কোন মডেল ব্যবহার করছেন তার ওপর নির্ভর করে, এটি এমন হতে পারে যে Claude ইতোমধ্যেই তার নির্ভরযোগ্যভাবে অনুসরণ করতে সক্ষম মোট নির্দেশনার প্রায় **এক-তৃতীয়াংশ** ব্যবহার করে ফেলেছে—এবং এটি `CLAUDE.md`-এর নিয়ম, plugins, skills বা আপনার user message যোগ করার আগেই।

**এর অর্থ হলো, `CLAUDE.md` ফাইলে যত কম নির্দেশনা রাখা যায় ততই ভালো।** আদর্শভাবে সেখানে **শুধুমাত্র সেই নির্দেশনাগুলোই থাকা উচিত, যা আপনার প্রজেক্টের সব ধরনের কাজের জন্য সবসময় প্রযোজ্য (universally applicable)।**

### **CLAUDE.md ফাইলের দৈর্ঘ্য ও প্রাসঙ্গিকতা (Applicability)**

সবকিছু একই থাকলে, একটি LLM কোনো নির্দিষ্ট কাজের ক্ষেত্রে ভালো পারফর্ম করে যখন তার **context window**-এ শুধুমাত্র প্রাসঙ্গিক ও কেন্দ্রীভূত তথ্য থাকে—যেমন:

* উদাহরণ (examples)
* সম্পর্কিত ফাইল (related files)
* টুল কল (tool calls)
* টুলের ফলাফল (tool results)

এর বিপরীতে, যদি context window-এ অনেক অপ্রয়োজনীয় তথ্য থাকে, তাহলে মডেলের কর্মক্ষমতা কমে যায়।

যেহেতু `CLAUDE.md` ফাইল **প্রতিটি Claude Code session-এ লোড হয়**, তাই এর মধ্যে থাকা বিষয়বস্তু যতটা সম্ভব **সবার জন্য প্রযোজ্য (universally applicable)** হওয়া উচিত।

উদাহরণস্বরূপ:

এমন নির্দেশনা দেওয়া এড়িয়ে চলুন:

> "নতুন database schema তৈরি করার সময় এই structure ব্যবহার করতে হবে।"

কারণ আপনি যখন database সম্পর্কিত নয় এমন কোনো কাজ করবেন, তখন এই নির্দেশনা Claude-কে বিভ্রান্ত করবে এবং অপ্রয়োজনীয় context দখল করবে।

### **দৈর্ঘ্যের ক্ষেত্রে: কমই ভালো**

"Less is more" নীতিটি `CLAUDE.md`-এর দৈর্ঘ্যের ক্ষেত্রেও প্রযোজ্য।

যদিও Anthropic আনুষ্ঠানিকভাবে `CLAUDE.md` কত বড় হওয়া উচিত তার কোনো নির্দিষ্ট সীমা দেয়নি, সাধারণভাবে মনে করা হয়:

* **৩০০ লাইনের কম রাখা ভালো**
* এর চেয়েও ছোট হলে আরও ভালো

উদাহরণ হিসেবে, HumanLayer-এর root `CLAUDE.md` ফাইল **৬০ লাইনেরও কম**।

**মূল ধারণা:**
`CLAUDE.md`-এ শুধু সেই নিয়মগুলো রাখুন যেগুলো Claude-কে প্রতিটি কাজের সময় জানা দরকার। নির্দিষ্ট feature, database design, বা কোনো একবারের কাজের নির্দেশনা সেখানে না রেখে প্রয়োজনে আলাদা documentation বা task prompt-এ দিন।

### **Progressive Disclosure (ধাপে ধাপে তথ্য প্রকাশ)**

একটি সংক্ষিপ্ত `CLAUDE.md` ফাইল তৈরি করা, যেখানে Claude-এর জানা দরকার এমন সব বিষয় রাখা হবে—বিশেষ করে বড় প্রজেক্টে—কঠিন হতে পারে।

এই সমস্যা সমাধানের জন্য আমরা **Progressive Disclosure** নীতি ব্যবহার করতে পারি। এর মাধ্যমে Claude শুধুমাত্র তখনই নির্দিষ্ট task বা project সম্পর্কিত নির্দেশনা দেখবে, যখন তার সেগুলোর প্রয়োজন হবে।

### কীভাবে কাজ করবে?

আপনার প্রজেক্ট তৈরি, test চালানো, code convention, architecture বা অন্যান্য গুরুত্বপূর্ণ তথ্য সব `CLAUDE.md`-এ রাখার পরিবর্তে, সেগুলো আলাদা আলাদা Markdown ফাইলে রাখার পরামর্শ দেওয়া হয়।

এই ফাইলগুলোকে এমন নাম দিন, যাতে নাম দেখেই বোঝা যায় ভেতরে কী আছে।

উদাহরণ:

```
agent_docs/
  |- building_the_project.md
  |- running_tests.md
  |- code_conventions.md
  |- service_architecture.md
  |- database_schema.md
  |- service_communication_patterns.md
```

এখানে:

* `building_the_project.md` → কীভাবে প্রজেক্ট build করতে হবে
* `running_tests.md` → কীভাবে test চালাতে হবে
* `code_conventions.md` → coding style ও rules
* `service_architecture.md` → service structure ও design
* `database_schema.md` → database সম্পর্কিত তথ্য
* `service_communication_patterns.md` → service-গুলোর মধ্যে যোগাযোগের নিয়ম

### এরপর `CLAUDE.md`-এ কী রাখবেন?

`CLAUDE.md`-এ শুধু এই document গুলোর একটি তালিকা এবং ছোট বর্ণনা রাখুন।

উদাহরণ:

```
Additional documentation:

- agent_docs/building_the_project.md
  How to build and run the project.

- agent_docs/running_tests.md
  Testing instructions.

- agent_docs/code_conventions.md
  Coding standards.

Before starting work, decide which documents are relevant and read only those.
```

অথবা Claude-কে বলতে পারেন:

> কাজ শুরু করার আগে কোন কোন documentation পড়া দরকার তা আমাকে দেখাও। আমার অনুমোদনের পর সেগুলো পড়ো।

### Copy না করে Reference (Pointer) ব্যবহার করুন

**Prefer pointers to copies.**

অর্থাৎ, একই তথ্য বারবার কপি করে বিভিন্ন জায়গায় রাখবেন না।

যেমন:

❌ খারাপ পদ্ধতি:

`CLAUDE.md`

```text
User service validates email like this:
(code snippet...)
```

কারণ সময়ের সাথে সাথে এই code snippet পুরনো হয়ে যাবে।

✅ ভালো পদ্ধতি:

```text
See:
src/services/user_service.py:45-80
```

অর্থাৎ Claude-কে আসল source file-এর নির্দিষ্ট জায়গার দিকে নির্দেশ করুন।

এর সুবিধা:

* Documentation সবসময় updated থাকে
* Duplicate তথ্য কমে
* Claude authoritative source থেকে শিখতে পারে

### Claude Skills-এর সাথে সম্পর্ক

ধারণাগতভাবে এটি অনেকটা **Claude Skills** কীভাবে কাজ করার জন্য তৈরি হয়েছে তার মতো।

তবে পার্থক্য হলো:

* **Progressive Disclosure / CLAUDE.md docs** → মূলত নির্দেশনা ও context দেওয়ার জন্য
* **Claude Skills** → বেশি focus করে tool ব্যবহার ও নির্দিষ্ট workflow-এর উপর

### মূল ধারণা

`CLAUDE.md` হবে একটি **index বা navigation guide**।

এতে সব তথ্য ঢুকিয়ে না রেখে:

```
CLAUDE.md
    ↓
    ├── Build instructions
    ├── Testing instructions
    ├── Architecture docs
    ├── Database docs
    └── Coding rules
```

Claude যখন যে কাজ করবে, শুধু সেই কাজের জন্য প্রয়োজনীয় document পড়বে।

**ফলাফল:**

* কম context usage
* কম confusion
* বেশি accurate code changes
* বড় project-এ ভালো performance


### **Claude একটি (দামি) Linter নয়**

অনেক মানুষ `CLAUDE.md` ফাইলে যে বিষয়গুলো সবচেয়ে বেশি যোগ করে, তার মধ্যে একটি হলো **code style guideline**।

কিন্তু একটি গুরুত্বপূর্ণ নিয়ম হলো:

> **Linter-এর কাজ করানোর জন্য কখনো LLM ব্যবহার করবেন না।**

কারণ:

* সাধারণ linter এবং formatter-এর তুলনায় LLM অনেক বেশি ব্যয়বহুল।
* LLM অনেক ধীরগতির।
* যেখানে নির্দিষ্ট (deterministic) tool ব্যবহার করা সম্ভব, সেখানে সেটিই ব্যবহার করা উচিত।

---

### **Code Style Guideline কেন `CLAUDE.md`-এ না রাখাই ভালো?**

Code style rules যোগ করলে সাধারণত:

* অনেক অতিরিক্ত instruction যোগ হয়
* অপ্রয়োজনীয় code snippet context window দখল করে
* LLM-এর performance কমে যায়
* instruction-following ক্ষমতা কমে যায়
* গুরুত্বপূর্ণ কাজের জন্য available context কমে যায়

অর্থাৎ, Claude-কে যদি বলা হয়:

> "সবসময় এভাবে code লিখবে, এই naming convention মানবে, এই formatting করবে..."

তাহলে এই অতিরিক্ত নিয়মগুলো তার মূল কাজের জন্য প্রয়োজনীয় context-এর জায়গা দখল করে।

---

### **LLM নিজে থেকেই Pattern শিখতে পারে**

LLM-এর একটি বড় ক্ষমতা হলো **in-context learning**।

যদি আপনার codebase-এ আগে থেকেই নির্দিষ্ট:

* coding style
* architecture pattern
* naming convention
* implementation approach

থাকে, তাহলে Claude সাধারণত:

* codebase search করে
* existing example দেখে
* সেই pattern অনুসরণ করতে পারে

অর্থাৎ আলাদা করে সব নিয়ম লিখে দেওয়ার প্রয়োজন হয় না।

ভালোভাবে তৈরি একটি codebase বা research document থাকলে Claude সাধারণত নিজেই existing convention অনুসরণ করবে।

---

### **তবুও যদি Style Rule গুরুত্বপূর্ণ হয়**

যদি কোনো নির্দিষ্ট formatting বা style নিয়ে আপনার খুব শক্ত অবস্থান থাকে, তাহলে ভালো পদ্ধতি হলো:

Claude-কে নিজে formatting issue খুঁজতে না দিয়ে একটি **Claude Code Stop Hook** ব্যবহার করা।

Workflow:

```
Claude code change করে
        ↓
Stop Hook চালু হয়
        ↓
Formatter + Linter রান করে
        ↓
Error Claude-কে দেখানো হয়
        ↓
Claude Fix করে
```

অর্থাৎ:

❌ খারাপ:

```
Claude → নিজে formatting সমস্যা খুঁজবে
```

✅ ভালো:

```
Linter → সমস্যা বের করবে
Claude → সমস্যা ঠিক করবে
```

---

### **Auto-fix করতে পারে এমন Linter ব্যবহার করুন**

আরও ভালো ফলাফলের জন্য:

* এমন linter ব্যবহার করুন যা নিজে সমস্যা ঠিক করতে পারে।
* Auto-fix rules ভালোভাবে configure করুন।

উদাহরণ হিসেবে, Biome-এর মতো tool ব্যবহার করা যায়।

যে সমস্যাগুলো নিরাপদে auto-fix করা যায়, সেগুলোর জন্য auto-fix enable করলে manual কাজ কমে যায়।

---

### **আরেকটি ভালো পদ্ধতি: Slash Command ব্যবহার করা**

আপনি চাইলে Claude Code-এর জন্য একটি Slash Command তৈরি করতে পারেন যেখানে থাকবে:

* আপনার code guideline
* বর্তমান git changes
* version control diff
* `git status`

এরপর workflow আলাদা করতে পারেন:

```
Implementation
        ↓
Code Review
        ↓
Formatting & Cleanup
        ↓
Commit
```

এতে Claude-এর কাজ পরিষ্কার ভাগে ভাগ হয়।

---

### **মূল ধারণা**

Claude-এর কাজ:

✅ নতুন feature তৈরি করা
✅ জটিল সমস্যা সমাধান করা
✅ code বুঝে পরিবর্তন করা
✅ architecture নিয়ে সাহায্য করা

Claude-এর কাজ নয়:

❌ indentation ঠিক করা
❌ semicolon যোগ করা
❌ formatting ঠিক করা
❌ lint rule enforce করা

এসব কাজের জন্য:

```
Linter + Formatter = Rules enforce করবে
Claude = সমস্যাগুলো বুঝে সমাধান করবে
```

এভাবে Claude দ্রুত, কম context ব্যবহার করে এবং বেশি নির্ভুলভাবে কাজ করতে পারে।


### **`/init` ব্যবহার করবেন না বা Auto-generate করা `CLAUDE.md` ব্যবহার করবেন না**

Claude Code এবং OpenCode-এর মতো অন্যান্য harness-এ এমন সুবিধা আছে, যার মাধ্যমে স্বয়ংক্রিয়ভাবে `CLAUDE.md` (বা `AGENTS.md`) ফাইল তৈরি করা যায়।

কিন্তু মনে রাখতে হবে:

`CLAUDE.md` Claude Code-এর **প্রতিটি session-এর সাথে লোড হয়**।

তাই এটি আপনার পুরো workflow-এর একটি অত্যন্ত গুরুত্বপূর্ণ অংশ (high leverage point)।

এটি ভালো ফল দিতে পারে, আবার ভুলভাবে ব্যবহার করলে বড় সমস্যা তৈরি করতে পারে।

---

### **একটি খারাপ Code Line-এর প্রভাব সীমিত**

যদি একটি code line খারাপ হয়:

```text
Bad code → একটি ছোট সমস্যা
```

কিন্তু:

* একটি খারাপ implementation plan-এর একটি লাইন অনেকগুলো খারাপ code তৈরি করতে পারে।
* একটি research document যদি system সম্পর্কে ভুল ধারণা দেয়, তাহলে সেই ভুল থেকে তৈরি হতে পারে:

  * ভুল implementation plan
  * এবং শেষ পর্যন্ত আরও অনেক ভুল code

অর্থাৎ ভুল যত উপরের স্তরে হয়, তার প্রভাব তত বেশি ছড়িয়ে পড়ে।

---

### **কিন্তু `CLAUDE.md` আরও বেশি গুরুত্বপূর্ণ**

কারণ `CLAUDE.md` প্রভাব ফেলে:

* আপনার workflow-এর প্রতিটি ধাপে
* Claude-এর প্রতিটি decision-এ
* তৈরি হওয়া প্রতিটি artifact-এ

অর্থাৎ:

```text
CLAUDE.md
      ↓
Claude-এর বোঝাপড়া
      ↓
Planning
      ↓
Implementation
      ↓
Code changes
      ↓
Final output
```

যদি শুরুতেই `CLAUDE.md`-এ ভুল বা অপ্রয়োজনীয় নির্দেশনা থাকে, তাহলে পুরো workflow-এ তার প্রভাব পড়তে পারে।

---

### **তাই কী করা উচিত?**

আমাদের মতে, `CLAUDE.md`-এর প্রতিটি লাইন যোগ করার আগে ভালোভাবে চিন্তা করা উচিত।

নিজেকে প্রশ্ন করুন:

* এই নিয়ম কি প্রতিটি task-এর জন্য প্রয়োজনীয়?
* Claude কি এটি না জানলে ভুল করবে?
* এটি কি অন্য কোনো tool/documentation দিয়ে ভালোভাবে handle করা যায়?
* এই তথ্য কি সত্যিই সব session-এ থাকা দরকার?

যে তথ্যগুলো শুধুমাত্র নির্দিষ্ট কাজের জন্য দরকার, সেগুলো `CLAUDE.md`-এ না রেখে আলাদা documentation ফাইলে রাখা ভালো।

---

### **মূল ধারণা**

`CLAUDE.md` কোনো সাধারণ notes file নয়।

এটি Claude-এর জন্য একটি **core operating instruction file**।

তাই:

❌ Auto-generated বড় `CLAUDE.md`
❌ অপ্রয়োজনীয় নিয়মের তালিকা
❌ পুরনো বা নির্দিষ্ট task-এর নির্দেশনা

এর পরিবর্তে:

✅ ছোট
✅ চিন্তাভাবনা করে লেখা
✅ সব project-wide কাজের জন্য প্রযোজ্য নির্দেশনা

একটি ভালো `CLAUDE.md` তৈরি করে।


## **উপসংহার (In Conclusion)**

`CLAUDE.md` ফাইলের মূল উদ্দেশ্য হলো Claude-কে আপনার codebase-এর সাথে পরিচিত করানো।

এতে আপনার project-এর তিনটি গুরুত্বপূর্ণ বিষয় পরিষ্কার করা উচিত:

* **WHY** → আপনার project কেন তৈরি করা হয়েছে, এর উদ্দেশ্য কী
* **WHAT** → project কী করে, এর প্রধান অংশগুলো কী
* **HOW** → project-এর সাথে কীভাবে কাজ করতে হবে

---

### **কম নির্দেশনা (Instructions) দেওয়াই ভালো**

"Less is more" নীতিটি এখানে প্রযোজ্য।

প্রয়োজনীয় নির্দেশনা বাদ দেওয়া উচিত নয়, তবে যতটা সম্ভব **কম সংখ্যক নির্দেশনা** রাখা উচিত।

`CLAUDE.md`-এ শুধুমাত্র সেই নিয়মগুলো রাখুন যেগুলো Claude-এর প্রতিটি কাজের জন্য দরকার।

---

### **`CLAUDE.md` সংক্ষিপ্ত এবং সর্বজনীন রাখুন**

এর বিষয়বস্তু হওয়া উচিত:

✅ ছোট
✅ পরিষ্কার
✅ পুরো project-এর জন্য প্রযোজ্য

এড়িয়ে চলুন:

❌ নির্দিষ্ট feature-এর নির্দেশনা
❌ একবারের কাজের নিয়ম
❌ অতিরিক্ত code style details
❌ অপ্রয়োজনীয় documentation

---

### **Progressive Disclosure ব্যবহার করুন**

Claude-কে এমন সব তথ্য একসাথে দেবেন না যা ভবিষ্যতে কখনো দরকার হতে পারে।

বরং:

* Claude-কে জানান কোথায় গুরুত্বপূর্ণ তথ্য পাওয়া যাবে।
* প্রয়োজন হলে সে যেন সেই তথ্য খুঁজে ব্যবহার করতে পারে।

উদাহরণ:

❌ খারাপ:

```text
CLAUDE.md-এ পুরো database schema, architecture এবং সব coding rules লিখে রাখা।
```

✅ ভালো:

```text
Database information:
See docs/database_schema.md

Architecture:
See docs/service_architecture.md
```

এর ফলে:

* context window অপ্রয়োজনীয় তথ্য দিয়ে ভরে যায় না
* instruction সংখ্যা কম থাকে
* Claude বেশি গুরুত্বপূর্ণ বিষয়ের দিকে মনোযোগ দিতে পারে

---

### **Claude Linter নয়**

Claude-এর কাজ নয়:

* formatting ঠিক করা
* style enforce করা
* lint error খোঁজা

এসব কাজের জন্য ব্যবহার করুন:

* Linter
* Code formatter
* Hooks
* Slash Commands

যেখানে সম্ভব, deterministic tools ব্যবহার করুন এবং Claude-কে মূলত সমস্যা সমাধান ও implementation-এর কাজে ব্যবহার করুন।

---

### **Auto-generate করা `CLAUDE.md` এড়িয়ে চলুন**

`CLAUDE.md` পুরো Claude Code workflow-এ প্রভাব ফেলে।

এটি প্রভাবিত করে:

```text
Understanding
      ↓
Planning
      ↓
Implementation
      ↓
Code Changes
      ↓
Final Output
```

তাই এটি auto-generate না করে নিজের project অনুযায়ী চিন্তাভাবনা করে তৈরি করা উচিত।

---

## **সংক্ষেপে মূল নীতিগুলো**

| নীতি                    | অর্থ                                            |
| ----------------------- | ----------------------------------------------- |
| CLAUDE.md = Onboarding  | Claude-কে project বুঝতে সাহায্য করবে            |
| WHY, WHAT, HOW          | Project-এর উদ্দেশ্য, কাজ ও পদ্ধতি ব্যাখ্যা করবে |
| Less is more            | কম কিন্তু গুরুত্বপূর্ণ instruction রাখবে        |
| Progressive Disclosure  | প্রয়োজনের সময় প্রয়োজনীয় তথ্য খুঁজে নেবে     |
| Use deterministic tools | Linter/Formatter দিয়ে নিয়ম enforce করবে       |
| Avoid auto-generation   | নিজে ভেবে তৈরি করা ভালো                         |

একটি ভালো `CLAUDE.md` হলো বড় instruction manual নয়; এটি হলো Claude-এর জন্য একটি **ছোট, পরিষ্কার এবং কার্যকর navigation guide**।
