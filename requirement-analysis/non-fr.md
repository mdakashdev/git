# Non Functional Requirement


**Non-Functional Requirement (NFR)** মানে হলো:

> **System কী কাজ করবে সেটা নয়, বরং System কাজগুলো কত ভালোভাবে করবে — তার মানদণ্ড।**

সহজভাবে:

* **Functional Requirement (FR)** = System-এর **কাজ** কী?
* **Non-Functional Requirement (NFR)** = System-এর **গুণগত মান / performance / security / reliability** কেমন হবে?

উদাহরণ:

FR:

> User login করতে পারবে।

NFR:

> Login request 2 সেকেন্ডের মধ্যে complete হতে হবে এবং password secure থাকতে হবে।

---

## কিছু সাধারণ Non-Functional Requirement:

---

# NFR-001: Performance Requirement

মানে system কত দ্রুত কাজ করবে।

Example:

* Page load 3 seconds-এর মধ্যে হতে হবে।
* Login response 2 seconds-এর মধ্যে দিতে হবে।
* System প্রতি মিনিটে 10,000 request handle করতে পারবে।

Example:

User login করলো:

```
Request → Server → Response

সময়: < 2 seconds
```

---

# NFR-002: Security Requirement

মানে system কতটা নিরাপদ হবে।

Example:

* Password encrypted/hashed করে রাখতে হবে।
* User password database-এ plain text হিসেবে রাখা যাবে না।
* API access token ব্যবহার করতে হবে।
* Unauthorized user sensitive data দেখতে পারবে না।

Example:

যদি একজন Customer অন্য user-এর profile দেখতে চায়:

```
Request:
GET /users/5/profile

Response:
403 Forbidden
```

---

# NFR-003: Availability Requirement

মানে system কত সময় available থাকবে।

Example:

```
System availability:
99.9%
```

মানে বছরে খুব কম সময় system down থাকবে।

যেমন:

* Website 24/7 চালু থাকবে।
* Server failure হলে দ্রুত recover করবে।

---

# NFR-004: Reliability Requirement

মানে system কতটা নির্ভরযোগ্য।

Example:

* Data হারানো যাবে না।
* Transaction মাঝপথে fail হলে rollback করতে হবে।
* System crash হলেও data safe থাকতে হবে।

Example:

Payment করার সময়:

```
Money deducted
       ↓
System crash
       ↓
Transaction rollback
```

---

# NFR-005: Scalability Requirement

মানে user বাড়লেও system handle করতে পারবে কিনা।

Example:

আজ:

```
1,000 users
```

আগামীকাল:

```
1,000,000 users
```

হলেও system কাজ করতে হবে।

---

# NFR-006: Maintainability Requirement

মানে developer সহজে system update করতে পারবে।

Example:

* Code clean হতে হবে।
* Proper documentation থাকতে হবে।
* Modular architecture ব্যবহার করতে হবে।

যেমন:

Login system আলাদা module:

```
Auth Module

User Module

Payment Module
```

---

# NFR-007: Usability Requirement

মানে user-এর জন্য system কত সহজ।

Example:

* Interface সহজ হতে হবে।
* Mobile এবং desktop দুই জায়গায় কাজ করতে হবে।
* Error message পরিষ্কার হতে হবে।

খারাপ:

```
Error 500
```

ভালো:

```
Your password is incorrect.
Please try again.
```

---

# NFR-008: Compatibility Requirement

মানে বিভিন্ন environment-এ কাজ করতে হবে।

Example:

Browser:

```
Chrome
Firefox
Edge
Safari
```

Device:

```
Mobile
Tablet
Desktop
```

---

# NFR-009: Backup & Recovery Requirement

মানে data backup এবং recovery।

Example:

* প্রতিদিন database backup নিতে হবে।
* System failure হলে 1 ঘণ্টার মধ্যে restore করতে হবে।

---

# NFR-010: Audit & Logging Requirement

মানে system-এর গুরুত্বপূর্ণ কাজ record রাখা।

Example:

Admin যদি permission পরিবর্তন করে:

```
User: Admin01
Action: Removed Delete Permission
Time: 10:30 AM
```

এই তথ্য log-এ থাকবে।

---

## FR বনাম NFR পার্থক্য:

| Functional Requirement | Non Functional Requirement    |
| ---------------------- | ----------------------------- |
| System কী করবে         | System কেমনভাবে করবে          |
| Feature নিয়ে কথা বলে   | Quality নিয়ে কথা বলে          |
| Login করা              | Login কত দ্রুত হবে            |
| Profile update করা     | Profile update কত secure হবে  |
| Password reset         | Reset process কত reliable হবে |

---

একটা **User Management System**-এর জন্য NFR উদাহরণ:

```
NFR-001: System must respond within 2 seconds.

NFR-002: Password must be stored using hashing algorithm.

NFR-003: System must support 10,000 concurrent users.

NFR-004: System availability must be 99.9%.

NFR-005: User data must be encrypted during transmission.

NFR-006: System must generate logs for admin activities.
```

সহজ কথায়: **FR বলে "কি বানাবো", আর NFR বলে "কী মানের বানাবো"।**