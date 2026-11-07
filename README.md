# Non-Tech Title & Microcopy Sanitizer Skill (`skill-nontech-title`)

[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](https://opensource.org/licenses/MIT)
[![Skill Standard](https://img.shields.io/badge/Skill-writing--skills%20compatible-brightgreen.svg)](#)

> **"If a user needs a developer manual to understand a button or status badge, the microcopy has failed. If an interface reveals backend tables, ports, cloud hosts, or status codes, the system has leaked."**

An AI Agent Skill designed to audit, sanitize, and rewrite user-facing UI/UX copy, page titles, form labels, buttons, badges, empty states, and error alerts. It eliminates developer jargon, backend database enums, and security-sensitive system leaks, converting them into empathetic, actionable, non-technical plain language (optimized for Thai public sector, healthcare, cooperative, and enterprise systems).

---

## 🌟 Why This Skill Exists

In many software projects, user interfaces are created by engineers who inadvertently expose internal architecture to end users:
1. **Reconnaissance Risks (CWE-200):** Exposing backend platforms (`Cloudflare Pages`, `AWS S3`, `D1`), hostnames (`coophpr.thaiddns.com`), internal ports, or database errors (`ORA-00942`, `Duplicate entry for PRIMARY`) gives attackers valuable reconnaissance insight.
2. **Developer Jargon & Cognitive Load:** Displaying database enums (`READY_FOR_TARGET_SETTER`), HTTP status codes (`HTTP 200`, `403`), third-party SDKs (`LINE LIFF`, `LIFF ID`), concurrency settings (`Parallel Workers: 4`, `Min Gap: 0.35s`), or mathematical terms (`Modulo 11`) confuses non-technical frontline staff (e.g., healthcare workers, cooperative members, administrative personnel).
3. **Third-Party SDK Exposure:** Leaking developer terms like `LINE LIFF` or `LIFF App` confuses everyday users who only know "LINE", while exposing LIFF IDs aids attacker reconnaissance.
4. **Dead-End Errors:** Generic messages like "An error occurred" or "No data found" leave users stranded without next steps.

This skill equips AI agents (Antigravity, Gemini CLI, Claude Code, Codex) with the domain rules, conversion dictionaries, and audit methodology proven on real-world systems like **PhosCCM** (Healthcare Campaign Manager) and **PhosCoop** (Hospital Cooperative Portal).

---

## 🎯 The 4 Golden Pillars

```
+------------------------------------------------------------------------------+
|                         FOUR GOLDEN PILLARS OF NON-TECH COPY                 |
+------------------------------------------------------------------------------+
| 1. Security by Sanitization  | Never expose architecture, hostnames, ports,   |
|                              | enums, or stack traces (Zero Reconnaissance). |
+------------------------------+-----------------------------------------------+
| 2. Zero-Manual Usability     | Labels and buttons explain the real-world     |
|                              | outcome, not the internal software mechanism.  |
+------------------------------+-----------------------------------------------+
| 3. Mandatory 3-Part Errors   | (1) What happened + (2) Why it happened +     |
|                              | (3) Actionable remedy with exact contact/path. |
+------------------------------+-----------------------------------------------+
| 4. Poka-Yoke Guidance        | Clear helper text, format examples, and safe   |
|                              | defaults to prevent user mistakes before click.|
+------------------------------------------------------------------------------+
```

---

## 🚫 The Strictly Prohibited Technical Terms

The following backend and infrastructure terms **must never appear** in user screens, error toasts, activity logs, or public footers:

| Forbidden Term | Where It Typically Leaks | Safe Non-Technical Alternative |
|---|---|---|
| **`Cloudflare`** | `Deploy บน Cloudflare Pages`, `Cloudflare Ray ID` | Remove from UI; log Ray ID internally |
| **`R2`** | `R2 bucket upload failed`, `R2 quota exceeded` | "ไม่สามารถบันทึกไฟล์ได้ในขณะนี้ กรุณาลองใหม่อีกครั้ง" |
| **`D1`** | `D1_ERROR: stepping`, `Cloudflare D1 query failed` | "ระบบฐานข้อมูลขัดข้องชั่วคราว เจ้าหน้าที่ได้รับแจ้งแล้ว" |
| **`MySQL`** | `MySQL server has gone away`, `MySQL Error 1062` | "ข้อมูลนี้มีอยู่ในระบบแล้ว" / "ไม่สามารถเชื่อมต่อฐานข้อมูลได้" |
| **`JWT`** | `JWT token expired`, `Invalid JWT signature` | "เซสชันหมดอายุเพื่อความปลอดภัย กรุณาเข้าสู่ระบบใหม่อีกครั้ง" |
| **`LINE LIFF`** | `เข้าผ่าน LINE LIFF`, `LIFF ID ไม่ถูกต้อง` | "เปิดผ่านแอปพลิเคชัน LINE" / "เข้าใช้งานด้วย LINE" |

---

## 🚀 Installation & Setup

### Option 1: For Antigravity & Agent CLI
Clone directly into your user agent skills directory:
```bash
git clone https://github.com/songwutk/skill-nontech-title ~/.agents/skills/nontech-title
```

### Option 2: For Google Gemini CLI
Clone or symlink into Gemini config skills directory:
```bash
git clone https://github.com/songwutk/skill-nontech-title ~/.gemini/config/skills/nontech-title
```

### Option 3: For Claude Code
Clone or symlink into Claude skills directory:
```bash
git clone https://github.com/songwutk/skill-nontech-title ~/.claude/skills/nontech-title
```

---

## 📖 How to Use

The skill triggers automatically when you ask your AI assistant to review, audit, or write UI/UX copy, or you can invoke it explicitly:

```text
"ช่วย audit คำศัพท์และข้อความในไฟล์ frontend/index.html ให้เป็นภาษาที่คน non-tech เข้าใจง่าย และตัดคำที่มีความเสี่ยงด้านความปลอดภัยออก"
```
```text
"รีวิวปุ่มและสถานะในฟอร์มนี้ตามมาตรฐาน nontech-title skill หน่อย"
```
```text
"ช่วยเขียน error toast สำหรับกรณีอัปโหลดรูปไม่สำเร็จ โดยใช้กฎข้อความผิดพลาด 3 ส่วน"
```

---

## 📂 Repository Structure

```
skill-nontech-title/
├── SKILL.md                          # Core Agent Skill specification
├── README.md                         # Project overview and installation guide
├── LICENSE                           # MIT License
├── dictionary/
│   ├── dev-to-human-th.md            # Comprehensive Thai vocabulary dictionary (Dev -> Plain Thai)
│   └── security-leak-indicators.md   # Security vulnerability guide & reconnaissance indicator catalog
├── rules/
│   ├── three-part-error-formula.md   # Guide and templates for the mandatory 3-part error formula
│   └── poka-yoke-microcopy.md        # Mistake-proofing rules for labels, inputs, and validation
└── examples/
    ├── before-after-showcase.md      # Side-by-side component comparisons (HTML, React, Vue)
    ├── hospital-ccm.md               # Real-world case study: Hospital campaign manager (PhosCCM)
    └── cooperative-portal.md         # Real-world case study: Cooperative member portal (PhosCoop)
```

---

## 📜 License

MIT License © 2026 [Songwut K.](https://github.com/songwutk)
