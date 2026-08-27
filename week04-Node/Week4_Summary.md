# Introduction to Server-side Web Development (Week 1)

**วิชา:** 06016418 Introduction to Server-side Web Development
**ผู้สอน:** Dr. Sarayut Nonsiri (sarayut@it.kmitl.ac.th) | **TA:** พี่นินจา (กอบชนม์ เหง้าศรี)

## 1. เจาะลึก Node.js และ JavaScript เบื้องต้น
**Node.js** 
- คือ Runtime Environment ที่ทำให้เราเขียน JavaScript นอกเว็บเบราว์เซอร์ได้ (ทำงานฝั่งเซิร์ฟเวอร์) โดยใช้เอนจิน V8

**ทำไมต้องใช้ Node.js? (จุดเด่นที่มักออกสอบ)**
1. **ใช้ภาษาเดียวทั้งระบบ (Full-stack JS):** 
	- นักพัฒนาไม่ต้องสลับภาษาไปมาระหว่าง Frontend (เช่น React) และ Backend
2. **สถาปัตยกรรมประสิทธิภาพสูง:** 
   - **Event-driven & Non-blocking I/O:** สามารถรับ Request ใหม่ได้ทันทีโดยไม่ต้องรอ Request เก่าทำงานเสร็จ 
3. **Community ขนาดใหญ่:** 
	- มี `npm` ให้โหลดแพ็กเกจมหาศาล
4. **รองรับ Real-time Application:** 
	- เหมาะมากกับระบบแชทหรือ Live Dashboard

## 2. การเขียนโปรแกรมแบบ Asynchronous (หัวใจของ Node.js)
เพื่อไม่ให้โปรแกรมหยุดรอ (Blocking) Node.js จึงใช้การทำงานแบบ Asynchronous ซึ่งมีการพัฒนาหลักๆ 3 ยุค:

### ยุคที่ 1: Callback Functions
- **คอนเซปต์:** 
	
	ส่งฟังก์ชันเข้าไปเป็น Argument เพื่อให้เรียกใช้งานเมื่อทำงานเสร็จ
- **ข้อเสียหลัก (ระวังข้อสอบถาม):** 

	เกิด **Callback Hell** (Pyramid of Doom) หรือโค้ดซ้อนกันเป็นรูปพีระมิด ทำให้อ่านและแก้บั๊กยากมาก
```javascript
// ตัวอย่าง Callback
function fetchData(callback) {
    setTimeout(() => {
        const data = "Data received";
        callback(data);
    }, 2000);
}
fetchData((data) => { console.log(data); });
```
### ยุคที่ 2: Promises
- **คอนเซปต์:** 

	เป็น Object ตัวแทนของผลลัพธ์ในอนาคต โค้ดจะแบนลง (ไม่ซ้อนลึก)

- **วิธีใช้:** 
	
	.then() ทำงานเมื่อสำเร็จ (Resolve) และ .catch() ดักจับ Error (Reject)

```JavaScript
// ตัวอย่าง Promise
fetchDataPromise()
    .then((data) => { console.log("Success:", data); })
    .catch((error) => { console.log("Error:", error); });
```

### ยุคที่ 3: Async / Await (ดีที่สุดในปัจจุบัน)
- **คอนเซปต์:** 
	
	Syntax ใหม่ที่ครอบ Promise ไว้ ทำให้เขียนโค้ด Asynchronous ให้อ่านง่ายเหมือนเขียนแบบ Synchronous (ทำทีละบรรทัด)

- **วิธีใช้:** 

	ใช้ async หน้าฟังก์ชัน และ await หน้าตัวแปรที่ต้องรอ การจัดการ Error จะใช้ try...catch แทน

```JavaScript
// ตัวอย่าง Async/Await
async function getAndPrintData() {
    try {
        const data = await fetchDataPromise(); // สั่งให้รอจนกว่าจะได้ข้อมูล
        console.log("Data:", data);
    } catch (error) {
        console.error("Error:", error);
    }
}
```

## 3. ระบบ Module (การนำเข้าและส่งออกโค้ด)
### CommonJS (CJS) vs ES Modules (ESM)
Node.js รองรับทั้ง 2 แบบ แต่มีข้อแตกต่างที่ต้องจำให้แม่น:

- **CommonJS (แบบดั้งเดิม):**

	- **Syntax:** require() และ module.exports

	- **การทำงาน:** โหลดแบบ Synchronous (รอจนโหลดเสร็จค่อยทำบรรทัดถัดไป)

- **ES Modules (แบบใหม่ / มาตรฐาน ES2015):**

	- **Syntax:** import และ export

	- **การทำงาน:** โหลดแบบ Asynchronous (โหลดแบบไม่รอ)

	- **ข้อควรระวัง:** ต้องระบุนามสกุล .js ตอน import ด้วย

### Core Modules ที่สำคัญ (ไม่ต้องใช้ npm install)
- **1. fs (File System):**
	
 	จัดการไฟล์ เช่น fs.readFileSync (อ่านไฟล์), fs.writeFile (เขียนไฟล์)

- **path:**
	
	จัดการเรื่อง \ หรือ / ของ Path ที่ต่างกันระหว่าง Windows และ Mac/Linux

	ตัวอย่าง: 
	```path.join('users', 'john', 'report.pdf')```

- **os (Operating System):** 
	
	ดูข้อมูลเครื่องเซิร์ฟเวอร์
	
	ตัวอย่าง: 
	```os.platform(), os.totalmem(), os.freemem(), os.uptime()```

## 4. NPM และเจาะลึก package.json
### NPM (Node Package Manager) 
คือตัวจัดการ Library ภายนอก
- ```npm init -y``` : สร้าง package.json เร็วๆ
- ```npm install <ชื่อ>``` : ติดตั้งใช้จริง
- ```npm install -D <ชื่อ>``` : ติดตั้งใช้เฉพาะตอนพัฒนา (เช่น nodemon)
### โครงสร้าง package.json (บัตรประชาชนโปรเจกต์)
- ```name``` / ```version``` : ชื่อโปรเจกต์ (พิมพ์เล็ก) / เวอร์ชัน (เช่น 1.0.0)
- ```main``` : ระบุไฟล์แรกที่ระบบจะรัน (Entry point มักเป็น app.js หรือ index.js)
- ```scripts``` : คำสั่งลัด (เช่นตั้ง "dev": "nodemon app.js" เวลาเรียกใช้จะรัน npm run dev)
- ```dependencies``` : Library ที่ใช้รันจริง (เช่น express)
- ```devDependencies``` : Library ที่ใช้แค่ตอนเขียนโค้ด (เช่น nodemon ช่วยรีสตาร์ทเซิร์ฟเวอร์อัตโนมัติ)

## 5. HTTP Protocol & Status Codes
### โครงสร้าง HTTP Message (แลกเปลี่ยนระหว่าง Client ↔ Server)
- **1. Start-line** : ระบุ Method (GET, POST) + URL + Protocol Version (สำหรับ Request) หรือ Status Code (สำหรับ Response)

- **2. Headers** : ข้อมูลอธิบายเนื้อหา (เช่น Content-Type: text/plain)

- **3. Body** : ตัวข้อมูลจริงๆ (Payload)
### Status Code Cheat Sheet (ที่มักออกสอบ)

- **```200``` OK** : สำเร็จ
- **```201``` Created** : สร้างข้อมูลใหม่สำเร็จ
- **```301``` Moved Permanently** : เปลี่ยน URL ถาวร
- **```400``` Bad Request** : Client ส่งข้อมูลมาผิด Format
- **```401``` Unauthorized** : ยังไม่ได้ล็อกอิน (ไม่มีสิทธิ์)
- **```404``` Not Found** : หาหน้าเว็บ/ข้อมูลไม่เจอ
- **```500``` Internal Server Error** : โค้ดฝั่งเซิร์ฟเวอร์พัง

## 6. การสร้าง Web Server (Native vs Express)
### 6.1 แบบดั้งเดิม (Native HTTP) + การอ่านค่าจากผู้ใช้
สร้างได้ด้วย http.createServer()

```JavaScript
const http = require('http');
const url = require('url');

const server = http.createServer((req, res) => {
    // การอ่าน Query String (เช่น /search?q=nodejs)
    const parsedUrl = url.parse(req.url, true);
    const searchTerm = parsedUrl.query.q;

    // การอ่าน Headers (เช่น User-Agent)
    const userAgent = req.headers['user-agent'];

    // การทำ Routing แบบดั้งเดิม (ใช้ if-else เช็ค URL)
    if (parsedUrl.pathname === '/about') {
        res.statusCode = 200;
        res.setHeader('Content-Type', 'text/plain; charset=utf-8');
        res.end('เกี่ยวกับเรา');
    } else {
        res.statusCode = 404;
        res.end('Not Found');
    }
});
server.listen(3000);
```
### 6.2 การใช้ Express.js (ลดความซับซ้อน)
Express เป็น Framework ที่ช่วยให้โค้ดสั้น เป็นระเบียบ มีระบบ Routing ที่ดี และมี Middleware ยืดหยุ่น

```JavaScript
const express = require('express');
const app = express();

app.get('/', (req, res) => {
    res.send('Hello, Express'); // จัดการ Status และ Headers ให้อัตโนมัติ
});

app.listen(3000, () => {
    console.log('Server is running...');
});
```

### 6.3 Express Router (การแยกไฟล์ Routing)
เมื่อเว็บใหญ่ขึ้น เราจะไม่เขียนทุก Route ไว้ใน app.js แต่จะแยกออกมาด้วย express.Router()

สร้างไฟล์ routes/user.js:

```JavaScript
const express = require('express');
const router = express.Router();

router.get('/', (req, res) => { res.send('User Home'); });
router.get('/:id', (req, res) => { 
    res.send(`User ID: ${req.params.id}`); // ดึงค่า id จาก URL
});
module.exports = router;
```

### ประกอบร่างใน app.js:

```JavaScript
const express = require('express');
const app = express();
const userRouter = require('./routes/user');

// ถ้า URL เริ่มต้นด้วย /user ให้โยนไปที่ userRouter จัดการ
app.use('/user', userRouter); 

app.listen(3000);
```