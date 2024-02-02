#user policy, password policy

<h1>User Policy</h1>

<div>
    <p>เมื่อมีการสร้างผู้ใช้ใหม่ ยูทิลิตี้ adduser จะสร้างโฮมไดเร็กตอรี่ใหม่ชื่อ/home/username. โปรไฟล์เริ่มต้นจะถูกจำลองตาม เนื้อหาที่พบในไดเร็กทอรีของ/etc/skelซึ่งรวมถึงข้อมูลพื้นฐานของโปรไฟล์ทั้งหมด</p>
    <p>หากเซิร์ฟเวอร์ของคุณจะเป็นบ้านสำหรับผู้ใช้หลายคน คุณควรใส่ใจกับการอนุญาตโฮมไดเร็กตอรี่ของผู้ใช้อย่างใกล้ชิดเพื่อให้แน่ใจว่าจะเป็นความลับ ตามค่าเริ่มต้น โฮมไดเร็กทอรีของผู้ใช้ใน Ubuntu จะถูกสร้างขึ้นด้วยสิทธิ์การอ่าน/ดำเนินการทั่วโลก ซึ่งหมายความว่าผู้ใช้ทุกคนสามารถเรียกดูและเข้าถึงเนื้อหาของโฮมไดเร็กทอรีของผู้ใช้รายอื่นได้ สิ่งนี้อาจไม่เหมาะกับสภาพแวดล้อมของคุณ</p>
</div>

<br>

<div>
   <li>หากต้องการตรวจสอบสิทธิ์โฮมไดเร็กทอรีของผู้ใช้ปัจจุบันของคุณ ให้ใช้ไวยากรณ์ต่อไปนี้:</li>
    <pre>
        <code>ls -ld /home/username</code>
    </pre> 
    <p>ผลลัพธ์ต่อไปนี้แสดงว่าไดเร็กทอรี/home/usernameมีสิทธิ์ที่สามารถอ่านได้ทั่วโลก:</p>
     <pre>
        <code>drwxr-xr-x  2 username username    4096 2007-10-02 20:03 username</code>
    </pre> 
</div>

<br>

<div>
   <li>คุณสามารถลบสิทธิ์การอ่านของโลกได้โดยใช้ไวยากรณ์ต่อไปนี้:</li>
    <pre>
        <code>lssudo chmod 0750 /home/username</code>
    </pre> 
    <blockquote>
        <p><b>บันทึก</b></p>
        <p>บางคนมีแนวโน้มที่จะใช้ตัวเลือกแบบเรียกซ้ำ (-R) โดยไม่เลือกปฏิบัติ ซึ่งจะแก้ไขโฟลเดอร์และไฟล์ย่อยทั้งหมด แต่ไม่จำเป็น และอาจให้ผลลัพธ์ที่ไม่พึงประสงค์อื่นๆ ไดเร็กทอรีพาเรนต์เพียงอย่างเดียวก็เพียงพอแล้วสำหรับการป้องกันการเข้าถึงสิ่งใดก็ตามที่อยู่ต่ำกว่าพาเรนต์โดยไม่ได้รับอนุญาต</p>
    </blockquote>
<p>แนวทางที่มีประสิทธิภาพมากขึ้นในเรื่องนี้คือการแก้ไขการอนุญาตเริ่มต้นส่วนกลางของ adduser เมื่อสร้างโฮมโฟลเดอร์ของผู้ใช้ เพียงแก้ไขไฟล์/etc/adduser.confและแก้ไขDIR_MODEตัวแปรให้เหมาะสม เพื่อให้โฮมไดเร็กทอรีใหม่ทั้งหมดได้รับการอนุญาตที่ถูกต้อง</p>
    <pre>
        <code>DIR_MODE=0750</code>
    </pre> 
</div>

<br>

<div>
    <li>หลังจากแก้ไขการอนุญาตไดเรกทอรีโดยใช้เทคนิคใดๆ ที่กล่าวถึงก่อนหน้านี้ ให้ตรวจสอบผลลัพธ์โดยใช้ไวยากรณ์ต่อไปนี้:</li>
    <pre>
        <code>ls -ld /home/username</code>
    </pre> 
    <p>ผลลัพธ์ด้านล่างแสดงว่าการอนุญาตที่โลกอ่านได้ได้ถูกลบออกแล้ว:</p>
     <pre>
        <code>drwxr-x---   2 username username    4096 2007-10-02 20:03 username</code>
    </pre> 
</div>

<h1>วิธีเปิดใช้งานและบังคับใช้นโยบายรหัสผ่านที่ปลอดภัย</h1>
<div>
    <p>Ubuntu วิธีเปิดใช้งานและบังคับใช้นโยบายรหัสผ่านที่ปลอดภัยบน Ubuntu 4 ปีที่แล้วโดยคาริม บุซดาร์ รหัสผ่านที่ปลอดภัยเป็นด่านแรกในการป้องกันการเข้าถึงโดยไม่ได้รับอนุญาต ไม่ว่าจะเป็นคอมพิวเตอร์ส่วนบุคคลหรือเซิร์ฟเวอร์ในองค์กรของคุณ อย่างไรก็ตาม พนักงานบางคนไม่ได้จริงจังกับเรื่องนี้ และใช้รหัสผ่านที่ไม่ปลอดภัยและคาดเดาได้ง่าย ซึ่งทำให้ระบบของพวกเขาถูกบุกรุก ดังนั้นจึงเป็นเรื่องสำคัญสำหรับผู้ดูแลระบบในการบังคับใช้นโยบายรหัสผ่านที่ปลอดภัยสำหรับผู้ใช้ นอกจากนี้ การเปลี่ยนรหัสผ่านหลังจากช่วงระยะเวลาหนึ่งเป็นสิ่งสำคัญ</p>
    <blockquote>
        <p><b>How do I enforce a password complexity policy?</b></p>
        <li>at least one upper case (อย่างน้อยหนึ่งตัวพิมพ์ใหญ่)</li>
        <li>at least one lower case (อย่างน้อยหนึ่งตัวพิมพ์เล็ก)</li>
        <li>at least one digit (อย่างน้อยหนึ่งหลัก)</li>
        <li>at least one special character (อักขระพิเศษอย่างน้อยหนึ่งตัว)</li>
    </blockquote>
    <p>เพื่อบังคับใช้นโยบายรหัสผ่านที่ปลอดภัยใน Ubuntu เราจะใช้โมดูล pwquality ของ PAM หากต้องการติดตั้งโมดูลนี้ ให้เปิด Terminal โดยใช้ทางลัด Ctrl+Alt+T จากนั้นรันคำสั่งนี้ใน Terminal:</p>
    <pre>
        <code>$ sudo apt ติดตั้ง libpam-pwquality</code>
    </pre> 
    <p>เมื่อได้รับแจ้งให้ใส่รหัสผ่าน ให้ป้อนรหัสผ่าน sudo</p>
    <img src="https://linuxhint.com/wp-content/uploads/2020/03/1-34.png">
    <br><br>
    <p>ขั้นแรกให้คัดลอกไฟล์ “/etc/pam.d/common-password” ก่อนกำหนดค่าการเปลี่ยนแปลงใดๆ</p>
    <img src="https://linuxhint.com/wp-content/uploads/2020/03/2-35.png">
    <p>จากนั้นแก้ไขเพื่อกำหนดค่านโยบายรหัสผ่าน:</p>
    <pre>
        <code>$ sudo nano /etc/pam.d/common-password</code>
    </pre> 
    <p>มองหาบรรทัดต่อไปนี้:</p>
    <pre>
        <code>Password   requisite   pam_pwquality.so retry=3</code>
    </pre>
    <p>และแทนที่ด้วยสิ่งต่อไปนี้:</p>
    <pre>
        <code>password        requisite
        pam_pwquality.so retry=4 minlen=9 difok=4 lcredit=-2 ucredit=-2 dcredit=-1 ocredit=-1 reject_username enforce_for_root</code><
    </pre>
    <image src="https://linuxhint.com/wp-content/uploads/2020/03/3-31.png"></image>
    <br><br>
    <h3>มาดูกันว่าพารามิเตอร์ในคำสั่งด้านบนหมายถึงอะไร:</h3>
    <table>
        <tr>
            <th>Parameter</th>
            <th>description</th>
        </tr>
        <tr>
            <td>retry</td>
            <td>จำนวนครั้งติดต่อกันที่ผู้ใช้สามารถป้อนรหัสผ่านไม่ถูกต้อง</td>
        </tr>
        <tr>
            <td>minlen</td>
            <td>ความยาวขั้นต่ำของรหัสผ่าน</td>
        </tr>
        <tr>
            <td>difok</td>
            <td>จำนวนตัวอักษรที่สามารถคล้ายกับรหัสผ่านเก่าได้</td>
        </tr><tr>
            <td>lcredit</td>
            <td>จำนวนขั้นต่ำของตัวอักษรตัวพิมพ์เล็ก</td>
        </tr>
        <tr>
            <td>ucredit</td>
            <td>จำนวนขั้นต่ำของตัวอักษรตัวพิมพ์ใหญ่</td>
        </tr>
        <tr>
            <td>credit</td>
            <td>จำนวนขั้นต่ำของหลัก</td>
        </tr><tr>
            <td>ocredit</td>
            <td>จำนวนสัญลักษณ์ขั้นต่ำ</td>
        </tr>
        <tr>
            <td>reject_username</td>
            <td>ปฏิเสธรหัสผ่านที่มีชื่อผู้ใช้</td>
        </tr><tr>
            <td>enforce_for_root</td>
            <td>บังคับใช้นโยบายสำหรับผู้ใช้รูทด้วย</td>
        </tr>
    </table>
    <h3>ตอนนี้รีบูทระบบเพื่อใช้การเปลี่ยนแปลงในนโยบายรหัสผ่าน</h3>
    <pre>
        <code>$ sudoรีบูต</code>
    </pre>  
</div>

<div>
    <h2>ทดสอบนโยบายรหัสผ่านที่ปลอดภัย</h2>
    <p>หลังจากกำหนดนโยบายรหัสผ่านที่ปลอดภัยแล้ว ควรตรวจสอบว่าใช้งานได้หรือไม่ หากต้องการตรวจสอบ ให้ตั้งรหัสผ่านง่ายๆ ที่ไม่เป็นไปตามข้อกำหนดนโยบายรหัสผ่านที่ปลอดภัยที่กำหนดค่าไว้ข้างต้น เราจะตรวจสอบกับผู้ใช้ทดสอบ</p>
    <p>รันคำสั่งนี้เพื่อเพิ่มผู้ใช้:</p>
    <pre>
        <code>$ sudo useradd  testuser</code>
    </pre>
    <p>$ sudo passwd testuser (Policy ตาม <b>How do I enforce a password complexity policy?)</p>
    <pre>
        <code>$ sudo passwd testuser</code>
    </pre>
</div>

<div>
    <h2>Set Password Rules</h2>
    <p>ตั้งกฎรหัสผ่านด้วยโมดูล [pam_pwquality]</p>
    <ol>
        <li>
            ติดตั้งไลบรารีการตรวจสอบคุณภาพรหัสผ่าน
        </li>
        <pre>
            <code>root@dlp:~# apt -y install libpam-pwquality</code>
        </pre>
        <li>
            กำหนดจำนวนวันหมดอายุของรหัสผ่านผู้ใช้จะต้องเปลี่ยนรหัสผ่านภายในไม่กี่วัน การตั้งค่านี้จะมีผลเฉพาะเมื่อสร้างผู้ใช้เท่านั้น ไม่ส่งผลต่อผู้ใช้ที่มีอยู่ หากตั้งค่าเป็นผู้ใช้ที่มีอยู่ ให้รันคำสั่ง [chage -M (days) (user)]
        </li>
        <pre>
            <code>root@dlp:~# vi /etc/login.defs
#set password Expiration days (example below means 60 days)
            PASS_MAX_DAYS 60</code>
        </pre>
        <li>
            กำหนดจำนวนวันขั้นต่ำสำหรับรหัสผ่าน ผู้ใช้ต้องใช้รหัสผ่านอย่างน้อยวันนี้หลังจากเปลี่ยน การตั้งค่านี้จะมีผลเฉพาะเมื่อสร้างผู้ใช้เท่านั้น ไม่ส่งผลต่อผู้ใช้ที่มีอยู่ หากตั้งค่าเป็นผู้ใช้ที่มีอยู่ ให้รันคำสั่ง [chage -m (days) (user)]
        </li>
        <pre>
            <code>root@dlp:~# vi /etc/login.defs
#minimum number of days available (example below means 1 day)
            PASS_MIN_DAYS 1</code>
        </pre>
        <li>
            กำหนดจำนวนวันในการเตือนก่อนหมดอายุ การตั้งค่านี้จะมีผลเฉพาะเมื่อสร้างผู้ใช้เท่านั้น ไม่ส่งผลต่อผู้ใช้ที่มีอยู่ หากตั้งค่าเป็นผู้ใช้ที่มีอยู่ ให้รันคำสั่ง [chage -W (days) (user)]
        </li>
        <pre>
            <code>root@dlp:~# vi /etc/login.defs
#set number of days for warnings (example below means 7 day)
            PASS_WARN_AGE 7</code>
        </pre>
        <li>
            จำกัดการใช้รหัสผ่านที่เคยใช้ในอดีต ผู้ใช้ไม่สามารถตั้งรหัสผ่านเดียวกันภายในรุ่นได้
        </li>
        <pre>
            <code>root@dlp:~# vi /etc/pam.d/common-password
#add [remember=*] (example below means 5 gen)
            password        [success=1 default=ignore]      pam_unix.so obscure use_authtok try_first_pass sha512 remember=5</code>
        </pre>
        <li>
            กำหนดความยาวรหัสผ่านขั้นต่ำ ผู้ใช้ไม่สามารถตั้งค่าความยาวของรหัสผ่านให้น้อยกว่าพารามิเตอร์นี้ได้
        </li>
        <pre>
            <code>root@dlp:~# vi /etc/security/pwquality.conf
#uncomment and set minimum length (example below means 8 char)
            minlen = 8</code>
        </pre>
        <li>
            กำหนดจำนวนคลาสอักขระขั้นต่ำที่จำเป็นสำหรับรหัสผ่านใหม่ (ชนิด ⇒ ตัวพิมพ์ใหญ่ / ตัวพิมพ์เล็ก / ตัวเลข / อื่นๆ)
        </li>
        <pre>
            <code>root@dlp:~# vi /etc/security/pwquality.conf
#uncomment and set parameter (example below means 2 kinds)
        minclass = 2</code>
        </pre>
        <li>
            กำหนดจำนวนอักขระเดียวกันติดต่อกันสูงสุดที่อนุญาตในรหัสผ่านใหม่
        </li>
        <pre>
            <code>root@dlp:~# vi /etc/security/pwquality.conf
#uncomment and set parameter (example below means 2 char)
        maxrepeat = 2</code>
        </pre>
        <li>
            กำหนดจำนวนอักขระต่อเนื่องสูงสุดที่อนุญาตของคลาสเดียวกันในรหัสผ่านใหม่
        </li>
        <pre>
            <code>root@dlp:~# vi /etc/security/pwquality.conf
#uncomment and set parameter (example below means 4 kinds)
        maxclassrepeat = 4</code>
        </pre>
        <li>
            ต้องมีอักขระตัวพิมพ์เล็กอย่างน้อยหนึ่งตัวในรหัสผ่านใหม่
        </li>
        <pre>
            <code>root@dlp:~# vi /etc/security/pwquality.conf
#uncomment and set parameter (example below means 1 char)
        lcredit = -1</code>
        </pre>
        <li>
            ต้องมีอักขระตัวพิมพ์ใหญ่อย่างน้อยหนึ่งตัวในรหัสผ่านใหม่
        </li>
        <pre>
            <code>root@dlp:~# vi /etc/security/pwquality.conf
#uncomment and set parameter (example below means 1 char)
        ucredit = -1</code>
        </pre>
        <li>
            ต้องมีอย่างน้อยหนึ่งหลักในรหัสผ่านใหม่
        </li>
        <pre>
            <code>root@dlp:~# vi /etc/security/pwquality.conf
#uncomment and set parameter (example below means 1 char)
        dcredit = -1</code>
        </pre>
        <li>
            ต้องมีอย่างน้อยหนึ่งหลักในรหัสผ่านใหม่
        </li>
        <pre>
            <code>root@dlp:~# vi /etc/security/pwquality.conf
#uncomment and set parameter (example below means 1 char)
        ocredit = -1</code>
        </pre>
        <li>
            ตั้งค่าความยาวสูงสุดของลำดับอักขระแบบโมโนโทนิกในรหัสผ่านใหม่ (เช่น ⇒ '12345', 'fedcb')
        </li>
        <pre>
            <code>root@dlp:~# vi /etc/security/pwquality.conf
# add to the end (example below means 2 characters are allowed but more than 3 characters are not allowed)
        maxsequence = 2</code>
        </pre>
        <li>
            กำหนดจำนวนอักขระในรหัสผ่านใหม่ที่ต้องไม่ปรากฏอยู่ในรหัสผ่านเก่า
        </li>
        <pre>
            <code>root@dlp:~# vi /etc/security/pwquality.conf
#uncomment and set parameter (example below means 5 char)
        difok = 5</code>
        </pre>
        <li>
            ตรวจสอบว่าคำที่ยาวเกิน 3 ตัวอักษรจากช่อง GECOS ของรายการรหัสผ่านของผู้ใช้มีอยู่ในรหัสผ่านใหม่หรือไม่
        </li>
        <pre>
            <code>root@dlp:~# vi /etc/security/pwquality.conf
#uncomment and change to enabled
        gecoscheck = 1</code>
        </pre>
        <li>
            กำหนดช่องว่างแยกรายการคำที่ต้องไม่มีอยู่ในรหัสผ่าน
        </li>
        <pre>
            <code>root@dlp:~# vi /etc/security/pwquality.conf
# add to the end
        badwords = denywords1 denywords2 denywords3</code>
        </pre>
    </ol>
</div>





