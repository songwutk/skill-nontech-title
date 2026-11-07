# คู่มือตัวชี้วัดความเสี่ยงด้านความปลอดภัยในข้อความหน้าจอ (Security Leak Indicators in UI Copy)

> **CWE-200: Exposure of Sensitive Information to an Unauthorized Actor**  
> ข้อความบนหน้าจอผู้ใช้ (UI Copy) ไม่ใช่แค่เรื่องของความสวยงามหรือความเข้าใจง่าย แต่เป็น **ด่านหน้าสุดของความปลอดภัยสารสนเทศ (Security by Sanitization)**

เมื่อนักพัฒนาปล่อยให้ชื่อเซิร์ฟเวอร์, หมายเลขพอร์ต, โครงสร้างฐานข้อมูล, หรือชื่อบริการคลาวด์ ปรากฏอยู่บนหน้าจอ ผู้ไม่หวังดี (Attacker) สามารถใช้ข้อมูลเหล่านี้ในการ **ลาดตระเวน (Reconnaissance & OSINT)** เพื่อระบุช่องโหว่ของระบบและวางแผนการโจมตีได้อย่างแม่นยำ

---

## 1. หมวดหมู่ข้อมูลรั่วไหลที่มีความเสี่ยงสูง (High-Risk Leak Categories)

### 1.1 ข้อมูลโฮสต์และเครือข่ายภายใน (Internal Hostnames & Network Addresses)
- **ตัวอย่างข้อความที่รั่วไหล:**
  - `เซิร์ฟเวอร์: coophpr.thaiddns.com`
  - `Connected to: 192.168.1.120:8080`
  - `API Endpoint: https://api-internal.hospital.local/v2/campaigns`
- **ความเสี่ยง:** ชี้เป้าโดเมน DDNS, IP ภายใน, และพอร์ตบริการ ทำให้ผู้โจมตีทราบเป้าหมายที่แท้จริงแม้ระบบจะอยู่หลัง Reverse Proxy หรือ CDN
- **แนวทางแก้ไข:** ลบชื่อโฮสต์และพอร์ตออกจากหน้าจอทั้งหมด หากต้องการแสดงสถานะ ให้ใช้ข้อความเช่น `"สถานะ: เชื่อมต่อปกติ"` เท่านั้น

### 1.2 โครงสร้างและเทคโนโลยีคลาวด์ (Cloud Platforms & Infrastructure: Cloudflare, R2)
- **ตัวอย่างข้อความที่รั่วไหล:**
  - `Deploy บน Cloudflare Pages`
  - `Cloudflare Ray ID: 88219afbc01`
  - `File stored in Cloudflare R2 Bucket: hospital-patients-prod`
  - `Upload error: R2 bucket quota exceeded or S3 connection timeout`
  - `Processed by Worker: cf-worker-phosccm-prod-02`
- **ความเสี่ยง:** เปิดเผยสถาปัตยกรรมคลาวด์ (Cloud Architecture Fingerprinting) ทำให้ผู้โจมตีสามารถค้นหาช่องโหว่เฉพาะแพลตฟอร์ม เช่น R2 bucket enumeration หรือวิเคราะห์ edge worker routing
- **แนวทางแก้ไข:** ห้ามระบุคำว่า `Cloudflare`, `R2` หรือชื่อ Bucket บนหน้าจอเด็ดขาด

### 1.3 โครงสร้างฐานข้อมูลและข้อผิดพลาด SQL (Database Schema & SQL Leaks: D1, MySQL)
- **ตัวอย่างข้อความที่รั่วไหล:**
  - `D1_ERROR: stepping, table members has no column named cid_hash`
  - `Cloudflare D1 batch execution failed: D1_TYPE_ERROR`
  - `MySQL Error 1062: Duplicate entry '3540100...' for key 'members.PRIMARY'`
  - `MySQL server has gone away`
  - `ORA-00942: table or view does not exist`
  - `PostgreSQL foreign key constraint "fk_campaign_author" failed`
- **ความเสี่ยง:** เปิดเผยชื่อฐานข้อมูล (`D1`, `MySQL`), ชื่อตาราง (Table), และโครงสร้างคอลัมน์ (Column) เป็นข้อมูลชั้นยอดสำหรับทำ SQL Injection
- **แนวทางแก้ไข:** ดักจับ Exception ใน Backend และส่งเฉพาะรหัสอ้างอิงข้อผิดพลาด (Trace ID / Reference ID) มายังหน้าบ้าน เช่น:
  > *"ไม่สามารถบันทึกข้อมูลได้ เนื่องจากข้อมูลนี้มีอยู่ในระบบแล้ว (รหัสอ้างอิง: ERR-8821)"*

### 1.4 กลไกและขั้นตอนวิธีทางความปลอดภัย (Cryptographic & Algorithmic Details: JWT, Modulo 11)
- **ตัวอย่างข้อความที่รั่วไหล:**
  - `JWT expired: Token expiration date is 2026-11-07T12:00:00Z`
  - `Invalid JWT signature (JsonWebTokenError)`
  - `Malformed JWT: Bearer token is missing payload segment`
  - `เลขประจำตัวประชาชน 13 หลักไม่ถูกต้องตามหลัก Modulo 11`
  - `Password validation failed: Bcrypt hash mismatch`
- **ความเสี่ยง:** บอกให้ผู้โจมตีรู้ว่าระบบใช้โทเคน `JWT` หรือขั้นตอนวิธีใดในการตรวจสอบ ช่วยลดขอบเขตการคำนวณในการทำ Brute-force หรือโจมตีช่องโหว่ของอัลกอริทึม
- **แนวทางแก้ไข:** ใช้ภาษาทั่วไปที่ไม่ระบุชื่ออัลกอริทึม:
  > *"เซสชันการใช้งานหมดอายุเพื่อความปลอดภัย — กรุณาเข้าสู่ระบบใหม่อีกครั้ง"* หรือ *"เลขบัตรประชาชนไม่ถูกต้อง — กรุณาตรวจตัวเลขอีกครั้ง"*

### 1.5 ข้อมูลการทดสอบและสิทธิ์ผู้ดูแลระบบ (Demo Credentials & Test Artefacts)
- **ตัวอย่างข้อความที่รั่วไหล:**
  - `บัญชีทดสอบ: admin / pass1234 (สำหรับทดสอบในโรงพยาบาล)`
  - `[DEV MODE] Bypass OTP verification enabled`
  - `Link to GitHub Repository: https://github.com/org/hospital-core`
- **ความเสี่ยง:** บัญชีทดสอบที่ลืมเอาออกในหน้า Production มักเป็นช่องทางที่ถูกเจาะบ่อยที่สุด ส่วนลิงก์ Repo อาจนำไปสู่โค้ดต้นฉบับหรือประวัติ Commit ที่มีกุญแจลับ
- **แนวทางแก้ไข:** ซ่อนปุ่มสลับโหมดและข้อมูลทดสอบด้วย Environment Flag (`NODE_ENV === 'production'`) และตัดลิงก์ Git Repository ออกจากหน้าจอผู้ใช้

### 1.6 เฟรมเวิร์กเชื่อมต่อภายนอกและ Client SDK (Third-Party SDKs & LINE LIFF Leaks)
- **ตัวอย่างข้อความที่รั่วไหล:**
  - `LINE LIFF App Error: Invalid liffId (2007280264-xxxx)`
  - `กรุณาเปิดผ่าน LINE LIFF เท่านั้น`
  - `ช่าง... กดรับเรื่องผ่าน LINE LIFF`
  - `Webhook failed: Invalid LINE Channel Secret or Access Token`
- **ความเสี่ยง:** การเปิดเผยชื่อเฟรมเวิร์กและ LIFF ID หรือ Channel Secret เปิดโอกาสให้ผู้ไม่ประสงค์ดีแกะรอย (Trace) ไปยังบัญชีผู้พัฒนา LINE Developers, ตรวจสอบ Scope สิทธิ์ที่ขอจากผู้ใช้, หรือจำลองการส่งข้อมูล (Spoofing) เข้าสู่ระบบ
- **แนวทางแก้ไข:** ตัดคำว่า "LIFF" ออกจากทุกส่วนของหน้าจอและ Activity Log โดยใช้คำว่า "LINE" หรือ "แอปพลิเคชัน LINE" แทน และซ่อน Client ID / Secret ไว้ในไฟล์คอนฟิกหลังบ้านเสมอ

---

## 2. เช็กลิสต์ตรวจจับความเสี่ยง (Audit Regex & Keywords)

เมื่อตรวจโค้ดหน้าบ้าน (HTML / JS / TSX / Vue) ให้ใช้ Regex สแกนหาคำศัพท์ต้องห้ามเหล่านี้:

```bash
# 🎯 คำสั่งสแกนด่วนหา 5 ศัพท์เทคนิคต้องห้ามหลัก (Cloudflare, R2, D1, MySQL, JWT)
grep -riE "(cloudflare|r2_bucket|\br2\b|d1_error|\bd1\b|mysql|jwt)" src/

# สแกนหาชื่อแพลตฟอร์มโครงสร้างพื้นฐาน
grep -riE "(cloudflare|aws|s3|azure|gcp|docker|nginx|apache|redis|memcached|d1|r2)" src/

# สแกนหารูปแบบรหัสสถานะ HTTP บนหน้าจอผู้ใช้
grep -riE "(HTTP\s*[2-5][0-9]{2}|Error\s*[2-5][0-9]{2}|Status:\s*[2-5][0-9]{2})" src/

# สแกนหาชื่อฐานข้อมูล ตาราง หรือข้อผิดพลาด SQL
grep -riE "(SELECT|INSERT|UPDATE|DELETE|FROM|WHERE|PRIMARY\s+KEY|Foreign\s+Key|ORA-|pg_|sqlite|mysql|d1_error)" src/

# สแกนหาชื่ออัลกอริทึมและศัพท์คณิตศาสตร์
grep -riE "(modulo\s*11|sha256|md5|bcrypt|jwt|hmac|base64)" src/

# สแกนหาเฟรมเวิร์กภายนอกและ SDK (LINE LIFF, Webhook)
grep -riE "(liff|liffId|line_liff|liff\.init|webhook|channel_secret)" src/

# สแกนหาหมายเลข IP พอร์ต หรือโดเมนชี้เป้า
grep -riE "([0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}|thaiddns|ddns|localhost:[0-9]+|:8080|:3000|:5000)" src/
```

---

## 3. ตัวอย่างการแปลงข้อความเพื่อความปลอดภัย (Before vs After)

| บริบท (Context) | ข้อความเดิมที่มีความเสี่ยง | ข้อความใหม่ที่ปลอดภัย | ผลลัพธ์ด้านความปลอดภัย |
|---|---|---|---|
| **Cloudflare** | `Deploy บน Cloudflare Pages (Ray ID: 88219afbc01)` | *(ตัดออกจากหน้าจอทั้งหมด)* | ปิดบัง CDN/WAF และ Ray ID |
| **R2** | `Upload failed: R2 bucket quota exceeded` | `ระบบพื้นที่จัดเก็บเอกสารขัดข้องชั่วคราว กรุณาติดต่อผู้ดูแลระบบ (โทร. 1234)` | ปิดบังชื่อ Cloudflare R2 |
| **D1** | `D1_ERROR: query execution failed in members table` | `ไม่สามารถดึงข้อมูลสมาชิกได้ในขณะนี้ กรุณาลองใหม่อีกครั้ง` | ปิดบัง Cloudflare D1 และชื่อ Table |
| **MySQL** | `MySQL Error 1062: Duplicate entry for PRIMARY` | `ข้อมูลนี้มีอยู่ในระบบแล้ว (เลขบัตร/รหัสซ้ำ)` | ปิดบังเอนจิน MySQL และ Error code |
| **JWT** | `Error: JWT expired or Invalid JWT signature` | `เซสชันหมดอายุเพื่อความปลอดภัย กรุณาเข้าสู่ระบบใหม่อีกครั้ง` | ปิดบังโปรโตคอล JWT และโครงสร้างโทเคน |
| ข้อผิดพลาด API | `Fetch failed: 502 Bad Gateway to api.coop.internal:8443` | `ไม่สามารถเชื่อมต่อระบบได้ชั่วคราว กรุณาลองใหม่อีกครั้ง` | ปิดบัง Internal IP, Domain, Port |
| รหัสผ่านผิด | `Incorrect password for user 'songwut' (Bcrypt hash check failed)` | `ชื่อผู้ใช้หรือรหัสผ่านไม่ถูกต้อง` | ป้องกัน User Enumeration และปิดบัง Algorithm |
| บริการ LINE | `กรุณาเปิดผ่าน LINE LIFF (LIFF ID: 2007280264-xxxx)` | `กรุณาเปิดผ่านแอปพลิเคชัน LINE` | ปิดบัง LIFF ID และไม่ใช้ศัพท์ SDK ที่คนทั่วไปไม่เข้าใจ |
