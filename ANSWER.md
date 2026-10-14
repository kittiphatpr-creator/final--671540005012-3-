# ANSWER — นาย กิตติพัฒน์ ประชุมสาร 671540005012-3

## Part A — ข้อมูลจาก image kamthornsa/ksu-promo:1.0

| คำถาม | คำตอบ | คำสั่งที่ใช้ |
| --- | --- | --- |
| Port | 3000 และ 80 | `docker inspect --format='{{.Config.ExposedPorts}}' kamthornsa/ksu-promo:1.0` |
| CMD | `[nginx -g daemon off;]` | `docker inspect --format='{{.Config.Cmd}}' kamthornsa/ksu-promo:1.0` |
| Base image | `nginx:alpine` | `docker history kamthornsa/ksu-promo:1.0` |

## Part B — รหัสลับบนหน้าโปรโมชั่น

ESPRESSO-47

## Part C — คำถาม

1. **ทำไมแก้ข้อความบนหน้าโปรโมชั่นไม่ได้ แต่หน้าเมนูแก้ได้:** 
เพราะหน้าโปรโมชั่นถูกดึงมาจาก Image สำเร็จรูป (Pre-built) บน Docker Hub ที่เราไม่มีซอร์สโค้ด ส่วนหน้าเมนูเรา Build เองจากไฟล์ index.html ในเครื่องของเรา จึงสามารถแก้ไขและ Build ใหม่ได้
2. **ถ้าเขียน image โดยไม่ใส่ tag จะได้หน้าเว็บแบบไหน เพราะอะไร:** 
จะได้หน้าเว็บเวอร์ชันล่าสุด (tag: latest) แทนที่จะเป็นเวอร์ชัน 1.0 ซึ่งอาจเป็นเวอร์ชันใหม่ที่มีการอัปเดตหน้าตา หรืออาจ error ได้หากผู้พัฒนาไม่ได้สร้าง tag latest เอาไว้
3. **ตัวเลขฝั่งซ้ายและขวาของ ports หมายถึงอะไร ทำไมฝั่งขวาไม่ใช่ 80:** 
ฝั่งซ้าย (8081) คือ Port ของเครื่องโฮสต์ ส่วนฝั่งขวา (3000) คือ Port ของ Container ที่แอปพลิเคชันกำลังทำงานอยู่ ฝั่งขวาไม่ใช่ 80 เพราะเว็บเซิร์ฟเวอร์ (เช่น Node.js) ใน Image ของ Promo ถูกตั้งค่าให้รันบน Port 3000
4. **ถ้าแก้ index.html แล้วสั่งแค่ docker compose up -d จะเกิดอะไรขึ้น คำสั่งที่ถูกคืออะไร:** 
หน้าเว็บจะไม่เปลี่ยน เพราะ Docker จะใช้ Image ตัวเก่าที่เคย Build ไว้แล้ว คำสั่งที่ถูกต้องเพื่อให้รวมไฟล์ที่มีการแก้ไขเข้าไปด้วยคือ `docker compose up -d --build`
5. **ลิงก์ใช้ URL อะไร และทำไมใช้ http://promo:3000 ไม่ได้:** 
ใช้ `http://localhost:8081` สาเหตุที่ใช้ http://promo:3000 ไม่ได้ เพราะลิงก์ถูกคลิกและเปิดโดย Web Browser ของผู้ใช้ (Client-side) ซึ่งอยู่ภายนอก Docker Network จึงไม่รู้จักชื่อ Service ภายใน (promo) ต้องเข้าผ่าน Port ของเครื่องโฮสต์ที่ Map ไว้

## ภาพหน้าจอ

![Menu](screenshots/01-menu.png)
![Promo](screenshots/02-promo.png)