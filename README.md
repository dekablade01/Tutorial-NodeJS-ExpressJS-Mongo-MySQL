# Tutorial-NodeJS-ExpressJS-Mongo-MySQL

## Node.js

Node.js คือ ระบบการเขียน JavaScript บน server ซึ่งต้อง download จาก https://nodejs.org แล้วติดตั้งลงไปในเครื่องที่ต้องการ จากนั้นลองใช้คำสั่ง node -v เพื่อตรวจสอบดูว่าติดตั้งสำเร็จหรือไม่

ลองเขียน code ง่ายๆ ให้แสดงว่ามี file และ folder ชื่ออะไรบ้าง

```javascript
var fs = require('fs');
var result = fs.readdirSync('./');
console.log(result);
```

สำหรับบน server เป็นระบบที่สามารถควบคุมได้ จึงมักนิยมใช้คำสั่งรุ่นใหม่ๆ เช่น แทนที่จะใช้ var ก็ใช้ let หรือ const แทน อย่าลืมต้องใช้ "use strict" ด้วย เช่น

```javascript
"use strict";
let fs = require('fs');
let result = fs.readdirSync('./');
console.log(result);
```

โดยทั่วไปมักนิยมเรียก function แบบ asynchronous เพราะไม่รู้ว่ามันจะทำงานเสร็จตอนไหน แต่เมื่อเสร็จแล้วให้มาทำงานที่ function ที่กำหนดไว้ เช่น

```javascript
const fs = require('fs');
fs.readdir('./', print);

function print(error, result) {
	console.log(result);
}
```

หรือจะใช้ anonymous function ก็ได้ เช่น

```javascript
const fs = require('fs');
fs.readdir('./', function(error, result) {
	console.log(result);
});
```
นอกจากนี้ยังนิยมใช้ arrow function ช่วยให้ code อ่านง่ายขึ้น ดังนี้

```javascript 
const fs = require('fs');
fs.readdir('./', (error, result) => console.log(result));
```

### การเขียน Web Server
เมื่อเราเรียกเว็บไซต์เช่น dekablade01.me ตัว web browser จะค้นหาว่า server นี้มี IP address คืออะไร จากนั้นจะส่งข้อความไปหาที่ port 80 ข้อความนั้นคือ
```
GET / HTTP/1.1
Host: dekablade01.me
```




