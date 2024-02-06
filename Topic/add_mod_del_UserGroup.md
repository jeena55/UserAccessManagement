
# Add, Delete, Modify Users and Groups

&nbsp;&nbsp;&nbsp;&nbsp;ในฐานะผู้ดูแลระบบ Linux จำเป็นต้องทราบวิธีเพิ่ม, แก้ไข และลบผู้ใช้ในระบบLinux ซึ่งเป็นการปฏิบัติที่ดีที่จะมีบัญชีที่แตกต่างกันสำหรับผู้ใช้แต่ละคนและตั้งสิทธิ์เพื่อเป็นการป้องกันทางด้านความปลอดภัย


&nbsp;&nbsp;&nbsp;&nbsp;Groups คือกลุ่มของผู้ใช้ การสร้างและการจัดการgroupsเป็นวิธีที่ง่ายที่สุดวิธีหนึ่งในการจัดการกับผู้ใช้หลายรายพร้อมๆกัน โดยเฉพาะอย่างยิ่งเมื่อต้องจัดการกับสิทธิ์ โดยทั่วไปไฟล์ <code style="color:darkorange">/etc/group</code> จะเก็บข้อมูลของกลุ่มและเป็นไฟล์การกำหนดค่าเริ่มต้น

>ผู้ดูแลระบบ Linux ใช้กลุ่มเพื่อกำหนดการเข้าถึงไฟล์และทรัพยากรอื่นๆ ทุกกลุ่มจะมี ID เฉพาะโดยแสดงอยู่ในไฟล์ <code style="color:darkorange">/etc/group</code> รวมถึงชื่อกลุ่มและสมาชิก กลุ่มแรกที่แสดงอยู่ในไฟล์นี้คือกลุ่มของระบบ เนื่องจากผู้ดูแลการแจกจ่ายกำหนดค่าไว้ล่วงหน้าสำหรับกิจกรรมของระบบ


ผู้ใช้แต่ละรายสามารถอยู่ในกลุ่มหลัก1กลุ่ม และกลุ่มรองได้หลายกลุ่ม หากคุณสร้างผู้ใช้บน Linux โดยใช้คำสั่ง 
- <code style="color:darkorange">useradd</code>

กลุ่มที่มีชื่อเดียวกับชื่อผู้ใช้จะถูกสร้างขึ้นและผู้ใช้จะถูกเพิ่มเป็นสมาชิกของกลุ่มนั้น ๆ กลุ่มนี้คือกลุ่มหลักของผู้ใช้

# Add User

### หากต้องการเพิ่มผู้ใช้ ให้รันคำสั่ง <code style="color:darkorange">useradd</code> ดังนี้:<br>
```bash
sudo useradd -m <name of the user>
```
หากต้องการเพิ่มผู้ใช้โดยให้สร้างโฮมไดเรกทอรี ให้ใช้คำสั่ง <code style="color:darkorange">useradd</code> โดยใช้ <code style="color:darkorange">-m</code>

>ถ้าคำสั่งสำเร็จ จะไม่มีใดๆผลลัพธ์ออกมา

ระบบจะสร้างผู้ใช้โดยอัตโนมัติ โดยการกำหนด ID ผู้ใช้เฉพาะสำหรับผู้ใช้ และเพิ่มรายละเอียดของผู้ใช้ลงในไฟล์ /etc/passwd นอกจากนี้ยังสร้างโฮมไดเร็กตอรี่สำหรับผู้ใช้ภายใต้ <code style="color:darkorange">/home/<name of the user\></code>

ณ จุดนี้ ผู้ใช้ได้ถูกสร้างขึ้นแล้ว แต่ไม่มีรหัสผ่านและไม่สามารถเข้าสู่ระบบได้ ดังนั้น 

### หากต้องการกำหนดรหัสผ่านให้กับผู้ใช้ที่สร้างขึ้นใหม่ ให้รันคำสั่ง passwd ดังนี้:
```bash
sudo passwd <username>
```
หลังจากนั้น ระบบจะถามคุณให้ป้อนรหัสผ่านสำหรับผู้ใช้ และยืนยันรหัสผ่านอีกครั้ง

>คำสั่งนี้จะเพิ่มรหัสผ่านของผู้ใช้ใน <code style="color:darkorange">/etc/shadow</code> ในรูปแบบที่เข้ารหัส หลังจากรันคำสั่งนี้แล้ว ผู้ใช้ใหม่ควรจะสามารถเข้าสู่ระบบได้ตามปกติ

คุณสามารถดู ID ผู้ใช้ใหม่ได้โดยใช้ 
- <code style="color:darkorange">id -u \<username\>.</code>

# Change Password of a User

ก่อนหน้านี้เมื่อเราสร้างผู้ใช้ใหม่ เราได้ใช้คำสั่ง <code style="color:darkorange">passwd</code> เพื่อกำหนดรหัสผ่านให้กับผู้ใช้ใหม่ คุณเปลี่ยนรหัสผ่านได้ของผู้ใช้ได้โดย:  
```bash
    passwd
```
>เมื่อคุณเปลี่ยนรหัสผ่านของคุณเอง ระบบจะถามรหัสผ่านปัจจุบันของคุณ เมื่อคุณป้อนอย่างถูกต้อง คุณจะถูกขอให้ป้อนรหัสผ่านใหม่สองครั้ง

คุณยังสามารถระบุชื่อผู้ใช้เพื่อเปลี่ยนรหัสของผู้ใช้นั้น แม้ว่าคุณจะต้องรูทเพื่อเปลี่ยนรหัสผ่านให้ผู้อื่นก็ตาม:
```bash
    sudo passwd <username>
```

>เมื่อคุณเป็น root-user คำสั้ง <code style="color:darkorange">passwd</code> จะไม่ถามรหัสผ่านปัจจุบันของคุณ แต่จะถามรหัสผ่านใหม่จากคุณเท่านั้น

คุณยังสามารถใช้ <code style="color:darkorange">passwd</code> เพื่อป้องกันไม่ให้ผู้ใช้เข้าสู่ระบบ (หรือที่เรียกว่า <strong>"ล็อคผู้ใช้"</strong>) โดยใช้ -l ตัวอย่างเช่น หากคุณต้องการป้องกันไม่ให้ sprite เข้าสู่ระบบ คุณสามารถใช้:
```bash
    sudo passwd -l sprite
```

# Grant Sudo Permissions to Users

<code style="color:darkorange">sudo</code>เป็นโปรแกรมอรรถประโยชน์ที่ช่วยให้ผู้ใช้สามารถรันคำสั่งในฐานะผู้ใช้รายอื่น ซึ่งโดยปกติจะเป็น <code style="color:darkorange">root-user</code> มีเพียงผู้ใช้บางกลุ่มเท่านั้นที่สามารถดำเนินการ <code style="color:darkorange">sudo</code>ได้

### หากคุณต้องการให้ผู้ใช้ (เช่น sprite) สามารถใช้ <code style="color:darkorange">sudo</code>ได้ คุณสามารถใช้ <code style="color:darkorange">usermod</code>เพื่อเพิ่มพวกเขาเข้าในกลุ่ม <code style="color:darkorange">sudo</code>ได้ดังนี้:
```bash
    sudo usermod -a -G sudo sprite
```

สำหรับ CentOS หรือ RHEL:
```bash
    sudo usermod -a -G wheel sprite
```

### จะเป็นอย่างไร
ถ้าคุณไม่ใช้บางอย่างตามที่ <code style="color:darkorange">Debian</code> หรือ <code style="color:darkorange">CentOS</code> กำหนด แม้ว่าการกำหนดค่า <code style="color:darkorange">sudo</code>เริ่มต้นอาจแตกต่างกันมากในแต่ละรุ่น แต่ขั้นตอนด้านล่างนี้จะช่วยให้คุณเข้าใจได้มากขึ้น

1. ขั้นแรก คุณควรสร้างกลุ่มของคุณเอง เช่น <code style="color:darkorange">sysadmins</code> และเพิ่มผู้ใช้เข้าไป เช่นเดียวกับที่เราเคยทำก่อนหน้านี้ จากนั้นคุณสามารถแก้ไขไฟล์ /etc/sudoers ในฐานะ<code style="color:darkorange">root-user</code>เพื่ออนุญาตให้ใครก็ตามที่เป็น <code style="color:darkorange">sysadmins</code> สามารถเข้าถึง <code style="color:darkorange">sudo</code>ได้ หากต้องการแก้ไขไฟล์ คุณสามารถใช้โปรแกรมแก้ไขเช่น nano หรือ vi ได้โดยการเรียกใช้:
```bash
    sudo nano /etc/sudoders # หากติดตั้ง nano แล้ว
    sudo vi /etc/sudoers # หากติดตั้ง vi แล้ว
```
2. จากนั้นไปที่ท้ายไฟล์แล้วเพิ่มข้อความต่อไปนี้ในบรรทัดของตัวเอง สิ่งนี้จะอนุญาตให้ใครก็ตามที่เป็น<code style="color:darkorange">sysadmins</code>สามารถใช้ <code style="color:darkorange">sudo</code>:
```bash
    %sysadmins ALL=(ALL) ALL
```
3. บันทึกไฟล์และออกจากโปรแกรมแก้ไข หลังจากนี้ ผู้ใช้ใน <code style="color:darkorange">sysadmins</code> จะสามารถใช้ <code style="color:darkorange">sudo</code>เพื่อรันคำสั่งได้

# Delete a User

### หากต้องการลบผู้ใช้ใน Linux คุณสามารถใช้คำสั่ง <code style="color:darkorange">userdel</code> ได้ดังนี้:
```bash
    sudo userdel -r <username>
```
ตามค่าเริ่มต้น คำสั่งนี้จะเก็บรักษาโฮมไดเร็กตอรี่และไฟล์พิเศษอื่นๆ เช่น list of cron jobs ของผู้ใช้ หากคุณต้องการลบไฟล์เหล่านี้ด้วย คุณควรใช้flag     <code style="color:darkorange">--remove-all-files</code>


# User Groups

Groups คือกลุ่มของผู้ใช้ groups สามารถกำหนดผู้ใช้ได้เป็นศูนย์หรือมากกว่านั้น แต่ละกลุ่มจะมี <strong>“ชื่อกลุ่ม”</strong> และ <strong>“รหัสกลุ่ม”</strong> ที่เป็นเอกลักษณ์ของตัวเอง กลุ่มใช้เพื่อกำหนดสิทธิ์แก่ผู้ใช้, การเข้าถึง หรือสิทธิ์พิเศษ

### แบ่งเป็น 2 ประเภท คือ

1. <strong>Primary Group:</strong> 
 - เมื่อสร้างผู้ใช้ ระบบจะกำหนดกลุ่มเริ่มต้นกลุ่มเดียวให้ผู้ใช้โดยอัตโนมัติ ซึ่งเรียกว่า "กลุ่มหลัก" โดยปกติแล้ว ชื่อของกลุ่มหลักจะเหมือนกับชื่อผู้ใช้ แต่คุณสามารถเปลี่ยนได้หากต้องการ

2. <strong>Supplementary Group:</strong>
- นอกเหนือจากกลุ่มหลักแล้ว คุณสามารถเพิ่มผู้ใช้ในกลุ่มอื่นๆ ได้ กลุ่มอื่นๆ ที่มีผู้ใช้อยู่เรียกว่ากลุ่มเสริม

>ข้อมูลเกี่ยวกับกลุ่มทั้งหมดในระบบของคุณจะถูกจัดเก็บไว้ใน <code style="color:darkorange">/etc/group</code> กลุ่มยังสามารถมีรหัสผ่านได้ โดยระบบจะจัดเก็บไว้ใน <code style="color:darkorange">/etc/gshadow</code>

# Create a new Group

### หากต้องการสร้างกลุ่มใหม่ใน Linux ให้รันคำสั่ง <code style="color:darkorange">groupadd</code> ดังนี้:
```bash
    sudo groupadd <groupname>
```
>เช่นเดียวกับคำสั่ง <code style="color:darkorange">useradd</code>, <code style="color:darkorange">groupadd</code> จะไม่แสดงผลใด ๆ หากรันคำสั่งสำเร็จ:

หากคุณต้องการตรวจสอบว่ากลุ่มนั้นถูกสร้างขึ้นจริง สามารถตรวจสอบได้ที่ <code style="color:darkorange">/etc/groups</code>

### หากคุณต้องการกำหนดรหัสผ่านกลุ่ม คุณสามารถใช้คำสั่ง <code style="color:darkorange">gpasswd</code> ได้:

```bash
    sudo gpasswd <groupname>
```

ป้อนรหัสผ่านของกลุ่มและพิมพ์อีกครั้งเพื่อยืนยัน เมื่อเสร็จสิ้นกระบวนการ รหัสผ่านของกลุ่มจะถูกเข้ารหัสและเก็บไว้ใน <code style="color:darkorange">/etc/gshadow</code>

# View a user’s groups and user ID

### หากต้องการดูข้อมูลของผู้ใช้ เช่น ID ของผู้ใช้ หรือกลุ่มของผู้ใช้ คุณสามารถใช้คำสั่ง <code style="color:darkorange">id</code> ได้:
```bash
    id 
```
>ผลลัพธ์จะแสดง ID ผู้ใช้ของคุณ <strong>(uid)</strong> และ ID ของกลุ่มหลัก <strong>(gid)</strong> รวมถึงรายการของกลุ่มหลักและกลุ่มเสริมที่คุณเป็นสมาชิก

ในทางกลับกัน หากคุณต้องการดูข้อมูลสำหรับผู้ใช้รายอื่น ให้ใช้คำสั่งต่อไปนี้:
```bash
    id <username>
```
>โดยจะแสดง<code style="color:darkorange">uid</code> <code style="color:darkorange">gid</code> รวมถึงชื่อกลุ่มหลักและกลุ่มเสริมที่ผู้ใช้เป็นสมาชิก:

### หากคุณต้องการดูเฉพาะกลุ่มที่ผู้ใช้เป็นสมาชิก คุณสามารถใช้คำสั่ง<code style="color:darkorange">groups</code>ได้ คล้ายกับ <code style="color:darkorange">id</code> โดยจะแสดงรายการกลุ่มของคุณเองตามค่าเริ่มต้น

หากคุณต้องการดูกลุ่มของผู้ใช้รายอื่น ให้ใช้:
```bash
    groups <username>
```

# Add a User to a Group

เมื่อคุณมีความรู้พื้นฐานเกี่ยวกับกลุ่มแล้ว เราสามารถแก้ไขผู้ใช้และกำหนดผู้ใช้ให้กับกลุ่มได้ 

### หากต้องการเพิ่มผู้ใช้ในกลุ่ม:
```bash
    sudo usermod -a -G <groupname> <username>
```
>ในที่นี้ <code style="color:darkorange">-a</code> <strong>“เพิ่ม”</strong> ผู้ใช้ไปยังกลุ่ม และ -G บ่งบอกว่าเรากำลังเพิ่มพวกเขาเข้าในกลุ่มเสริม (ซึ่งตรงข้ามกับการเปลี่ยนกลุ่มหลัก)

### หากคุณต้องการเปลี่ยนกลุ่มหลักของผู้ใช้ คุณสามารถใช้ <code style="color:darkorange">-g</code> ดังนี้:
```bash
    sudo usermod -g <primary-groupname> <username>
```
>ในคำสั่งข้างต้น โปรดสังเกตว่าเราไม่ได้ใช้ <code style="color:darkorange">-a</code> ต่อท้าย เนื่องจากเราต้องการเพียงเปลี่ยนกลุ่มหลักของผู้ใช้ และเราไม่ได้เพิ่มผู้ใช้เข้ากลุ่ม ตามคำจำกัดความแล้ว <strong>"กลุ่มหลักจะมีผู้ใช้ได้เพียงคนเดียวเท่านั้น"</strong>

# Reference
- <a href="https://https://www.booleanworld.com/how-to-add-remove-and-modify-users-in-linux/">Boolean World</a>
- <a href="https://www.redhat.com/sysadmin/linux-groups">RedHat</a>
