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

### 1.2 โครงสร้างและเทคโนโลยีคลาวด์ (Cloud Platforms & Infrastructure)
- **ตัวอย่างข้อความที่รั่วไหล:**
  - `Deploy บน Cloudflare Pages / D1 Database`
  - `File stored in AWS S3 Bucket: hospital-patients-prod`
  - `Processed by Worker: cf-worker-phosccm-prod-02`
- **ความเสี่ยง:** เปิดเผยสถาปัตยกรรมคลาวด์ (Cloud Architecture Fingerprinting) ทำให้ผู้โจมตีสามารถค้นหาช่องโหว่เฉพาะแพลตฟอร์ม (Platform-specific exploits หรือ Bucket enumeration)
- **แนวทางแก้ไข:** ห้ามระบุชื่อแบรนด์คลาวด์หรือชื่อ Bucket บนหน้าจอเด็ดขาด

### 1.3 โครงสร้างฐานข้อมูลและข้อผิดพลาด SQL (Database Schema & SQL Leaks)
- **ตัวอย่างข้อความที่รั่วไหล:**
  - `Duplicate entry '3540100...' for key 'members.PRIMARY'`
  - `ORA-00942: table or view does not exist`
  - `PostgreSQL foreign key constraint "fk_campaign_author" failed`
  - `column "cid_hash" of relation "patients" does not exist`
- **ความเสี่ยง:** เปิดเผยชื่อตาราง (Table), ชื่อคอลัมน์ (Column), และชนิดของฐานข้อมูล เป็นข้อมูลชั้นยอดสำหรับทำ SQL Injection
- **แนวทางแก้ไข:** ดักจับ Exception ใน Backend และส่งเฉพาะรหัสอ้างอิงข้อผิดพลาด (Trace ID / Reference ID) มายังหน้าบ้าน เช่น:
  > *"ไม่สามารถบันทึกข้อมูลได้ เนื่องจากข้อมูลนี้มีอยู่ในระบบแล้ว (รหัสอ้างอิง: ERR-8821)"*

### 1.4 กลไกและขั้นตอนวิธีทางความปลอดภัย (Cryptographic & Algorithmic Details)
- **ตัวอย่างข้อความที่รั่วไหล:**
  - `เลขประจำตัวประชาชน 13 หลักไม่ถูกต้องตามหลัก Modulo 11`
  - `Password validation failed: Bcrypt hash mismatch`
  - `Token signature invalid (HMAC-SHA256)`
- **ความเสี่ยง:** บอกให้ผู้โจมตีรู้ว่าระบบใช้ขั้นตอนวิธีใดในการตรวจสอบ ซึ่งช่วยลดขอบเขตการคำนวณในการทำ Brute-force หรือบายพาส
- **แนวทางแก้ไข:** ใช้ภาษาทั่วไปที่ไม่ระบุชื่ออัลกอริทึม:
  > *"เลขบัตรประชาชนไม่ถูกต้อง — กรุณาตรวจตัวเลขอีกครั้ง"*

### 1.5 ข้อมูลการทดสอบและสิทธิ์ผู้ดูแลระบบ (Demo Credentials & Test Artefacts)
- **ตัวอย่างข้อความที่รั่วไหล:**
  - `บัญชีทดสอบ: admin / pass1234 (สำหรับทดสอบในโรงพยาบาล)`
  - `[DEV MODE] Bypass OTP verification enabled`
  - `Link to GitHub Repository: https://github.com/org/hospital-core`
- **ความเสี่ยง:** บัญชีทดสอบที่ลืมเอาออกในหน้า Production มักเป็นช่องทางที่ถูกเจาะบ่อยที่สุด ส่วนลิงก์ Repo อาจนำไปสู่โค้ดต้นฉบับหรือประวัติ Commit ที่มีกุญแจลับ
- **แนวทางแก้ไข:** ซ่อนปุ่มสลับโหมดและข้อมูลทดสอบด้วย Environment Flag (`NODE_ENV === 'production'`) และตัดลิงก์ Git Repository ออกจากหน้าจอผู้ใช้

---

## 2. เช็กลิสต์ตรวจจับความเสี่ยง (Audit Regex & Keywords)

เมื่อตรวจโค้ดหน้าบ้าน (HTML / JS / TSX / Vue) ให้ใช้ Regex สแกนหาคำศัพท์ต้องห้ามเหล่านี้:

```bash
# สแกนหาชื่อแพลตฟอร์มโครงสร้างพื้นฐาน
grep -riE "(cloudflare|aws|s3|azure|gcp|docker|nginx|apache|redis|memcached|d1|r2)" src/

# สแกนหารูปแบบรหัสสถานะ HTTP บนหน้าจอผู้ใช้
grep -riE "(HTTP\s*[2-5][0-9]{2}|Error\s*[2-5][0-9]{2}|Status:\s*[2-5][0-9]{2})" src/

# สแกนหาชื่อฐานข้อมูล ตาราง หรือข้อผิดพลาด SQL
grep -riE "(SELECT|INSERT|UPDATE|DELETE|FROM|WHERE|PRIMARY\s+KEY|Foreign\s+Key|ORA-|pg_|sqlite|mysql)" src/

# สแกนหาชื่ออัลกอริทึมและศัพท์คณิตศาสตร์
grep -riE "(modulo\s*11|sha256|md5|bcrypt|jwt|hmac|base64)" src/

# สแกนหาหมายเลข IP พอร์ต หรือโดเมนชี้เป้า
grep -riE "([0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}|thaiddns|ddns|localhost:[0-9]+|:8080|:3000|:5000)" src/
```

---

## 3. ตัวอย่างการแปลงข้อความเพื่อความปลอดภัย (Before vs After)

| บริบท (Context) | ข้อความเดิมที่มีความเสี่ยง | ข้อความใหม่ที่ปลอดภัย | ผลลัพธ์ด้านความปลอดภัย |
|---|---|---|---|
| ข้อผิดพลาด API | `Fetch failed: 502 Bad Gateway to api.coop.internal:8443` | `ไม่สามารถเชื่อมต่อระบบได้ชั่วคราว กรุณาลองใหม่อีกครั้ง` | ปิดบัง Internal IP, Domain, Port |
| รหัสผ่านผิด | `Incorrect password for user 'songwut' (Bcrypt hash check failed)` | `ชื่อผู้ใช้หรือรหัสผ่านไม่ถูกต้อง` | ป้องกัน User Enumeration และปิดบัง Algorithm |
| อัปโหลดไฟล์ | `Upload failed: R2 bucket quota exceeded or S3 connection timeout` | `ระบบพื้นที่จัดเก็บเต็มชั่วคราว กรุณาติดต่อผู้ดูแลระบบ (โทร. 1234)` | ปิดบังผู้ให้บริการคลาวด์และสถาปัตยกรรม |
| ฐานข้อมูลชนกัน | `IntegrityConstraintViolation: duplicate key value violates unique constraint "users_email_key"` | `อีเมลนี้ถูกใช้งานในระบบแล้ว กรุณาใช้อีเมลอื่นหรือกดลืมรหัสผ่าน` | ปิดบังชื่อ Schema, Constraint, และฐานข้อมูล |
