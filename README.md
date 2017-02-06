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

และทาง server จะตอบกลับมาคือ

```
HTTP/1.1 200 OK
Content-Type: text/html

<html>
	<body>
		<i>Hello World</i>
	</body>
</html>
```

จากหลักการนี้ สามารถเขียนได้ดังนี้

```javascript
const http = require('http');
const server = http.createServer((req, res) => {
	res.writeHead(200, { 'Content-Type': 'text/html' });
	res.write(
	`
		<html>
			<body>
				<i>Hello World</i>
			</body>
		</html>
	`);
	res.end();
});
server.listen(1200);
```

จากนั้นใช้คำสั่ง node แล้วตามด้วยชื่อไฟล์ แล้วเปิด web browser ไปที่ localhost:1200 จะเห็นหน้า web ที่สร้างขึ้นมา ตัวแปร req ที่รับเข้ามาใน callback จะมีข้อมูลที่น่าสนใจคือ req.url, req.method และ req.headers เช่น

```javascript
{
	url: '/',
	method: 'GET',
	headers: {
		host: 'dekablade01.me:1200',
		connection: 'keep-alive',
		'cache-control': 'max-age=0',
		'user-agent': 'Mozilla/5.0 ...'
	}
}
```

ถ้าต้องการส่ง object ออกไปต้องใช้ JSON.stringify() เช่น

```javascript
const http = require('http');
const server = http.createServer((req, res) => {
	res.writeHead(200, { 'Content-Type': 'application/json' });
	res.write(JSON.stringify({name:'James Bond'}));
	res.end();
});
server.listen(1200);
```

#### แบบฝึกหัดที่ 1
1.1 ให้เขียน web server โดยมี path ที่ต้องการดังนี้ แต่ละ path ต้องมี HTML ที่แตกต่างกัน ( / , /products, /about )

1.2 ให้เขียน web server เพื่อหาว่าแต่ละ path ถูกเรียกมากี่ครั้งแล้ว

1.3 ให้เขียน web server เพื่อหาว่า server นี้ถูกเรียกด้วย browser แต่ละตัวกี่ครั้ง เช่น ถูกเรียกด้วย Chrome กี่ครั้ง, Firefox กี่ครั้ง, Edge กี่ครั้ง

1.4 ให้เขียน service เพื่อหาพื้นที่วงกลมจากรัศมีที่กำหนดให้ เช่น /area?radius=10 ได้ผลคือ 314.15926

1.5 ให้เขียน service เพื่อหาเขตต่างๆมีรหัสไปรษณีย์ว่าอะไร เช่น /zipcode/บางบัวทอง ได้ผลเป็น 11110 ถ้าหาไม่เจอให้ตอบว่า not found

```
	อำเภอ        รหัสไปรษณีย์
	เมืองนนทบุรี    11000
	บางบัวทอง     11110
	ปากเกร็ด      11120
	บางกรวย      11130
	บางใหญ่       11140
	ไทรน้อย       11150
```
1.6 ให้เขียน service หาว่าเลขจำนวนเต็มที่ใส่เข้ามามีตัวเลขอะไรหารได้บ้าง เช่น /divider/15 ได้ผลเป็น [1,3,5,15]

1.7 ให้เขียน service หาว่าเลขจำนวนเต็มที่ใส่เข้ามา 2 ตัว มีตัวเลขอะไรบ้างที่หารทั้งคู่ได้ เช่น /dividers/15/20 ได้ผลเป็น [1,5]

1.8 ให้เขียน service หาว่าเลขจำนวนเต็มที่ใส่เข้ามา 2 ตัว ให้หาตัวเลขที่มากที่สุดที่หารทั้งคู่ได้ เช่น /max-divider/15/20 ได้ผลเป็น 5

1.9 ให้เขียน service หาว่าจำนวนเต็มที่ใส่เข้ามา 2 ตัว สามารถไปหารตัวเลขอะไรได้พร้อมกัน และตัวเลขนั้นต้องน้อยที่สุดด้วย เช่น /least-common/10/15 ได้ 30 เพราะ 10 หาร 10, 20, 30, 40, ... ได้ลงตัว ส่วน 15 หาร 15, 30, 45, 60, ... ได้ลงตัว ดังนั้นตัวน้อยที่สุดที่หารได้พร้อมกันคือ 30

1.10 ให้เขียน function searchFile(name) ค้นหาว่าในเครื่องมี file ที่ชื่อ name หรือไม่ อย่าลืมต้องเข้าไปหาใน subfolder ด้วย

## การใช้ Express.js
Express.js เป็น framework ที่ช่วยให้เขียน web server ได้ง่ายขึ้น และกลับไปใช้คำสั่งของ Node.js ได้ง่ายอีกด้วย ตัวอย่างการเขียนคือ
```javascript
var express = require('express');
var app = express();
app.get('/', (req, res) => res.send(`
	<html>
		<body>
			<i>Hello World</i>
		</body>
	</html>
	`));
app.listen(1200);
```

ก่อนจะทำงานได้ต้องมีการติดตั้ง Express.js เขามาใน folder ก่อน ใช้คำสั่ง

`npm install express`

ตอนนี้ทุกอย่างพร้อมแล้ว ถ้าต้องการสั่งให้ทำงานก็ใช้คำสั่ง node แล้วตามด้วยชื่อ file เช่น

`node server.js`

จากนั้นเปิด browser ไปที่ localhost:1200 จะเห็นหน้า web ที่สร้างไว้

ถ้าต้องการใช้ static content เช่น HTML, CSS, JavaScript รวมไปถึงรูปภาพ และ video ใช้คำสั่ง express.static() เพื่อกำหนด folder ที่เก็บ static content ได้ เช่น

```javascript
app.use(express.static('public'));
```

## การส่ง Object ออกไป
บน Express.js สามารถส่ง object ออกไปได้ทันที เช่น
```javascript
var express = require('express');
var app = express();
var fs = require('fs');

app.get("/test", (req, res) => {
	fs.readdir('./', (error, result) => {
		res.send(result);
	});
});

app.listen(1200);
```

## การใช้ Parameter

Parameter มีหลายแบบเช่น
1. แบบส่งผ่าน ? เช่น /product?id=123 ข้อมูลด้านหลัง ? คือ parameter
2. แบบส่งผ่าน / เช่น /product/123 ข้อมูลจะดูเหมือนเป็น path
3. แบบส่งผ่าน post method
แต่ละวิธีมีรายละเอียดคือ

### แบบส่งผ่าน ? สามารถเข้าถึงโดยตรงผ่าน req.query เช่น
```javascript
app.get('/', (req, res) => {
	console.log(req.query);
	res.send(...);
});
```

### แบบส่งผ่าน / มีประโยชน์สำหรับ Search Engine เช่น ต้องการให้มีการเข้าถึงสินค้าได้ในรูปแบบของชื่อ
`my-web.com/book/programming-windows`

`my-web.com/book/web-programming`

หรือจะใช้แบบตัวเลขก็ได้ เช่น

`my-web.com/product/12345`

`my-web.com/product/67890`

เริ่มต้นต้องกำหนดให้ path ที่ต้องการรับ parameter ก่อน เช่น
```javascript
app.get("/book/:title", ... );
app.get("/product/:id", ... );
```
จากนั้นใน function สามารถใช้ตัวแปร req.params เพื่อเข้าถึง parameter แต่ละตัวได้ จากข้างบนคือ req.params.title และ req.params.id สามารถใช้ได้ในแต่ละ function

### ถ้าต้องการส่ง parameter ผ่าน post method สามารถใช้ body-parser ได้ เช่น
```javasript
const parser = require('body-parser');
app.use(parser.urlencoded(extended: false)); // string only
```
ถ้าต้องการส่ง parameter ผ่าน post method สามารถใช้ body-parser ได้ เช่น

### แบบฝึกหัดที่ 2

2.1 ให้เขียน web server เพื่อหา VAT จากราคาสินค้า เช่น /vat?price=200 ได้ผลลัพธ์ 14

2.2 ให้เขียน web server เพื่อหาพื้นที่วงกลมจากรัศมีที่กำหนดให้ เช่น /area/10 ได้ผลคือ 314.15926

2.3 ให้เขียน web server เพื่อหาพื้นที่วงกลมจากรัศมีที่กำหนดให้ เช่น /area?radius=10 ได้ผลคือ 314.15926

2.4 ให้เขียน web server มีหน้า /home แสดงช่องให้ผู้ใช้กรอกข้อมูล รัศมีของวงกลม เมื่อกรอกเสร็จกดปุ่ม submit จะเรียกหน้า /area?radius=... ให้คำนวณหาพื้นที่วงกลมแล้วแสดงผลเป็น HTML

2.5 ให้เขียน service รับจำนวนเงินเข้ามาแล้วบอกกว่าตู้ ATM ต้องจ่ายธนบัตรอะไรกี่ใบ เช่น /dispense/1700 ได้คำตอบคือ ต้องใช้ธนบัตร 1000 จำนวน 1 ใบ ธนบัตร 500 จำนวน 1 ใบ ธนบัตร 100 จำนวน 2 ใบ

```javascipt
[
	{denomination: 1000, amount: 1},
	{denomination:  500, amount: 1},
	{denomination:  100, amount: 2}
]
```

2.6 ให้เขียน service หาว่าเลขจำนวนเต็มนั้นเกิดขึ้นมาจากจำนวนเต็มอะไรคูณกัน เช่น /factor/18 ได้ผลเป็น 2 * 3 * 3

2.7 ให้เขียน service หาว่าเลขจำนวนเต็มที่ใส่เข้ามา 2 ตัว ให้หาตัวเลขที่น้อยที่สุดที่ทั้งคู่หารได้ เช่น /least-common-divider/15/20 ได้ผลเป็น 60

2.8 ให้เขียน service แปลงจากเดือนภาษาอังกฤษเป็นภาษาไทย เช่น /thai-month/january ได้ผลเป็น มกราคม

2.9 ให้เขียน service หาว่า string 2 อันที่เข้ามาเป็น anagram หรือ ประกอบด้วยตัวอักษรเหมือนกันหรือไม่ เช่น /anagram/largely/gallery ได้ true 10.

## การเขียน View ด้วย Jade
Jade เป็น Template Engine อย่างหนึ่ง ก่อนใช้งานต้องติดตั้งด้วยคำสั่ง npm install jade จากนั้น set ให้ view engine เป็น jade เช่น

```javascript
app.set('view engine', 'jade');
```

จากนั้นสร้าง file ชื่อ index.jade ภายใน folder ชื่อ views เขียนเป็นภาษา Jade ลักษณะแบบนี้

```jade
html
	body
		h1 Hello
		p.
			First Paragraph

		ul
			for value in data
				li=value
```
เมื่อมี request มาที่ path ที่เราต้องการก็สามารถนำ index.jade มาแสดงผลได้ด้วยคำสั่ง render() เช่น

```javascript
เมื่อมี request มาที่ path ที่เราต้องการก็สามารถนำ index.jade มาแสดงผลได้ด้วยคำสั่ง render() เช่น
```

### การเขียน View ด้วย EJS
EJS เป็น Template Engine อย่างหนึ่ง ก่อนใช้งานต้องติดตั้งด้วยคำสั่ง npm install ejs จากนั้น set ให้ view engine เป็น ejs เช่น
```javascript
app.set('view engine', 'ejs');
```
จากนั้นสร้าง file ชื่อ index.ejs ใน folder ชื่อ views

```ejs
<html>
	<body>
		<ul>
			<% for (var v of data) { %>
				<li><%= v %></li>
			<% } %>
		</ul>
	</body>
</html>
```

เมื่อมี request มาที่ path ที่เราต้องการก็สามารถนำ index.ejs มาแสดงผลได้ด้วยคำสั่ง render() เช่น

```javascript
app.get('/', (req, res) => res.render('index', {data:[1,3,5]}));
```

ในบาง IDE ต้องการให้ตั้งชื่อลงท้ายด้วย .html ก็สามารถ set ได้ด้วยคำสั่งนี้

```javascript
app.engine('html', require('ejs').renderFile);
```

แต่อย่าลืมเปลี่ยน render() ให้ส่งชื่อ file แบบเต็มเข้าไปแทน
```javascript
app.get('/', (req, res) => res.render('index.html', {data:[1,3,5]}));
```

### Middleware

Middleware คือ function ที่ทำงานก่อนหรือหลัง function หลัก เช่น ถ้าต้องการให้มีการบีบอัดข้อมูลก่อนส่ง สามารถใช้ compression เป็น middleware ได้

```javascript
var compression = require('compression');
app.use(compression());
```

ถ้ามี 2 functions ที่แยกกันทำงานโดยที่ทั้งคู่ทำงานเมื่อมี request มาที่ path เดียวกัน เช่น

```javascript
var express = require('express');
var app = express();
app.get('/', f1);
app.get('/', f2);
app.listen(1200);

function f1(req, res) { res.send('index');               }
function f2(req, res) { console.log('index was called'); }
```

จากข้างบน เมื่อมี request มาที่ / แล้ว f1() จะถูกเรียก แต่ f2() จะไม่ถูกเรียก เพราะ function แรกถูกเรียกไปแล้ว และจบการทำงานโดยไม่มีการเรียก function ถัดมา ถ้าต้องการให้ function ถัดไปถูกเรียกด้วย ต้องรับตัวแปร next เข้ามา แล้วเรียก next() เช่น

```javascript
function f1(req, res, next) { res.send('index'); next();       }
function f2(req, res)       { console.log('index was called'); }
```

ตัวอย่างการเขียน middleware ชื่อ UrlLogger ทำงานโดยพิมพ์ URL และ เวลาที่ถูกเรียกออกมา

```javascript
var express = require('express');
var app = express();
app.use(UrlLogger);
app.get('/', (req, res) => { res.send('index'); });
app.listen(1200);

function UrlLogger(req, res, next) {
	console.log(req.url + ' was called at ' +
		new Date().toISOString());
	next();
}
```

ตัวอย่างการเขียน middleware เพื่อให้รองรับการเรียกข้อมูลข้าม domain

```javascript
function Allower(req, res, next) {
	res.header("Access-Control-Allow-Origin", "*");
	res.header("Access-Control-Allow-Headers",
		"Origin, X-Requested-With, Content-Type, Accept");
	next();
}
```
Error Handling สามารถทำผ่าน middleware ได้ โดยรับตัวแปร 4 ตัว คือ error, request, response และ next ตามลำดับ และไม่จำเป็นต้องเรียก next() เพราะต้องทำงานเป็นลำดับสุดท้าย

```javascript
var express = require('express');
var app = express();
app.get('/', (req, res) => { res.send('index'); });
app.use(ErrorHandler);
app.listen(1200);

function ErrorHandler(error, req, res, next) {
	console.error(error.stack);
	res.status(404).send('File not found');
}
```
### แบบฝึกหัดที่ 3

1. ให้เขียน middleware เพื่อหาว่า path แต่ละอันถูกเรียกมากี่ครั้งแล้ว โดยดูจาก req.url
2. ให้เขียน middleware เพื่อหาว่า server นี้ถูกเรียกด้วย browser แต่ละตัวกี่ครั้ง เช่น ถูกเรียกด้วย Chrome กี่ครั้ง, Firefox กี่ครั้ง, Edge กี่ครั้ง โดยดูจาก User-Agent
3. ให้เขียน middleware เพื่อหาว่า server นี้ถูกเรียกด้วย OS อะไร กี่ครั้ง
4. ให้เขียน middleware ที่ทำงานเมื่อเกิด error เพื่อบอกผู้ใช้ว่า หน้าที่ใกล้เคียงมีอะไรบ้าง เช่น ถ้าผู้ใช้เข้า /abot ควรถามผู้ใช้ว่า หมายถึง /about ใช่หรือไม่
5. ให้เขียน middleware เพื่อตรวจสอบว่า request จากเครื่องไหนเกิน 1000 ครั้ง ใน 1 ชั่วโมง ให้ส่ง Error 429 Too Many Requests กลับไป ตรวจสอบ IP Address ได้จาก req.ip
6. ให้เขียน middleware ที่มี array ของ IP address ถ้ามี request จากเครื่องใน array นี้ให้ส่ง content กลับไป แต่ถ้า request มาจากเครื่องอื่นที่ไม่อยู่ใน array ให้ส่ง Error 403 Forbidden กลับไป
7. ให้เขียน middleware ตรวจสอบว่า request มาหา video ที่ลงท้ายด้วย .flv หรือไม่ ถ้าใช่ให้ redirect ไปที่ file เดิมแต่ลงท้ายด้วย .mp4
8. ให้เขียน middleware เพื่อ redirect ทุก request ที่มาที่ HTTP ไปที่ HTTPS โดยดูจาก req.protocol
9. ให้เขียน middleware เพื่อเก็บข้อมูลว่า Error 404 ที่เกิดมาจาก path ไหนกี่ครั้ง
10. ให้เขียน middleware เพื่อเก็บข้อมูลว่า Referer หรือเว็บก่อนหน้ามีอะไรบ้าง กี่ครั้ง โดยดูจาก Referer ใน HTTP Header

## การใช้ MongoDB

เริ่มต้นต้อง run ตัว server ก่อน ด้วยคำสั่ง mongod ใน UNIX อย่าลืมใส่ sudo ข้างหน้า

`mongod --dbpath`

จากนั้นเรียกใช้งานด้วยคำสั่ง mongo แล้วลองการสร้างฐานข้อมูล ด้วยคำสั่ง
`use bookshop;`

การใส่ข้อมูล
`db.books.insert({title:'Mathematics', price:190});`

การค้นหาข้อมูลใช้คำสั่ง find()
การแก้ไขข้อมูลใช้คำสั่ง update()
การลบข้อมูลใช้คำสั่ง remove()

`db.books.find();`

`db.books.update();`

`db.books.remove();`

การ export ข้อมูลลง file

`mongoexport --db bookshop --collection books --out books.json`

การ import ข้อมูลจาก file

`mongoimport --db bookshop --collection books --drop --file data.json`

การใช้ MongoDB ผ่าน Node.js ก่อนจะใช้งานต้องติดตั้ง module ด้วย npm install mongodb ก่อน

```
"use strict";
let mongo = require('mongodb').MongoClient;
let connection = 'mongodb://localhost:27017/bookshop';
mongo.connect(connection, (error, database) => {
	database
	.collection('books')
	.find()
	.toArray((error, result) => {
		console.log(result);
	});
});
```

## MySQL
การใช้ MySQL ต้องใช้ module ชื่อ mysql จากนั้นใช้ pool ในการเชื่อมต่อ เช่น
```
let mysql = require('mysql');
let connection = {
	host:'127.0.0.1',
	user:'user',
	password:'password',
	database:'coffee_shop'	
};
let pool = mysql.createPool(connection);
pool.query("select * from stations", 
	(e, rows) => {
		console.log(rows);
	}
);
```
## Header
Header ใน HTTP สามารถใช้คำสั่ง res.set() เพื่อตั้งค่าได้เช่น

`res.set('Content-Type', 'text/plain');`

หรือจะ set เป็น object ก็ได้เช่นกัน
```javascript
res.set({
	'Content-Type': 'text/plain',
	'Content-Length': '123',
	'ETag': '12345'
});
```

การสั่งให้ browser เก็บข้อมูลลงใน cookie ต้องใช้ header ชื่อ Set-Cookie เช่น ต้องการเขียนใน header เป็น Set-Cookie: Token=123456789 เขียนได้ดังนี้
```javascript
let id = parseInt(Math.random() * 1000000000000);
res.set('Set-Cookie', 'token=' + id);
```

หรือจะกำหนดเวลาด้วยก็ได้ เช่นต้องการให้ cookie ใช้งานได้ 30 นาที ใน header ต้องมีการกำหนดเวลาด้วย Expires=... ไว้ด้วย เช่น

```javascript
let id = parseInt(Math.random() * 1000000000000);
let time = Date.now();
res.set('Set-Cookie', 'token=' + id +
	'; Expires=' + new Date(time + 30 * 60 * 1000).toUTCString());
```

## แบบฝึกหัดที่ 3

1. ให้เขียนหน้า /register เมื่อกดปุ่ม Register แล้วส่งข้อมูล ชื่อ email และรหัสผ่าน ด้วย Post Method เก็บข้อมูลลง MongoDB
2. ให้เขียนหน้า /login ใส่ email และ รหัสผ่าน ถ้ารหัสผ่านถูกให้เก็บข้อมูลไว้ใน cookie
3. ให้เขียนหน้า /settings ให้ผู้ใช้แก้ไขชื่อได้ โดยหน้านี้ต้อง login ก่อน
4. ให้เขียนหน้า /find มีช่องให้ใส่ชื่อผู้ใช้จากนั้นค้นหาผู้ใช้ตาม email
5. ให้เขียนหน้า /logout เพื่อออกจากระบบและลบข้อมูลระบบ
6. ให้เขียน web ที่มีหน้าให้ใส่ข้อมูลหนังสือและราคา จากนั้นเก็บข้อมูลลง MongoDB
7. ให้เขียน web ที่มีหน้าให้ค้นหาจากหนังสือใน MongoDB
8. ให้เขียน web ที่มีหน้าให้แก้ไขข้อมูลหนังสือใน MongoDB
9. ให้เขียน web ที่มีหน้าให้ลบข้อมูลหนังสือใน MongoDB
10. ให้เขียน web ที่แสดงข้อมูลหนังสือโดยใช้ _id ของ MongoDB เช่น /book/12345






