# Functional Requirement

**Functional Requirement মানে: Software/System কী কী কাজ করতে পারবে — সেই কাজগুলোর তালিকা।**

ধরুন আপনি একটা **User Management System** বানাচ্ছেন। তাহলে system-এর কী কী feature থাকা দরকার, সেগুলো FR দিয়ে লেখা হয়।

---

## FR-001: User Registration (Account তৈরি)

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

# NFR-001: Performance Requirement

মানে system কত দ্রুত কাজ করবে।

# NFR-002: Security Requirement

মানে system কতটা নিরাপদ হবে।

# NFR-003: Availability Requirement

মানে system কত সময় available থাকবে।

# NFR-005: Scalability Requirement

মানে user বাড়লেও system handle করতে পারবে কিনা।

# NFR-006: Maintainability Requirement

মানে developer সহজে system update করতে পারবে।

# NFR-007: Usability Requirement

মানে user-এর জন্য system কত সহজ।

# NFR-008: Compatibility Requirement

মানে বিভিন্ন environment-এ কাজ করতে হবে।

# NFR-009: Backup & Recovery Requirement

মানে data backup এবং recovery।

# NFR-010: Audit & Logging Requirement

মানে system-এর গুরুত্বপূর্ণ কাজ record রাখা।

## FR বনাম NFR পার্থক্য:

| Functional Requirement | Non Functional Requirement    |
| ---------------------- | ----------------------------- |
| System কী করবে         | System কেমনভাবে করবে          |
| Feature নিয়ে কথা বলে   | Quality নিয়ে কথা বলে          |
| Login করা              | Login কত দ্রুত হবে            |
| Profile update করা     | Profile update কত secure হবে  |
| Password reset         | Reset process কত reliable হবে |

---
সহজ কথায়: **FR বলে "কি বানাবো", আর NFR বলে "কী মানের বানাবো"।**
