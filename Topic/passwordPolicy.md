<div>
    <h1>Password Policy</h1>
    <blockquote>
        <p><b>How do I enforce a password complexity policy?</b></p>
        <li>at least one upper case (อย่างน้อยหนึ่งตัวพิมพ์ใหญ่)</li>
        <li>at least one lower case (อย่างน้อยหนึ่งตัวพิมพ์เล็ก)</li>
        <li>at least one digit (อย่างน้อยหนึ่งหลัก)</li>
        <li>at least one special character (อักขระพิเศษอย่างน้อยหนึ่งตัว)</li>
    </blockquote>
<h3>ทดสอบนโยบายรหัสผ่านที่ปลอดภัย</h3>
<p>หลังจากกำหนดนโยบายรหัสผ่านที่ปลอดภัยแล้ว ควรตรวจสอบว่าใช้งานได้หรือไม่ หากต้องการตรวจสอบ ให้ตั้งรหัสผ่านง่ายๆ ที่ไม่เป็นไปตามข้อกำหนดนโยบายรหัสผ่านที่ปลอดภัยที่กำหนดค่าไว้ข้างต้น เราจะตรวจสอบกับผู้ใช้ทดสอบ</p>
<p>รันคำสั่งนี้เพื่อเพิ่มผู้ใช้:</p>
<pre>
<code>$ sudo useradd  testuser</code>
</pre>
<p>$ sudo passwd testuser (Policy ตาม <b>How do I enforce a password complexity policy?)</b></p>
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
    <code>
    root@dlp:~# apt -y install libpam-pwquality
    </code>
</pre>
<li>
    กำหนดจำนวนวันหมดอายุของรหัสผ่านผู้ใช้จะต้องเปลี่ยนรหัสผ่านภายในไม่กี่วัน การตั้งค่านี้จะมีผลเฉพาะเมื่อสร้างผู้ใช้เท่านั้น ไม่ส่งผลต่อผู้ใช้ที่มีอยู่ หากตั้งค่าเป็นผู้ใช้ที่มีอยู่ ให้รันคำสั่ง [chage -M (days) (user)]
</li>
<pre>
    <code>
    root@dlp:~# vi /etc/login.defs
    PASS_MAX_DAYS 60
        # set password Expiration days (example below means 60 days)</code>
</pre>
<li>
    กำหนดจำนวนวันขั้นต่ำสำหรับรหัสผ่าน ผู้ใช้ต้องใช้รหัสผ่านอย่างน้อยวันนี้หลังจากเปลี่ยน การตั้งค่านี้จะมีผลเฉพาะเมื่อสร้างผู้ใช้เท่านั้น ไม่ส่งผลต่อผู้ใช้ที่มีอยู่ หากตั้งค่าเป็นผู้ใช้ที่มีอยู่ ให้รันคำสั่ง [chage -m (days) (user)]
</li>
<pre>
    <code>
    root@dlp:~# vi /etc/login.defs
    PASS_MIN_DAYS 1
        # minimum number of days available (example below means 1 day)</code>
</pre>
<li>
    กำหนดจำนวนวันในการเตือนก่อนหมดอายุ การตั้งค่านี้จะมีผลเฉพาะเมื่อสร้างผู้ใช้เท่านั้น ไม่ส่งผลต่อผู้ใช้ที่มีอยู่ หากตั้งค่าเป็นผู้ใช้ที่มีอยู่ ให้รันคำสั่ง [chage -W (days) (user)]
</li>
<pre>
    <code>
    root@dlp:~# vi /etc/login.defs
    PASS_WARN_AGE 7
        # set number of days for warnings (example below means 7 day)</code>
</pre>
<li>
    จำกัดการใช้รหัสผ่านที่เคยใช้ในอดีต ผู้ใช้ไม่สามารถตั้งรหัสผ่านเดียวกันภายในรุ่นได้
</li>
<pre>
    <code>
    root@dlp:~# vi /etc/pam.d/common-password
    password        [success=1 default=ignore]      pam_unix.so obscure use_authtok try_first_pass sha512 remember=5
        # add [remember=*] (example below means 5 gen)</code>
</pre>
<li>
    กำหนดความยาวรหัสผ่านขั้นต่ำ ผู้ใช้ไม่สามารถตั้งค่าความยาวของรหัสผ่านให้น้อยกว่าพารามิเตอร์นี้ได้
</li>
<pre>
    <code>
    root@dlp:~# vi /etc/security/pwquality.conf
    minlen = 8
        # uncomment and set minimum length (example below means 8 char)</code>
</pre>
<li>
    กำหนดจำนวนคลาสอักขระขั้นต่ำที่จำเป็นสำหรับรหัสผ่านใหม่ (ชนิด ⇒ ตัวพิมพ์ใหญ่ / ตัวพิมพ์เล็ก / ตัวเลข / อื่นๆ)
</li>
<pre>
    <code>
    root@dlp:~# vi /etc/security/pwquality.conf
    minclass = 2
        # uncomment and set parameter (example below means 2 kinds)</code>
</pre>
<li>
    กำหนดจำนวนอักขระเดียวกันติดต่อกันสูงสุดที่อนุญาตในรหัสผ่านใหม่
</li>
<pre>
    <code>
    root@dlp:~# vi /etc/security/pwquality.conf
    maxrepeat = 2
        # uncomment and set parameter (example below means 2 char)</code>
</pre>
<li>
    กำหนดจำนวนอักขระต่อเนื่องสูงสุดที่อนุญาตของคลาสเดียวกันในรหัสผ่านใหม่
</li>
<pre>
    <code>
    root@dlp:~# vi /etc/security/pwquality.conf
    maxclassrepeat = 4
        # uncomment and set parameter (example below means 4 kinds)</code>
</pre>
<li>
    ต้องมีอักขระตัวพิมพ์เล็กอย่างน้อยหนึ่งตัวในรหัสผ่านใหม่
</li>
<pre>
    <code>
    root@dlp:~# vi /etc/security/pwquality.conf
    lcredit = -1
        # uncomment and set parameter (example below means 1 char)
    </code>
</pre>
<li>
    ต้องมีอักขระตัวพิมพ์ใหญ่อย่างน้อยหนึ่งตัวในรหัสผ่านใหม่
</li>
<pre>
    <code>
    root@dlp:~# vi /etc/security/pwquality.conf
    ucredit = -1
        # uncomment and set parameter (example below means 1 char)</code>
</pre>
<li>
    ต้องมีอย่างน้อยหนึ่งหลักในรหัสผ่านใหม่
</li>
<pre>
    <code>
    root@dlp:~# vi /etc/security/pwquality.conf
    dcredit = -1
        # uncomment and set parameter (example below means 1 char)
    </code>
</pre>
<li>
    ต้องมีอย่างน้อยหนึ่งหลักในรหัสผ่านใหม่
</li>
<pre>
    <code>
    root@dlp:~# vi /etc/security/pwquality.conf
    ocredit = -1
        # uncomment and set parameter (example below means 1 char)</code>
</pre>
<li>
    ตั้งค่าความยาวสูงสุดของลำดับอักขระแบบโมโนโทนิกในรหัสผ่านใหม่ (เช่น ⇒ '12345', 'fedcb')
</li>
<pre>
    <code>
    root@dlp:~# vi /etc/security/pwquality.conf
    maxsequence = 2
        # add to the end (example below means 2 characters are allowed but more than 3 characters are not allowed)</code>
</pre>
<li>
    กำหนดจำนวนอักขระในรหัสผ่านใหม่ที่ต้องไม่ปรากฏอยู่ในรหัสผ่านเก่า
</li>
<pre>
    <code>
    root@dlp:~# vi /etc/security/pwquality.conf
    difok = 5
        # uncomment and set parameter (example below means 5 char)</code>
</pre>
<li>
    ตรวจสอบว่าคำที่ยาวเกิน 3 ตัวอักษรจากช่อง GECOS ของรายการรหัสผ่านของผู้ใช้มีอยู่ในรหัสผ่านใหม่หรือไม่
</li>

    root@dlp:~# vi /etc/security/pwquality.conf
    gecoscheck = 1
        # uncomment and change to enabled</code>

<li>
    กำหนดช่องว่างแยกรายการคำที่ต้องไม่มีอยู่ในรหัสผ่าน
</li>

    root@dlp:~# vi /etc/security/pwquality.conf
    badwords = denywords1 denywords2 denywords3
        # add to the end
</ol>
</div>


