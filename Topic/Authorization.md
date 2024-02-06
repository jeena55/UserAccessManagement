## Authorization
<p>การอนุญาต (Authorization) เป็นขั้นตอนหนึ่งในกระบวนการ User/Access Management ที่เกี่ยวข้องกับการกำหนดสิทธิ์และการควบคุมการเข้าถึงข้อมูลหรือทรัพยากรในระบบคอมพิวเตอร์หรือเครือข่าย โดยมีวัตถุประสงค์เพื่อปกป้องข้อมูลและรักษาความปลอดภัยของระบบ</p>
<p>
การอนุญาตมีลักษณะการกำหนดสิทธิ์ (permissions) หรือกำหนดการเข้าถึง (access rights) ให้แก่ผู้ใช้หรือกลุ่มผู้ใช้ที่ต้องการเข้าถึงข้อมูลหรือทรัพยากรต่าง ๆ ในระบบ โดยทำให้ระบบรู้ว่าผู้ใช้นั้น ๆ มีสิทธิ์ในการทำบางอย่างบนระบบหรือไม่ เช่น การอนุญาตสามารถเป็นระดับการเข้าถึงทั้งระบบ (system-wide) หรือเฉพาะกลุ่มทรัพยากรหรือฟังก์ชันที่กำหนดไว้ </p> <br>
<p>ตัวอย่างการอนุญาต：</p>
    
- **การกำหนดสิทธิ์ในการอ่าน (Read):** ผู้ใช้ที่ได้รับอนุญาตสามารถดูข้อมูลหรือทรัพยากรที่เฉพาะเจาะจงได้
    
- **การกำหนดสิทธิ์ในการเขียน (Write):** ผู้ใช้ที่ได้รับอนุญาตสามารถแก้ไขหรือบันทึกข้อมูลได้
    
- **การกำหนดสิทธิ์ในการดำเนินการ (Execute):** ผู้ใช้ที่ได้รับอนุญาตสามารถรันหรือทำงานกับโปรแกรมหรือสคริปต์ที่เฉพาะเจาะจงได้
    
- **การกำหนดสิทธิ์ในการลบ (Delete):** ผู้ใช้ที่ได้รับอนุญาตสามารถลบข้อมูลหรือทรัพยากรที่เฉพาะเจาะจงได้ <br>    
    
<p>การอนุญาตมีบทบาทสำคัญในการควบคุมความเข้าถึงข้อมูลและป้องกันการไม่ได้รับอนุญาตในการเข้าถึงทรัพยากรที่สำคัญของระบบคอมพิวเตอร์หรือเครือข่าย ซึ่งช่วยรักษาความปลอดภัยและควบคุมการใช้งานในระบบ
 </p>
<br>
<p><b>sudoers:</b> ไฟล์ sudoers เป็นไฟล์การกำหนดค่าที่กำหนดกฎและนโยบายสำหรับอนุญาตให้ผู้ใช้หรือกลุ่มทำงานบางอย่างด้วยสิทธิพิเศษโดยใช้คำสั่ง sudo <br>
<b>ตัวอย่างการใช้งาน:</b>แก้ไขไฟล์ sudoers โดยใช้คำสั่ง visudo (เพื่อป้องกันข้อผิดพลาดของไวยากรณ์) เพื่ออนุญาตให้ผู้ใช้ชื่อ สามารถรันคำสั่งใด ๆ ด้วยสิทธิ sudo <br></p>
<pre>
    <code> john   ALL=(ALL:ALL) ALL </code>
</pre>
<p><b>chown: </b> เปลี่ยนเจ้าของของไฟล์หรือไดเรกทอรี <br>
<b>ตัวอย่างการใช้งาน: </b> เปลี่ยนเจ้าของของไฟล์ชื่อ "example.txt" เป็นผู้ใช้ "user1" </p>
<pre>
    <code> sudo chown user1 example.txt </code>
</pre>
<p><b>chgrp: </b> เปลี่ยนเจ้าของกลุ่มของไฟล์หรือไดเรกทอรี<br>
<b>ตัวอย่างการใช้งาน: </b>  เปลี่ยนเจ้าของกลุ่มของไฟล์ชื่อ "example.txt" เป็นกลุ่ม "staff": </p>
<pre>
    <code> sudo chgrp staff example.txt </code>
</pre>
<p><b>chmod: </b>  ปรับเปลี่ยนสิทธิ์ (อ่าน เขียน ดำเนินการ) ของไฟล์หรือไดเรกทอรี <br>
<b>ตัวอย่างการใช้งาน: </b> ให้สิทธิ์อ่านและเขียนถึงเจ้าของของไฟล์ชื่อ "example.txt":</p>
<pre>
    <code> sudo chmod u+rw example.txt </code>
</pre>
<p><b>getfacl: </b> แสดงรายการควบคุมการเข้าถึง (ACLs) ของไฟล์และไดเรกทอรี <br>
<b>ตัวอย่างการใช้งาน: </b> ดู ACLs ของไฟล์ชื่อ "example.txt"</p>
<pre>
    <code> getfacl example.txt </code>
</pre>
<p><b>setfacl: </b> ตั้งค่ารายการควบคุมการเข้าถึง (ACLs) สำหรับไฟล์และไดเรกทอรี<br>
<b>ตัวอย่างการใช้งาน: </b> ให้สิทธิ์อ่านและเขียนถึงผู้ใช้ที่ระบุบนไฟล์ชื่อ "example.txt": </p> <br<br>
<pre>
    <code> sudo setfacl -m u:ชื่อผู้ใช้:rw- example.txt </code>
</pre>
<p> คำสั่งเหล่านี้เป็นเครื่องมือสำคัญในการจัดการสิทธิ์การเข้าถึงไฟล์และไดเรกทอรี โดยมีเป้าหมายเพื่อรักษาความปลอดภัยของระบบและทรัพยากรข้อมูลอย่างเหมาะสม อย่างไรก็ตาม ควรใช้คำสั่งเหล่านี้อย่างระมัดระวังเสมอ โดยเฉพาะเมื่อมีสิทธิ์พิเศษ เพื่อป้องกันการปรับเปลี่ยนโดยไม่ตั้งใจหรือไม่ได้รับอนุญาตในการเปลี่ยนแปลงแบบไม่พึงประสงค์หรือไม่มีอำนาจ </p>