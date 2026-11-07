# ตัวอย่างการเปรียบเทียบโค้ดคอมโพเนนต์ ก่อนและหลังปรับปรุง (Before & After Component Showcase)

รวมตัวอย่างการแก้ไขโค้ดจริงในระดับคอมโพเนนต์ (HTML, JSX, Vue) เพื่อแสดงการนำหลักการ **Non-Tech Microcopy & Sanitization** ไปใช้งาน

---

## 1. การตรวจความถูกต้องของฟอร์ม (Form Validation & Helper Text)

### ❌ ก่อนปรับปรุง (Developer-Centric & Jargon)
```html
<div class="form-group">
  <label>CID Number</label>
  <input type="text" id="cid" placeholder="เลข 13 หลัก" />
  <!-- ข้อความผิดพลาดเมื่อกดส่ง -->
  <span class="error-msg">CID 13 digits Modulo 11 check failed</span>
</div>
```

### ✅ หลังปรับปรุง (Non-Tech & Poka-Yoke)
```html
<div class="field">
  <label for="cid-input">เลขประจำตัวประชาชน <span class="required" aria-hidden="true">*</span></label>
  <input 
    type="text" 
    id="cid-input" 
    inputmode="numeric" 
    maxlength="17" 
    aria-describedby="cid-hint cid-error"
  />
  <p id="cid-hint" class="helper-text">
    กรอกตัวเลข 13 หลัก (ระบบจะเว้นวรรคให้อัตโนมัติ เช่น 1 5401 00012 34 5)
  </p>
  <!-- แสดงเมื่อตรวจพบข้อผิดพลาดตามสูตร 3 ส่วน -->
  <p id="cid-error" class="error-text" role="alert">
    เลขบัตรประชาชนไม่ถูกต้อง — กรุณาตรวจตัวเลขอีกครั้ง (ระบบช่วยตรวจหลักสุดท้ายให้แล้ว)
  </p>
</div>
```

---

## 2. ป้ายสถานะเอกสาร (Status Badges)

### ❌ ก่อนปรับปรุง (Raw Backend Enums)
```jsx
// React component showing database enum
function CampaignStatus({ status }) {
  return (
    <span className={`badge badge-${status.toLowerCase()}`}>
      {status} {/* แสดงผล: READY_FOR_TARGET_SETTER, PENDING_APPROVAL */}
    </span>
  );
}
```

### ✅ หลังปรับปรุง (Human Progress & Next Step)
```jsx
// React component showing human-centric state
const STATUS_LABELS = {
  DRAFT: { label: 'ฉบับร่าง (ยังไม่ส่ง)', variant: 'neutral' },
  READY_FOR_TARGET_SETTER: { label: 'ส่งต่อแล้ว — รอเลือกผู้รับ', variant: 'info' },
  PENDING_EXECUTIVE_APPROVAL: { label: 'รอผู้อำนวยการอนุมัติ', variant: 'warning' },
  APPROVED: { label: 'อนุมัติแล้ว — พร้อมส่ง', variant: 'success' },
  REJECTED: { label: 'ส่งกลับแก้ไข (มีข้อเสนอแนะ)', variant: 'danger' }
};

function CampaignStatus({ status }) {
  const current = STATUS_LABELS[status] || { label: 'อยู่ระหว่างดำเนินการ', variant: 'neutral' };
  return (
    <span className={`badge badge-${current.variant}`}>
      <span className="dot" aria-hidden="true" />
      {current.label}
    </span>
  );
}
```

---

## 3. การแจ้งเตือนข้อผิดพลาดเครือข่าย (Error Toasts & Banners)

### ❌ ก่อนปรับปรุง (Exposing HTTP & Platform)
```javascript
// JavaScript Toast displaying API error directly
fetch('/api/v1/broadcast', { method: 'POST', body: data })
  .catch(err => {
    toast.error(`Error 500: Internal Server Error (${err.message})`);
  });
```

### ✅ หลังปรับปรุง (3-Part Formula & Friendly Tone)
```javascript
// JavaScript Toast adhering to the 3-part rule
fetch('/api/v1/broadcast', { method: 'POST', body: data })
  .catch(err => {
    // บันทึกรายละเอียดเชิงเทคนิคไว้ใน Console/Telemetry สำหรับทีมพัฒนา
    console.error('[API Failure Trace]', err);

    // แสดงข้อความที่สุภาพและให้ทางออกแก่ผู้ใช้งาน
    toast.error({
      title: 'ไม่สามารถส่งข้อความได้ในขณะนี้',
      message: 'ระบบเครือข่ายขัดข้องชั่วคราว กรุณารอสักครู่แล้วกดลองใหม่อีกครั้ง หรือติดต่อผู้ดูแลระบบ โทร. 1234',
      duration: 6000
    });
  });
```

---

## 4. คอมโพเนนต์หน้าจอว่างเปล่า (Empty States)

### ❌ ก่อนปรับปรุง (Bland & Empty)
```html
<div class="empty-container">
  <p>ไม่พบข้อมูลในระบบ</p>
</div>
```

### ✅ หลังปรับปรุง (Educational & Action-Oriented)
```html
<div class="empty-state-card" role="region" aria-label="ยังไม่มีรายการ">
  <div class="empty-icon" aria-hidden="true">
    <svg width="48" height="48" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.5">
      <path d="M12 4v16m8-8H4" stroke-linecap="round"/>
    </svg>
  </div>
  <h3 class="empty-title">ยังไม่มีโครงการส่งข้อความในระบบ</h3>
  <p class="empty-description">
    เริ่มต้นสร้างโครงการแรกเพื่อส่งข้อความแจ้งเตือนสุขภาพถึงผู้รับบริการผ่าน LINE Official Account (ใช้เวลาประมาณ 3 นาที)
  </p>
  <a href="/campaigns/new" class="btn btn-primary">
    + สร้างโครงการแรกของคุณ
  </a>
</div>
```

---

## 5. การเชื่อมต่อระบบ LINE และการตัดคำว่า "LINE LIFF" ออกจากหน้าจอ (LINE Integration & LIFF Sanitization)

### ❌ ก่อนปรับปรุง (รั่วไหลคำว่า LINE LIFF สู่หน้าจอและบันทึกประวัติ)
```html
<!-- 1. หน้าจอตอนโหลด SDK -->
<div id="loading" class="splash">
  <div class="spinner"></div>
  <p>กำลังเชื่อมต่อระบบ LINE LIFF (LIFF ID: 2007280264-vKuB0oR5)...</p>
</div>

<!-- 2. บันทึกไทม์ไลน์กิจกรรม (Activity Log) -->
<li class="timeline-item">
  <span class="time">10:30 น.</span>
  <span class="actor">ช่างสมหมาย สายตรวจ</span>
  <span class="action">ช่างสมหมาย กดรับเรื่องผ่าน LINE LIFF (ภายใน 10 นาที)</span>
</li>

<!-- 3. แจ้งเตือนเมื่อเปิดนอก LINE -->
<div class="alert alert-warning">
  <p>Error: ไม่รองรับการทำงานนอก LINE LIFF กรุณาเปิดผ่าน LINE LIFF เท่านั้น</p>
</div>
```

### ✅ หลังปรับปรุง (ใช้ภาษาไทยที่สื่อถึง LINE ชัดเจน ปลอดภัย และไม่หลุดศัพท์ SDK)
```html
<!-- 1. หน้าจอตอนโหลด SDK -->
<div id="loading" class="splash">
  <div class="spinner" aria-hidden="true"></div>
  <p>กำลังเปิดบริการผ่าน LINE...</p>
</div>

<!-- 2. บันทึกไทม์ไลน์กิจกรรม (Activity Log) -->
<li class="timeline-item">
  <span class="time">10:30 น.</span>
  <span class="actor">ช่างสมหมาย สายตรวจ</span>
  <span class="action">ช่างสมหมาย กดรับเรื่องผ่านแอป LINE (ภายใน 10 นาที)</span>
</li>

<!-- 3. แจ้งเตือนเมื่อเปิดนอก LINE (ตามสูตร 3 ส่วน) -->
<div class="card alert-card" role="alert">
  <h3>บริการนี้เปิดใช้งานผ่านแอปพลิเคชัน LINE</h3>
  <p>
    เพื่อความปลอดภัยและการยืนยันตัวตน กรุณากดเปิดบริการนี้จากห้องแชทในแอป LINE 
    หรือสแกน QR Code ด้านล่างด้วยโทรศัพท์มือถือของคุณ
  </p>
  <div class="qr-container">
    <img src="/images/line-qr.png" alt="สแกน QR เพื่อเปิดบริการบนแอป LINE" width="180" height="180" />
  </div>
</div>
```
