# 1. ใช้ Base Image เป็น Node.js เวอร์ชัน 20 (Alpine จะมีขนาดเล็กและเบา)
FROM node:20-alpine

# 2. กำหนดโฟลเดอร์ทำงานหลักข้างใน Container
WORKDIR /app

# 3. ก๊อปปี้ไฟล์ package.json เพื่อเข้าไปติดตั้ง dependencies
COPY package.json ./

# 4. รันคำสั่งติดตั้ง Express
RUN npm install

# 5. ก๊อปปี้โค้ดทั้งหมดที่เหลือในเครื่องเรา เข้าไปใน Container
COPY . .

# 6. บอกว่าแอปนี้เปิดใช้งานพอร์ต 3000
EXPOSE 3000

# 7. คำสั่งที่ใช้เปิดรันตัวเว็บตอนจังหวะเปิด Container
CMD ["node", "index.js"]