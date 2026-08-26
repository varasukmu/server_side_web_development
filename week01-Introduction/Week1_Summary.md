# Introduction to Server-side Web Development (Week 1)

**วิชา:** 06016418 Introduction to Server-side Web Development
**ผู้สอน:** Dr. Sarayut Nonsiri (sarayut@it.kmitl.ac.th) | **TA:** พี่นินจา (กอบชนม์ เหง้าศรี)

## 1. ภาพรวมวิชาและเกณฑ์การให้คะแนน
**เนื้อหาที่ครอบคลุม (Course Description):**
- Web Development in Django or Node.js
- MVC Concepts, Form Validation, Middleware, Sessions, Server-sided Rendering (SSR)
- Database Connections, Database transactions, Object Relational Mapper (ORM)
- Error Handling, Authentication and Authorization (LDAP, SSO with Common Accounts)
- Deployment

**เกณฑ์การให้คะแนน (Scoring Criteria):**
- การบ้าน (Homework) 20% | สอบกลางภาค (Midterm) 20% | สอบปลายภาค (Final) 30% | โครงงาน (Project) 30%
- **เกรด:** A (90-100), B+ (85-89), B (80-84), C+ (75-79), C (70-74), D+ (65-69), D (60-64), F (0-59)

**คะแนนพิเศษ (Extra 5 Points):** 
- เรียนและสอบคอร์ส Coursera: "Developing Back-End Apps with Node.js and Express" ให้เสร็จใน 7 วัน
- ส่งหลักฐานภาพหน้าจอ Grades ที่ผ่าน ไปที่อีเมลอาจารย์ (CC: 66070010@kmitl.ac.th) หัวข้อ `[06016418][Sec 2] ขอส่งรายละเอียดการสอบ Coursera`

## 2. พื้นฐานระบบคอมพิวเตอร์และการเขียนโปรแกรม (Computer & Programming Concepts)
- **การทำงานของคอมพิวเตอร์:** 
  ``` bash
  รับข้อมูล (Input) -> ประมวลผล (CPU & Memory) -> แสดงผล (Output) 
  ```
- **ประวัติศาสตร์:** 

  ภาษา C และการเขียนโปรแกรม `hello, world` ปรากฏครั้งแรกในหนังสือ "The C Programming Language" โดย Brian W. Kernighan & Dennis M. Ritchie (1978)
- **ภาษาโปรแกรมระดับสูง (High-level programming language):** 

  เครื่องมืออำนวยความสะดวกในการเขียนโปรแกรม ออกแบบมาให้มนุษย์แปลงความคิดเป็นชุดคำสั่ง (Source code) ได้ชัดเจน

- **โครงสร้างของชุดคำสั่ง (Code Structure):**
  - **Module:** กลุ่มของฟังก์ชันที่ถูกรวมไว้ด้วยกัน
  - **Package:** การรวม Modules ที่เกี่ยวข้องมาไว้ด้วยกัน
  - **Library:** การรวม Packages ที่เกี่ยวข้องมาไว้ด้วยกัน (Library คือหน่วยที่ใหญ่ที่สุด)

### ตัวแปลภาษา (Compiler vs Interpreter)
1. **Compiler (คอมไพเลอร์):** 

    โปรแกรมแปลชุดคำสั่งเป็นภาษาเครื่อง โดย **"ตรวจสอบความถูกต้องของชุดคำสั่งทั้งหมดก่อน"** จากนั้นจึงทำการแปลทีเดียวเพื่อนำไปทำงาน
2. **Interpreter (อินเทอร์พรีเตอร์):** 

    โปรแกรมแปลชุดคำสั่งเป็นภาษาเครื่อง โดย **"แปลทีละบรรทัด"** และป้อนเข้าสู่หน่วยประมวลผลให้ทำงานทันที

  > **💡 เนื้อหาเพิ่มเติมที่ควรรู้ (Extra Tips สำหรับข้อสอบ):**
  > - **Compiler:** โค้ดทำงานได้เร็วมาก (เพราะแปลเสร็จหมดแล้ว) แต่ตอนตรวจสอบบั๊กก่อนรันจะใช้เวลานาน (ตัวอย่างภาษา: C, C++, Java)
  > - **Interpreter:** หาและแก้บั๊กได้ง่ายกว่า (เพราะพังบรรทัดไหนก็หยุดตรงนั้น) แต่รันจริงจะช้ากว่า (ตัวอย่างภาษา: Python, JavaScript/Node.js)

## 3. คุณภาพซอฟต์แวร์และข้อผิดพลาด (Software Quality & Bugs)
- **First Law of Software Quality:** 
  
  $	{Errors} = ({more code})^2$ ยิ่งเขียนโค้ดเยอะ โอกาสเกิดข้อผิดพลาดยิ่งทวีคูณ

- **Bug (บั๊ก):** 

  จุดบกพร่องที่ทำให้โปรแกรมทำงานผิดพลาด คำนี้มาจากประวัติศาสตร์ที่ "แมลงมอด (Moth)" เข้าไปติดในรีเลย์ (Relay #70) ของคอมพิวเตอร์ Mark II

- **Debugging:** 

  การแก้ไขจุดบกพร่องของโปรแกรมให้ทำงานเป็นปกติ

- **กรณีศึกษาความเสียหายระดับโลกจาก Bug (Case Studies):**
  1. **Ariane 5 Explosion:** 
  
      จรวดมูลค่า $500M ระเบิดเพราะ Operand Error (การแปลงข้อมูล 64-bit float เป็น 16-bit integer ผิดพลาดในภาษา Ada)
  2. **Boeing 737 MAX:** 
  
      เครื่องบินตก (Lion Air และ Ethiopian Airlines) ผู้เสียชีวิต 346 คน เกิดจากเซนเซอร์ AOA (Angle-of-attack) ผิดพลาด และระบบ MCAS ทำงานโดยกดหัวเครื่องบินลงเองเพื่อแก้ปัญหา Stall

## 4. พื้นฐานการพัฒนาเว็บไซต์ (Web Basics)
- **องค์ประกอบพื้นฐานหน้าเว็บ:** 
    - `HTML` (เนื้อหา/โครงสร้าง), 
    - `CSS` (ความสวยงาม/ดีไซน์), 
    - `JS` (การทำงาน/ลอจิก)
- **การทำงานของระบบเว็บ (HTTP - Hyper Text Transfer Protocol):**
  - **Web Browser (ฝั่ง Client):** ส่งคำร้องขอ (`Request`)
  - **Web Server:** รับคำร้องขอ ประมวลผล และส่งผลลัพธ์กลับ (`Respond`)

### รหัสตอบกลับ (HTTP Status Codes)
- **100-199:** Informational Codes (แจ้งข้อมูล)
- **200-299:** Successful Codes (ทำงานสำเร็จ) 
  - `200` = OK (สำเร็จ)
  - `204` = No content (สำเร็จแต่ไม่มีเนื้อหาตอบกลับ)
- **300-399:** Redirection Codes (เปลี่ยนเส้นทาง)
- **400-499:** Client Error Codes (ความผิดพลาดจากฝั่งผู้ใช้)
  - `401` = Unauthorized (ไม่ได้รับสิทธิ์/ยังไม่ได้ล็อกอิน)
  - `404` = Not found (ไม่พบทรัพยากร/หน้าเว็บที่หา)
- **500-599:** Server Error Codes (ความผิดพลาดจากฝั่งเซิร์ฟเวอร์)
  - `500` = Internal server error (เซิร์ฟเวอร์พัง/โค้ดฝั่งหลังบ้านเออเร่อ)

> **💡 เนื้อหาเพิ่มเติมที่ควรรู้ :**
>
> ข้อสอบมักจะหลอกด้วย **HTTP Methods** ควบคู่กันไป สิ่งที่ต้องรู้เพิ่มคือ 
> - `GET` (ดึงข้อมูล), 
> - `POST` (สร้างข้อมูลใหม่ มักใช้กับ Code 201 Created), 
> - `PUT/PATCH` (อัปเดตข้อมูล), 
> - `DELETE` (ลบข้อมูล)
> - รหัส `403 Forbidden` ต่างกับ `401 Unauthorized` ตรงที่ 403 คือรู้ว่าเป็นใครแต่ "ไม่มีสิทธิ์เข้าถึง" ส่วน 401 คือ "ยังไม่ได้ยืนยันตัวตน"

## 5. โครงสร้างไฟล์และการตั้งชื่อ (Website File Structure & Naming)
- **กฎการตั้งชื่อไฟล์ที่ดี (Best Practices):**
  - **ห้ามเว้นวรรค (Avoid spaces):** ห้ามตั้งชื่อแบบ `product catalog.html`
  - **ใช้ตัวพิมพ์เล็กทั้งหมด (Use all lowercase letters):** เช่น `index.html` ไม่ใช่ `INDEX.html`
  - **ใช้ชื่อที่สั้นและมีความหมาย (Short, meaningful, descriptive):** เช่น `products.html`, `services.html`
  - **ดีที่สุดสำหรับ SEO (Search Engine Optimization):** ใช้เครื่องหมายขีดกลาง (Hyphen) คั่นคำ เช่น `product-catalog.html` 

> **💡 เนื้อหาเพิ่มเติมที่ควรรู้ :**
>
> สาเหตุที่ต้องใช้ตัวพิมพ์เล็กทั้งหมด เพราะ Server ส่วนใหญ่ใช้ระบบปฏิบัติการ Linux ซึ่งมีคุณสมบัติ **Case-sensitive** (ตัวพิมพ์เล็ก-พิมพ์ใหญ่ ถือว่าเป็นคนละไฟล์) การตั้งตัวพิมพ์เล็กทั้งหมดจะป้องกันปัญหาพังตอนนำขึ้น (Deployment) เซิร์ฟเวอร์จริงได้

## 6. รายละเอียดโครงงาน (Project Details)
- **สมาชิก:** ไม่เกิน 3 คนต่อกลุ่ม
- **โจทย์:** พัฒนาเว็บไซต์ตอบโจทย์ปัญหาทางสังคมหรือธุรกิจ (คะแนนรวม 30 คะแนน)
- **เกณฑ์ประเมินโครงงาน:**
  1. ไอเดียตอบโจทย์ปัญหา (10 คะแนน)
  2. เทคนิคการพัฒนาที่ใช้ (10 คะแนน)
  3. ความสวยงามและการออกแบบ UI/UX (5 คะแนน)
  4. การนำเสนอ รายบุคคล (5 คะแนน)
- **การส่งรายชื่อกลุ่ม:**
  - อีเมลหา: sarayut@it.kmitl.ac.th
  - CC: 66070010@kmitl.ac.th
  - หัวข้อ: `[06016418][Sec 1/2] ชื่อกลุ่ม`
- **Resource Link (ข้อมูลเพิ่มเติม):** `shorturl.at/WV7iq` หรือผ่าน OneDrive ลิงก์ที่ระบุในสไลด์
