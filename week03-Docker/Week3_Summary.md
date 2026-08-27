# Introduction to Server-side Web Development (Week 1)

**วิชา:** 06016418 Introduction to Server-side Web Development
**ผู้สอน:** Dr. Sarayut Nonsiri (sarayut@it.kmitl.ac.th) | **TA:** พี่นินจา (กอบชนม์ เหง้าศรี)

## 📌 พาร์ทที่ 1: เนื้อหาทฤษฎีหลัก (Core Concepts & Theory)

### 1.1 ปัญหาที่ Docker เข้ามาแก้
ปัญหาคลาสสิกของนักพัฒนาคือ **"It works on my machine"** (รันบนเครื่องตัวเองได้ แต่ไปพังบนเครื่องคนอื่น) สาเหตุหลักมาจาก:
- **Environment Mismatch:** ความแตกต่างระหว่างเครื่อง Dev และ Production (เช่น OS คนละระบบ)
- **Dependency Hell:** ปัญหาไลบรารีคนละเวอร์ชัน หรือการตั้งค่า OS (OS Settings) ที่ไม่ตรงกัน
- **Manual Setup:** การติดตั้งซอฟต์แวร์แบบเดิมใช้เวลานานและเสี่ยงต่อความผิดพลาด

### 1.2 Docker คืออะไร และทำไมต้องใช้?
	Docker เป็นเครื่องมือที่ช่วยให้นักพัฒนาสามารถ "แยก (Isolate) และ บรรจุ (Pack)" แอปพลิเคชันรวมถึงสภาพแวดล้อมต่างๆ ขึ้นมาเป็นก้อนเดียวกันเรียกว่า **"Container"**

**เหตุผลที่ควรใช้ Docker (Why Docker?):**
- **Standardization:** สร้างมาตรฐานเดียวในการ Pack แอป ทำให้ทุกคนทำงานบนพื้นฐานเดียวกัน
- **Portability:** ย้ายแอปไปรันที่ไหนก็ได้ ไม่ว่าจะเป็นเครื่องเพื่อน, Cloud หรือ On-premise
- **Microservices:** แยกแอปใหญ่ออกเป็นส่วนเล็กๆ รันแยกกัน อิสระต่อกัน และแชร์ทรัพยากรคุ้มค่า

### 1.3 องค์ประกอบหลักของ Docker (Architecture)
- **Docker Image:** เปรียบเสมือน "พิมพ์เขียว (Blueprint)" หรือแผ่น CD ติดตั้งโปรแกรมที่เป็นแบบ Read-only
- **Docker Container:** เปรียบเสมือน "บ้านที่สร้างจากพิมพ์เขียว" คือตัวโปรแกรมที่กำลังทำงานจริง (Runtime instance)
- **Docker Registry (Docker Hub):** เปรียบเสมือน Store หรือคลังเก็บ Image ที่เราสามารถไปโหลดมาใช้ได้
- **Docker Daemon:** ทำหน้าที่จัดการสร้าง การทำงาน และการกระจายคอนเทนเนอร์ (อยู่ฝั่ง Docker Host)
- **Docker Client:** ส่วนที่ผู้ใช้ใช้โต้ตอบกับ Docker Daemon (เช่น การพิมพ์คำสั่ง `docker run`)

### 1.4 เปรียบเทียบ Virtual Machines (VMs) vs. Docker Containers
- **Virtual Machine (VM):** The Heavyweight Approach
  - มี Guest OS ในทุกๆ VM
  - ขนาดใหญ่ (หลาย GB)
  - Start Speed: ช้า (หลักนาที)
  - Isolation: สูงมาก (Hardware Level)
- **Docker Container:** The Lightweight Revolution
  - ใช้ OS Kernel ร่วมกัน (Shared Kernel) กับ Host ไม่มี Guest OS
  - ขนาดเล็ก (MB ถึงไม่กี่ร้อย MB)
  - Start Speed: เร็วมาก (หลักวินาที)
  - Isolation: สูง (Process Level)

### 1.5 การจัดการเครือข่ายและข้อมูล (Networking & Volumes)
- **Port Mapping:** การเชื่อมพอร์ตจากเครื่องเราเข้ากับ Container
- **Environment Variables:** การส่งค่าตัวแปรเข้าไปในโปรแกรม
- **Docker Volumes:** การทำ Data Persistence เพื่อให้ข้อมูลไม่หายเมื่อ Container ถูกลบ

### 1.6 การจัดการหลาย Container (Docker Compose)
- **ปัญหา:** ถ้าแอปมีทั้ง App, Database, Redis รันทีละตัวจะยุ่งยาก
- **Docker Compose:** การเขียนไฟล์ `docker-compose.yml` เพื่อสั่งรันทุกอย่างพร้อมกันด้วยคำสั่งเดียว (คำสั่งสำคัญ: `docker-compose up -d` และ `docker-compose down`)

## 🛠️ พาร์ทที่ 2: ตัวอย่างโค้ดและคำสั่งปฏิบัติ (Practical Examples & CLI)

### 2.1 คำสั่งพื้นฐาน (Basic CLI)
- `docker --version` หรือ `docker -v`: เช็คว่าติดตั้ง Docker เรียบร้อยแล้วหรือไม่
- `docker pull <image_name>`: โหลด Image จาก Registry
- `docker run <image_name>`: สร้างและรัน Container
  - *ตัวอย่าง Hello World:* `docker run hello-world` (ถ้าไม่มี Image ในเครื่อง จะ Pull มาให้อัตโนมัติ)
- `docker ps`: ดู Container ที่กำลังทำงานอยู่
- `docker stop <name>` / `docker start <name>`: หยุดหรือเริ่ม Container
- `docker rm <name>`: ลบ Container
- `docker rmi <image_name>`: ลบ Image
- `docker exec`: การเข้าไปพิมพ์คำสั่งข้างใน Container

### 2.2 ตัวอย่างการรัน Web Server (Nginx)
```bash
docker run -d -p 3000:80 --name my-webserver nginx
```

### 2.4 ขั้นตอนการ Build, Run และนำขึ้น Docker Hub
สร้าง Image (Build):
```bash
docker build -t student-web .
# (อย่าลืมจุด . ด้านหลังสุด หมายถึงให้สร้างจากโฟลเดอร์ปัจจุบัน)
```
รัน Container เพื่อทดสอบ:

```Bash
docker run -d -p 3000:3000 --name my-app student-web
```
หยุดและลบ Container ทิ้ง (เมื่อเทสต์เสร็จ):

```Bash
docker stop my-app && docker rm my-app
```
Login เข้าสู่ Docker Hub:
```Bash
docker login
```
ติดป้ายชื่อ (Tag) ให้ตรงกับชื่อบัญชี Docker Hub:

```Bash
# รูปแบบ: docker tag [ชื่อเดิม] [username_ของคุณ]/[ชื่อ_image]:[เวอร์ชัน]
docker tag student-web somchai_hub/student-profile:v1
```
ดันขึ้นคลาวด์ (Push):
```Bash
docker push somchai_hub/student-profile:v1
```