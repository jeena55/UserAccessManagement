
# Add, Delete, Modify Users and Groups

&nbsp;&nbsp;&nbsp;&nbsp;ในฐานะผู้ดูแลระบบ Linux จำเป็นต้องทราบวิธีเพิ่ม, แก้ไข และลบผู้ใช้ในระบบLinux ซึ่งเป็นการปฏิบัติที่ดีที่จะมีบัญชีที่แตกต่างกันสำหรับผู้ใช้แต่ละคนและตั้งสิทธิ์เพื่อเป็นการป้องกันทางด้านความปลอดภัย


&nbsp;&nbsp;&nbsp;&nbsp;Groups คือกลุ่มของผู้ใช้ การสร้างและการจัดการgroupsเป็นวิธีที่ง่ายที่สุดวิธีหนึ่งในการจัดการกับผู้ใช้หลายรายพร้อมๆกัน โดยเฉพาะอย่างยิ่งเมื่อต้องจัดการกับสิทธิ์ โดยทั่วไปไฟล์ <strong>/etc/group</strong> จะเก็บข้อมูลของกลุ่มและเป็นไฟล์การกำหนดค่าเริ่มต้น

>ผู้ดูแลระบบ Linux ใช้กลุ่มเพื่อกำหนดการเข้าถึงไฟล์และทรัพยากรอื่นๆ ทุกกลุ่มจะมี ID เฉพาะโดยแสดงอยู่ในไฟล์ <strong><em>/etc/group</em></strong> รวมถึงชื่อกลุ่มและสมาชิก กลุ่มแรกที่แสดงอยู่ในไฟล์นี้คือกลุ่มของระบบ เนื่องจากผู้ดูแลการแจกจ่ายกำหนดค่าไว้ล่วงหน้าสำหรับกิจกรรมของระบบ


ผู้ใช้แต่ละรายสามารถอยู่ในกลุ่มหลัก1กลุ่ม และกลุ่มรองได้หลายกลุ่ม หากคุณสร้างผู้ใช้บน Linux โดยใช้คำสั่ง 
- <strong>useradd</strong>

กลุ่มที่มีชื่อเดียวกับชื่อผู้ใช้จะถูกสร้างขึ้นและผู้ใช้จะถูกเพิ่มเป็นสมาชิกของกลุ่มนั้น ๆ กลุ่มนี้คือกลุ่มหลักของผู้ใช้

## Add User

หากต้องการเพิ่มผู้ใช้ ให้รันคำสั่ง <strong>useradd</strong> ดังนี้:<br>
```bat
cd \
copy a b
ping 192.168.0.1
```