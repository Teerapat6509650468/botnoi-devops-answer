# สำหรับ Devops

### 1. จงอธิบายการทำงานของภาพข้างด้านล่างนี้อย่างละเอียด
![Screenshot_20250416_204629_Docs](https://github.com/user-attachments/assets/ff390f80-b80e-4db6-8835-b3ba430500ad) 
ภาพนี้แสดงผลลัพธ์ของคำสั่ง `ip a` (ย่อจาก `ip address`) ที่รันในเทอร์มินัลของระบบ Linux ซึ่งใช้เพื่อแสดงข้อมูลของอินเตอร์เฟซเครือข่ายในระบบ Linux โดยข้อมูลในภาพมีรายละเอียดเกี่ยวกับ network interfaces ที่อยู่ในเครื่อง โดยเฉพาะ `lo` (loopback) และ `ens4` (network interface ที่เชื่อมต่อภายนอก)

ภายในกรอบสีเขียว แสดงการใช้คำสั่ง `ip a` โดยทั่วไปจะให้ข้อมูลต่อไปนี้
1. หมายเลขและชื่ออินเตอร์เฟซ
2. ค่า MTU และ queue discipline
3. ที่อยู่ IP ของแต่ละอินเตอร์เฟซ

คำสั่ง ip a ใช้สำหรับ:

- ดูสถานะของ network interfaces
- ตรวจสอบ IP address ที่กำหนดไว้
- ตรวจสอบ IPv4 และ IPv6
- ใช้แทนคำสั่งเก่า ifconfig

ก่อนกรอบสีเหลือง คือ รายละเอียดเกี่ยวกับข้อมูลที่ปรากฏของ Interface `lo`
```
1: lo: <LOOPBACK,UP,LOWER_UP> ...
```
- `lo` คือ loopback interface ซึ่งใช้สำหรับการสื่อสารภายในเครื่องเอง (localhost)
- สถานะ: `UP` แสดงว่า interface ทำงานอยู่
- มี IP:
  - `127.0.0.1/8` (IPv4) — ใช้เรียกตัวเอง
  - `::1/128` (IPv6) — ใช้ใน IPv6 เพื่อเรียกตัวเอง

ภายในกรอบสีเหลือง แสดงข้อมูลเกี่ยวกับ Interface `ens4` 
```
2: ens4: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1460 ...
```
- `ens4` คือชื่อของ network interface ที่เชื่อมต่อภายนอก
- สถานะ: `UP` และ `LOWER_UP` หมายถึง interface กำลังทำงานและมีสายหรือสัญญาณเชื่อมต่ออยู่
- `mtu 1460`: Maximum Transmission Unit — ขนาดสูงสุดของ packet ที่สามารถส่งได้

ภายในกรอบสีม่วง แสดงข้อมูลเกี่ยวกับ IPv4 Address
```
inet 10.0.1.7/32 metric 100 scope global dynamic ens4
```
- `inet` แสดงว่าเป็น IPv4 address
- IP คือ `10.0.1.7/32` — ซึ่ง `/32` หมายถึง subnet mask 255.255.255.255 (host เดี่ยว)
- `metric 100`: ค่าลำดับความสำคัญของเส้นทาง routing (ยิ่งต่ำยิ่งมี priority สูง)
- `dynamic`: แสดงว่า IP ได้รับมาจาก DHCP server
- `scope global`: เป็น IP ที่ใช้ในเครือข่ายภายนอก (ไม่ใช่แค่ภายในเครื่อง)

ภายในกรอบสีแดง แสดงข้อมูลเกี่ยวกับ IPv6 Address
```
inet6 fe80::4001:aff:fe00:107/64 scope link
```
- `inet6` หมายถึง IPv6 address
- `fe80::...` เป็น link-local address ใช้สื่อสารภายใน segment เดียวกันของเครือข่าย
- `scope link` แสดงว่า IP นี้ใช้ภายในลิงก์ (interface เดียวกันเท่านั้น)

---

### 2. จงอธิบายการทำงานของภาพข้างด้านล่างนี้อย่างละเอียด
![Screenshot_20250416_204657_Docs](https://github.com/user-attachments/assets/9a8abd40-8690-47ff-891e-e586f6deec73)
ภาพนี้แสดงถึงโครงสร้างของไฟล์ระบบในระบบปฏิบัติการ Linux หรือ Unix ซึ่งเริ่มต้นจาก root directory (/) และแยกออกเป็นโฟลเดอร์ต่าง ๆ ที่มีหน้าที่และความสำคัญเฉพาะด้าน โดยแต่ละส่วนมีรายละเอียดดังนี้

**Root Directory `/`**
- คือไดเรกทอรีหลักสุดของระบบ
- ทุกไฟล์และไดเรกทอรีย่อยจะอยู่ภายใต้ `/` นี้

**`/bin`**
- ย่อมาจาก **binary**
- เก็บคำสั่งพื้นฐาน เช่น `ls`, `cp`, `mv`, `cat`, `mkdir`

**`/boot`**
- เก็บไฟล์ที่ใช้สำหรับ boot เครื่อง
- เช่น `vmlinuz`, `initrd.img`, `grub`

**`/dev`**
- ย่อมาจาก **devices**
- แทนอุปกรณ์ฮาร์ดแวร์ในระบบ เช่น `/dev/sda`, `/dev/usb`

**`/etc`**
- เก็บไฟล์ **คอนฟิกของระบบ**
- เช่น network, user, service settings

**`/home`**
- เก็บโฟลเดอร์ส่วนตัวของผู้ใช้
- เช่น `/home/user1`, `/home/user2`

**`/lib`**
- เก็บ shared libraries (.so) ที่ใช้ร่วมกับคำสั่งใน `/bin` และ `/sbin`

**`/media` และ `/mnt`**
- สำหรับ **mount** อุปกรณ์ภายนอก (USB, CD)
- `/media` มักใช้ mount แบบอัตโนมัติ
- `/mnt` ใช้สำหรับ mount ชั่วคราว

**`/opt`**
- เก็บแอปพลิเคชันเสริมหรือ third-party software
- เช่น `/opt/google/`

**`/root`**
- โฟลเดอร์ส่วนตัวของ **ผู้ดูแลระบบ (root)**
- แตกต่างจาก `/home` ของผู้ใช้ทั่วไป

**`/sbin`**
- คำสั่งสำหรับ root เช่น `shutdown`, `reboot`
- ใช้สำหรับจัดการระบบ

**`/srv`**
- เก็บข้อมูลของ services ที่ให้บริการ เช่น web, ftp
- เช่น `/srv/www/`

**`/tmp`**
- ไฟล์ชั่วคราว
- ระบบจะลบออกหลัง reboot

**`/usr`**
- ย่อมาจาก **Unix System Resources**
- เก็บไฟล์ของผู้ใช้ทั่วไปและโปรแกรมต่าง ๆ
- ภายในประกอบด้วย:
  - `/usr/bin` : คำสั่งทั่วไป
  - `/usr/sbin` : คำสั่งของ root
  - `/usr/lib` : ไลบรารี
  - `/usr/include` : header files สำหรับเขียนโปรแกรม

**/var`**
- ย่อมาจาก **variable**
- เก็บข้อมูลที่เปลี่ยนแปลงบ่อย
- ภายในประกอบด้วย:
  - `/var/log` : ไฟล์ log
  - `/var/cache` : ข้อมูลแคช
  - `/var/spool` : งานรอคิว
  - `/var/tmp` : ไฟล์ชั่วคราว (ระยะยาวกว่า `/tmp`)

***

### 3. จงอธิบายการทำงานของภาพข้างด้านล่างนี้อย่างละเอียดและเป็นการทำงานของอะไร
![Screenshot_20250412_154745_Docs](https://github.com/user-attachments/assets/4e61bc3b-2814-4e9c-b24d-f964bb5bd354)
Lorem Ipsum
