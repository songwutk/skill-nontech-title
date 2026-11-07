---
name: nontech-title
description: |
  Audit, sanitize, and rewrite UI/UX copy, page titles, form labels, buttons, badges, empty states, and error alerts to eliminate developer jargon and security-sensitive system leaks, converting them into empathetic, actionable, non-technical plain language (Thai & English).
  Use when:
  - Auditing UI/UX screens, frontend templates, forms, modals, or toasts for non-technical users (hospital staff, cooperative members, general public).
  - Removing developer jargon, backend enums, HTTP status codes, DB errors, stack traces, and infrastructure platforms from user screens.
  - Eliminating third-party developer frameworks and SDK terminology (e.g. "LINE LIFF", "LIFF ID", "Webhook") from public views and timelines.
  - Sanitizing copy to prevent system reconnaissance, info leakage, and internal architecture exposure.
  - Writing user-facing error messages that adhere to the 3-part rule: (1) เกิดอะไรขึ้น (What happened), (2) เกิดจากอะไร (Why), and (3) ต้องแก้ไขอย่างไรพร้อมทางออก (Actionable remedy).
  - Designing zero-manual empty states and Poka-Yoke helper microcopy that guide users without needing training manuals.
license: MIT
metadata:
  version: "1.2.0"
  author: "Songwut K."
  keywords: ["ui-copy", "microcopy", "ux-writing", "security-sanitization", "plain-thai", "non-tech-ui", "poka-yoke", "line-liff", "liff-sanitization", "cloudflare", "r2", "d1", "mysql", "jwt"]
---

# Non-Tech Title & Microcopy Sanitizer

> **Core Philosophy:** *If a user needs a developer manual or IT training to understand a button, label, status badge, or error alert, the microcopy has failed. If an interface reveals backend tables, ports, cloud hosts, or status codes, the system has leaked.*

`nontech-title` audits, sanitizes, and translates user-facing interfaces from **developer-centric, backend-exposed language** into **empathetic, action-oriented, non-technical plain language**, with special calibration for Thai public sector, healthcare, cooperative, and enterprise systems.

---

## 1. When to Invoke

Activate this skill when:
- **Auditing UI code or templates:** Reviewing HTML, JSX, TSX, Vue, Svelte, Blade, or mobile views for user-facing text.
- **Reviewing PRs / Features:** Ensuring newly added toasts, status badges, form labels, or error dialogs are non-technical.
- **Fixing Error Messages:** Refactoring raw HTTP status codes (`HTTP 200`, `403 Forbidden`, `500`), database errors, or exceptions into human-friendly explanations.
- **Hiding Infrastructure:** Scrubbing platform names (`Cloudflare`, `D1`, `R2`, `AWS`, `S3`, `Nginx`, `Redis`, `Docker`), hostnames, ports, and internal endpoints from the UI.
- **Translating State Enums:** Converting database state constants (`PENDING_APPROVAL`, `READY_FOR_TARGET_SETTER`, `FAILED_RETRY`) into human progress steps.
- **Designing Empty States:** Replacing bland "ไม่พบข้อมูล" (No data found) with actionable guidance.

---

## 2. The 4 Golden Pillars

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

### Pillar 1: Security by Sanitization (Zero Reconnaissance)
Technical terms leak reconnaissance data to potential attackers and induce anxiety in everyday users:
- ❌ **Forbidden:** Mentioning server hosts (`coophpr.thaiddns.com`), cloud vendors (`Cloudflare Pages`, `AWS S3`), database names (`D1`, `PostgreSQL`), internal ports (`:8080`), framework tokens, third-party SDKs (`LINE LIFF`, `LIFF ID`, `Webhook`), or crypto algorithms (`Modulo 11`, `SHA256`).
- ❌ **Forbidden:** Showing raw HTTP status codes (`HTTP 200`, `404`, `500`) or database error dumps (`ORA-00942`, `Duplicate entry 'xxx' for key 'PRIMARY'`).
- ✅ **Sanitized:** State what the system accomplished or what human action is required, keeping all technical telemetry in internal logs only (e.g. use "เปิดผ่านแอป LINE" instead of "เปิดผ่าน LINE LIFF").

#### 🚨 5 คำศัพท์เทคนิคต้องห้ามเด็ดขาดบนหน้าจอผู้ใช้ (The Big 5 Strictly Prohibited Terms)
1. **`Cloudflare`** — ห้ามแสดงชื่อโฮสต์/CDN/WAF เช่น `Deploy บน Cloudflare Pages`, `Cloudflare Ray ID`, หรือ Cloudflare 5xx Error หน้าบ้าน
2. **`R2`** — ห้ามแสดงชื่อ Cloudflare R2 Object Storage, Bucket URL, หรือ `Upload to R2 failed`
3. **`D1`** — ห้ามแสดงชื่อ Cloudflare D1 Database, `D1_ERROR`, หรือข้อผิดพลาดการคิวรีฐานข้อมูล D1
4. **`MySQL`** — ห้ามแสดงชื่อเอนจินฐานข้อมูล, ตาราง, หรือข้อผิดพลาด `MySQL server has gone away`, `MySQL Error 1062`
5. **`JWT`** — ห้ามแสดงคำว่า `JWT`, `JWT Token expired`, `Invalid JWT signature`, หรือ `Bearer token` ต่อหน้าผู้ใช้ทั่วไป

### Pillar 2: Zero-Manual Usability (Human Outcome Over Mechanism)
Users do not care about the system's internal processing pipeline; they care about their real-world task:
- ❌ **Mechanism:** "ส่งต่องานไปยังผู้กำหนดเป้าหมาย" (Forwarding task to target setter system entity)
- ✅ **Human Outcome:** "ส่งต่อให้ผู้กำหนดเป้าหมายเลือกผู้รับ" (Forwarding so the goal setter can choose recipients)
- ❌ **Mechanism:** "Min Gap: 0.35s", "Parallel Workers: 4 Workers"
- ✅ **Human Outcome:** Remove throttle controls from non-technical users; default safely to "ความเร็วในการส่ง: ปกติ (แนะนำ)"

### Pillar 3: The Mandatory 3-Part Error Formula
Every error toast, banner, or dialog **must** have all three parts:
```
[1. เกิดอะไรขึ้น]  +  [2. เกิดจากอะไร]  +  [3. ต้องแก้ไขอย่างไร / ติดต่อใคร]
 (What happened)        (Why it happened)        (Actionable remedy & next step)
```
- ❌ **Bad:** "เกิดข้อผิดพลาดในการเชื่อมต่อ (HTTP 500)"
- ❌ **Bad:** "ไม่พบบัญชีผู้ใช้งานในระบบ"
- ✅ **Good:** "ไม่พบบัญชีนี้ในระบบ รพ.แพร่ — เนื่องจากยังไม่ได้ลงทะเบียนผู้ใช้งาน กรุณาตรวจเลขบัตรประชาชน หรือติดต่อฝ่าย IT โทร. 1234"

### Pillar 4: Poka-Yoke & Actionable Empty States
- **Empty States:** Never leave a screen blank or say just "ไม่มีข้อมูล". State why it is empty, how long it takes to create the first record, and provide a direct primary action button:
  - *Example:* "ยังไม่มีโครงการส่งข้อความ… เริ่มสร้างโครงการแรกได้ทันที (ใช้เวลาประมาณ 3 นาที) [ + สร้างโครงการใหม่ ]"
- **Poka-Yoke Hints:** Helper texts must show real examples and safe defaults before the user submits:
  - *Example:* "ตัวอย่าง: กลุ่มผู้ป่วยเบาหวาน คลินิกวันพุธ (ความยาวไม่เกิน 50 ตัวอักษร)"

---

## 3. Quick Conversion Matrix

| หมวดหมู่ (Category) | คำศัพท์ทางเทคนิค / ความเสี่ยง (Technical / Risky) | คำศัพท์สำหรับผู้ใช้งานทั่วไป (Non-Tech Plain Thai) | เหตุผลและหลักการ (Rationale) |
|---|---|---|---|
| **สถานะระบบ (Status Enums)** | `READY_FOR_TARGET_SETTER` | **ส่งต่อแล้ว — ขั้นต่อไปเป็นของผู้กำหนดเป้าหมาย** | ตัด Enum ออก ชี้ชัดว่าใครเป็นผู้รับผิดชอบขั้นต่อไป |
| | `PENDING_EXECUTIVE_APPROVAL` | **รอผู้อำนวยการอนุมัติ** | ใช้ตำแหน่งบุคคลจริงแทนชื่อ State ทางเทคนิค |
| | `DRAFT` | **ฉบับร่าง (ยังไม่ส่ง)** | มีคำอธิบายกำกับ ไม่ทับศัพท์ลอย ๆ |
| | `FAILED_RETRY` | **ส่งไม่สำเร็จ — ระบบจะลองใหม่อัตโนมัติ** | อธิบายพฤติกรรมระบบให้ผู้ใช้อุ่นใจ |
| **ผลลัพธ์ (Responses)** | `ส่งข้อมูลสำเร็จ (HTTP 200)` | **ส่งข้อความทดสอบเข้ามือถือของคุณเรียบร้อยแล้ว** | ผู้ใช้ต้องการรู้ผลลัพธ์จริง ไม่ใช่ HTTP code |
| | `Error 403: Forbidden` | **คุณไม่มีสิทธิ์เข้าถึงหน้านี้ กรุณาติดต่อหัวหน้างาน** | ปิดโค้ดความปลอดภัย ให้แนวทางแก้ไข |
| | `500 Internal Server Error` | **ระบบขัดข้องชั่วคราว เจ้าหน้าที่ได้รับแจ้งแล้ว กรุณาลองใหม่ใน 5 นาที** | สื่อสารอย่างสุภาพ ไม่เปิดเผย Backend Crash |
| **การตรวจสอบ (Validation)** | `เลข CID ไม่ถูกต้องตาม Modulo 11` | **เลขบัตรประชาชนไม่ถูกต้อง กรุณาตรวจตัวเลขอีกครั้ง** | ลบชื่ออัลกอริทึมคณิตศาสตร์ออก |
| | `Regex format mismatch` | **รูปแบบอีเมลไม่ถูกต้อง (ตัวอย่าง: name@hospital.go.th)** | ให้ตัวอย่างที่ถูกต้องแทนศัพท์นิพจน์เรกิวลาร์ |
| | `Payload too large (>5MB)` | **ไฟล์มีขนาดเกิน 5 MB กรุณาเลือกไฟล์ที่ย่อขนาดแล้ว** | แปลงศัพท์ Payload เป็นคำว่า "ไฟล์" |
| **ประสิทธิภาพ (Concurrency)** | `จำนวน Parallel Workers: 4 Workers` | **ความเร็วในการส่ง: ปกติ (แนะนำ)** | แปลงจำนวนเธรดเป็นระดับความเร็วที่เข้าใจง่าย |
| | `Min Gap: 0.35s` | *(ตัดออกจากหน้าจอ ใช้ค่ามาตรฐานระบบ)* | พารามิเตอร์ภายในไม่ควรให้ผู้ใช้ปรับแต่งเอง |
| | `Ping Latency: 42ms` | **สถานะเครือข่าย: เชื่อมต่อปกติ** | แปลงตัวเลขมิลลิวินาทีเป็นสถานะความพร้อม |
| **ข้อมูลระบบ (Infra Leaks)** | `Cloudflare` (`Deploy บน Cloudflare Pages`) | *(ตัดออกทั้งหมด)* | ป้องกัน Reconnaissance ของโครงสร้างคลาวด์ |
| | `R2` (`Upload failed: R2 bucket error`) | **ไม่สามารถบันทึกไฟล์ได้ในขณะนี้ กรุณาลองใหม่อีกครั้ง** | ตัดชื่อบริการจัดเก็บไฟล์ Cloudflare R2 |
| | `D1` (`D1_ERROR: query syntax error`) | **ระบบฐานข้อมูลขัดข้องชั่วคราว เจ้าหน้าที่กำลังตรวจสอบ** | ตัดชื่อฐานข้อมูล Cloudflare D1 |
| | `MySQL` (`MySQL server has gone away / Error 1062`) | **ข้อมูลนี้มีอยู่ในระบบแล้ว / ไม่สามารถเชื่อมต่อระบบได้ชั่วคราว** | ตัดชื่อเอนจิน MySQL และ Error code |
| | `JWT` (`JWT token expired / signature invalid`) | **เซสชันหมดอายุเพื่อความปลอดภัย กรุณาเข้าสู่ระบบใหม่อีกครั้ง** | ผู้ใช้ไม่รู้จัก JWT แปลงเป็นแจ้งหมดเวลาเซสชัน |
| | `เซิร์ฟเวอร์: coophpr.thaiddns.com` | *(ตัดออกจากหน้าล็อกอิน)* | ป้องกันการชี้เป้าโฮสต์และโดเมนหลังบ้าน |
| | `GitHub Repository` | **ศูนย์ช่วยเหลือและแจ้งปัญหาการใช้งาน** | ไม่นำคำศัพท์ของนักพัฒนามาปนกับหน่วยงาน |
| **เฟรมเวิร์กเชื่อมต่อ (SDKs)** | `เปิดผ่าน LINE LIFF` / `Login ผ่าน LIFF` | **เปิดผ่านแอปพลิเคชัน LINE** / **เข้าสู่ระบบด้วย LINE** | ผู้ใช้รู้จัก "LINE" แต่ไม่รู้จัก "LIFF" ที่เป็นชื่อ SDK |
| | `LIFF ID ไม่ถูกต้อง / LIFF Init Error` | **ไม่สามารถเชื่อมต่อกับ LINE ได้ กรุณาเปิดจากเมนูในแอป LINE อีกครั้ง** | ลบชื่อทางเทคนิค พร้อมบอกวิธีแก้ |
| | `กดรับเรื่องผ่าน LINE LIFF` | **กดรับเรื่องผ่าน LINE** | ลบคำว่า LIFF ใน Activity Log / ไทม์ไลน์งาน |
| **ปุ่มและการกระทำ (Actions)** | `Reset Form` / `Clear Data` | **ล้างข้อมูลที่กรอกทั้งหมด** | ชี้ชัดว่าจะเกิดอะไรขึ้นกับข้อมูล |
| | `Submit` / `Execute Batch` | **ส่งข้อมูล** / **เริ่มการส่งข้อความ** | ใช้คำกริยาภาษาไทยที่สื่อความหมายชัดเจน |
| | `Download (PDF) (1.2MB)` | **ดาวน์โหลดเอกสาร (1.2 MB)** | ไม่ต้องใส่วงเล็บซ้ำซ้อน สื่อสารให้ตรงประเด็น |

---

## 4. Step-by-Step UI Audit Workflow

When asked to audit a screen, component, or file, follow this 4-step workflow:

```
Step 1: Scan for Security Leaks
  ├── Platform & Infrastructure names (AWS, Cloudflare, Docker, D1, R2, Nginx)
  ├── Hostnames, internal IPs, port numbers (:3000, :8080, thaiddns.com)
  └── Raw HTTP status codes, SQL queries, database column names, and stack traces

Step 2: Scrub Developer Jargon & Enums
  ├── Backend database constants (ALL_CAPS_ENUMS, boolean flags)
  ├── Third-party SDK & framework names (LINE LIFF, LIFF ID, Webhook, API Client)
  ├── Concurrency & algorithmic terms (Workers, Threads, Modulo 11, Regex)
  └── Developer commands (Execute, Fetch, Payload, Cache, Callback)

Step 3: Enforce the 3-Part Error Rule
  ├── Ensure every toast, error text, and alert contains:
  │   1. What happened
  │   2. Why it happened
  │   3. Actionable next step / direct support contact
  └── Replace raw browser prompt() and alert() with accessible UI dialogs

Step 4: Verify Poka-Yoke & Accessibility
  ├── Ensure form fields have descriptive labels, placeholders, and helper text
  ├── Verify empty states provide educational copy + primary call-to-action
  └── Confirm status colors are always accompanied by descriptive text
```

---

## 5. Repository Documentation Reference

For in-depth guides, specialized dictionaries, and real-system case studies, consult:
- [`dictionary/dev-to-human-th.md`](file:///home/songwut/skill-nontech-title/dictionary/dev-to-human-th.md) — Comprehensive technical vocabulary replacement table (Thai).
- [`dictionary/security-leak-indicators.md`](file:///home/songwut/skill-nontech-title/dictionary/security-leak-indicators.md) — Security risks of UI copy and reconnaissance indicator catalog.
- [`rules/three-part-error-formula.md`](file:///home/songwut/skill-nontech-title/rules/three-part-error-formula.md) — The 3-part error messaging standard with before/after templates.
- [`rules/poka-yoke-microcopy.md`](file:///home/songwut/skill-nontech-title/rules/poka-yoke-microcopy.md) — Error-prevention copy rules for forms, buttons, and badges.
- [`examples/before-after-showcase.md`](file:///home/songwut/skill-nontech-title/examples/before-after-showcase.md) — Side-by-side UI component comparison.
- [`examples/hospital-ccm.md`](file:///home/songwut/skill-nontech-title/examples/hospital-ccm.md) — PhosCCM healthcare campaign case study.
- [`examples/cooperative-portal.md`](file:///home/songwut/skill-nontech-title/examples/cooperative-portal.md) — PhosCoop financial portal case study.
