# Functional Requirement

**Functional Requirement মানে: Software/System কী কী কাজ করতে পারবে — সেই কাজগুলোর তালিকা।**

ধরুন আপনি একটা **User Management System** বানাচ্ছেন। তাহলে system-এর কী কী feature থাকা দরকার, সেগুলো FR দিয়ে লেখা হয়।

---

## FR-001: User Registration (Account তৈরি)

এর মানে:

> System user-কে নতুন account তৈরি করার সুযোগ দেবে।

যখন user register করবে, তখন সে কিছু তথ্য দেবে:

**Input:**

```
name
email
password
password_confirmation
```

উদাহরণ:

```
Name: Rahim
Email: rahim@gmail.com
Password: 12345678
Confirm Password: 12345678
```

তারপর system কিছু rule check করবে।

### Rules:

### 1. Email unique হতে হবে

মানে: একই email দিয়ে দুইটা account তৈরি করা যাবে না।

Example: Database:

| id | email                                     |
| -- | ----------------------------------------- |
| 1  | [rahim@gmail.com](mailto:rahim@gmail.com) |

এখন কেউ আবার:

```
Email: rahim@gmail.com
```

দিলে system বলবে:

```
Email already exists
```

---

### 2. Password minimum length থাকতে হবে

মানে password ছোট হলে accept করবে না।

Example:

Rule:

```
Minimum password length = 8 characters
```

তাহলে:

```
12345 ❌
12345678 ✅
```

---

### 3. Password hash করে store করতে হবে

এটা security এর জন্য।

User password:

```
mypassword123
```

Database-এ সরাসরি রাখা যাবে না।

না:

| email                                     | password      |
| ----------------------------------------- | ------------- |
| [rahim@gmail.com](mailto:rahim@gmail.com) | mypassword123 |

বরং hash করে রাখতে হবে:

| email                                     | password        |
| ----------------------------------------- | --------------- |
| [rahim@gmail.com](mailto:rahim@gmail.com) | $2y$10$8x93.... |

কারণ database leak হলেও আসল password দেখা যাবে না।

---

# FR-002: User Login

# FR-003: User Logout

# FR-004: Profile Management

# FR-005: Password Recovery

# FR-006: Email Verification

# FR-007: Role Management

---

সংক্ষেপে:

| FR     | System কী করবে                |
| ------ | ----------------------------- |
| FR-001 | Account তৈরি করবে             |
| FR-002 | Login করাবে                   |
| FR-003 | Logout করাবে                  |
| FR-004 | Profile দেখাবে ও update করবে  |
| FR-005 | Password reset করবে           |
| FR-006 | Email verify করবে             |
| FR-007 | Role ও permission manage করবে |

এগুলো আসলে software বানানোর আগে developer, client, tester সবাইকে বোঝানোর জন্য লেখা হয়—যাতে সবাই জানে system-এর expected behavior কী।