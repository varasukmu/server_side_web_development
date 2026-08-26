# Introduction to Server-side Web Development (Week 1)

**วิชา:** 06016418 Introduction to Server-side Web Development
**ผู้สอน:** Dr. Sarayut Nonsiri (sarayut@it.kmitl.ac.th) | **TA:** พี่นินจา (กอบชนม์ เหง้าศรี)

## 1. Version Control System (VCS) คืออะไร?
Version Control คือ "ระบบจัดเก็บประวัติการเปลี่ยนแปลงของไฟล์" (ส่วนใหญ่ใช้กับ Source Code) เหมาะสำหรับทำงานเดี่ยวและทีม ช่วยให้สามารถ **Revert** (ย้อนกลับไปยังสถานะก่อนหน้าได้หากโค้ดพัง) และดูประวัติได้ว่าใครแก้อะไร เมื่อไหร่

**ประเภทของ VCS:**
1. **Local VCS:** 

    เก็บ Patch (ความแตกต่างของไฟล์) ไว้ในฐานข้อมูลบนเครื่องตัวเอง
2. **Centralized VCS (CVCS):** 

    เช่น SVN มี Server กลางเก็บข้อมูล ข้อดีคือจัดการสิทธิ์ง่าย แต่มีข้อเสียร้ายแรงคือ **Single Point of Failure** (ถ้า Server พัง ทุกอย่างหยุดทำงาน)
3. **Distributed VCS (DVCS):** 

    เช่น **Git** เครื่องของนักพัฒนาทุกคนจะมีสำเนา (Clone) ของ Repository และประวัติครบถ้วน ไม่ต้องพึ่งพาระบบกลางตลอดเวลา

## 2. คอนเซปต์สำคัญของ Git
- Git ถูกสร้างโดย **Linus Torvalds** (ผู้สร้าง Linux)
- **Snapshots vs Deltas:** Git เก็บข้อมูลด้วยวิธี **Snapshots (ภาพรวมของไฟล์ ณ เวลานั้น)** ไม่ใช่แค่การเก็บ Deltas (ส่วนต่างที่เปลี่ยนไป)
- **SHA-1 Checksum:** ทุกการเปลี่ยนแปลงจะถูกเข้ารหัสตรวจสอบความถูกต้องด้วย SHA-1 (ความยาว 40 ตัวอักษร) เสมอ
- Git เก็บข้อมูลอ้างอิงจาก Hash ไม่ใช่ชื่อไฟล์ และแทบไม่มีคำสั่งใดที่ลบข้อมูลโดยตรง ทุกอย่างย้อนกลับได้หากทำการ Commit แล้ว

## 3. พื้นที่ทำงาน 3 ส่วน (The Three States)
1. **Working Tree:** 
    - โฟลเดอร์โปรเจกต์ที่เรากำลังเปิดแก้ไขไฟล์อยู่
2. **Staging Area (Index):** 
    - พื้นที่พักไฟล์ เพื่อเตรียมตัวยืนยันการเปลี่ยนแปลง (เตรียม Commit)
3. **Git Directory (.git):** 
    - ฐานข้อมูลหลักของ Git ที่เก็บ Metadata และ History ทั้งหมด

### สถานะของไฟล์ (File Statuses)
- **Untracked:** ไฟล์ใหม่ที่ Git ยังไม่รู้จัก
- **Tracked:** ไฟล์ที่ Git ติดตามอยู่ แบ่งออกเป็น:
  - `Modified`: แก้ไขโค้ดแล้ว แต่ยังไม่ได้เข้าสู่ Staging
  - `Staged`: เตรียมพร้อมที่จะ Commit
  - `Committed`: ถูกบันทึกลง Database ของ Git อย่างถาวร

### โครงสร้างที่สำคัญในโฟลเดอร์ `.git`
- `hooks/`: สคริปต์ทำงานอัตโนมัติเมื่อเกิด Event
- `info/`: เก็บไฟล์ Exclude 
- `objects/`: เก็บข้อมูลทั้งหมดแบบเข้ารหัส
- `refs/`: เก็บตำแหน่ง Branch และ Tag
- `config`: ตั้งค่าเฉพาะ Repository นั้น
- `HEAD`: ตัวชี้ว่าปัจจุบันเราทำงานอยู่ที่ Branch หรือ Commit ไหน

## 4. Cheat Sheet: คำสั่ง Command Line ที่ต้องจำ
- `git config --global user.name "Name"` / `user.email`: ตั้งค่าตัวตนผู้ใช้งาน
- `git init`: สร้าง Git Repository ในโฟลเดอร์ปัจจุบัน
- `git add <file>` (หรือ `git add .`): ดึงไฟล์จาก Working Tree เข้าสู่ Staging Area
- `git commit -m "Message"`: บันทึก Snapshot จาก Staging ลง Git Directory
- `git status`: ตรวจสอบสถานะไฟล์ (Modified, Staged, Untracked)
- `git log`: ดูประวัติการ Commit (ทริค: ใช้ `git log --oneline --graph --all` เพื่อดูแบบย่อและเห็น Branch)
- `git diff`: ดูรายละเอียดว่าโค้ดบรรทัดไหนถูกเพิ่มหรือลบ

## 5. การอัปโหลดโค้ดขึ้น Remote Repository (GitHub)
เมื่อเริ่มต้นโปรเจกต์ใหม่ และต้องการนำขึ้นเซิร์ฟเวอร์เป็นครั้งแรก:
1. `git init` (สร้าง Repo)
2. `git add .` (ดึงไฟล์ทั้งหมดเข้า Staging)
3. `git commit -m "Initial commit"` (บันทึกเวอร์ชันแรก)
4. `git remote add origin <URL>` (ผูก Git ในเครื่องเข้ากับ GitHub)
5. `git push -u origin main` (อัปโหลด Branch 'main' ขึ้นสู่เซิร์ฟเวอร์ และเซ็ต Upstream ด้วย `-u`)

> **💡 Extra Tips สำหรับข้อสอบ:**
> - สาเหตุที่โปรแกรมเมอร์มักถูกทดสอบเรื่อง CLI (Command Line Interface) มากกว่า GUI เพราะ CLI รองรับคำสั่งของ Git ได้ครอบคลุม 100%
> - หากมีการถามเรื่องความเสี่ยง ให้ตอบว่า CVCS เสี่ยงกว่า DVCS เพราะหากเน็ตหลุดหรือเซิร์ฟเวอร์กลางพัง CVCS จะทำงานต่อไม่ได้เลย
