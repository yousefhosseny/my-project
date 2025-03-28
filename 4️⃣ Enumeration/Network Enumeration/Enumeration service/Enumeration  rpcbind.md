### 🔥 **شرح عميق وبسيط جدًا لـ `rpcbind Enumeration` في Network Enumeration**

---

## **🚀 1. يعني إيه `rpcbind`؟**

`rpcbind` هو **خدمة شغالة على لينكس/يونكس**، وظيفتها إنها **تدير خدمات الـ RPC (Remote Procedure Call)**، ودي طريقة تخلي البرامج تتواصل مع بعضها عبر الشبكة.

### ✅ **إزاي بيشتغل؟**

- خدمات الـ RPC (زي `NFS` و `rstatd`) بتشتغل على **بورتات عشوائية**.
- أي جهاز عايز يتصل بخدمة RPC، بيحتاج يسأل `rpcbind` علشان يعرف البورت اللي الخدمة شغالة عليه.

💡 **أمثلة على خدمات بتستخدم `rpcbind`**:

- **`NFS`** → مشاركة الملفات بين الأجهزة.
- **`mountd`** → إدارة مشاركة الملفات على `NFS`.
- **`rusersd` و `rstatd`** → جمع معلومات عن المستخدمين.

🟢 **الخلاصة بالعامية المصرية:**  
`rpcbind` هو **سيرفر بيقولك كل خدمات الـ RPC شغالة على انهي بورت**، وده ممكن يساعدك تعرف حاجات خطيرة زي `NFS` اللي ممكن يكون مفتوح للكل!

---

## **🔍 2. إزاي نكتشف `rpcbind` على السيرفر؟**

#### ✅ **1. باستخدام `Nmap` لمعرفة إذا كان `rpcbind` شغال:**

```bash
nmap -p 111 --script=rpcinfo target.com
```

- البروتوكول ده **بيشتغل على البورت 111 افتراضيًا**.
- `rpcinfo` **هيجيب كل الخدمات اللي بتستخدمه** والمنافذ اللي شغالة عليها.

🟢 **الخلاصة بالعامية المصرية:**  
لو السيرفر فيه `rpcbind`، هنلاقيه شغال على **البورت 111**، ونقدر نعرف كل الخدمات اللي بتستخدمه!

---

## **🔎 3. معرفة الخدمات اللي بتستخدم `rpcbind`**

#### ✅ **2. باستخدام `rpcinfo` لاستكشاف الخدمات على السيرفر**

```bash
rpcinfo -p target.com
```

🔹 الأمر ده هيعرض **كل الخدمات اللي بتستخدم RPC** مع أرقام البورتات بتاعتها.

#### 📌 **مثال على النتيجة:**

```bash
   program vers proto   port  
    100000    2   tcp    111  rpcbind  
    100000    2   udp    111  rpcbind  
    100003    3   udp   2049  nfs  
    100005    1   udp   4045  mountd  
```

- هنا واضح إن **`NFS` شغال على البورت 2049** و**`mountd` شغال على 4045**.
- لو `NFS` مفتوح، ممكن نلاقي **مجلدات مشاركة فيها ملفات حساسة**.

🟢 **الخلاصة بالعامية المصرية:**  
`rpcinfo -p` هيقول لنا إيه الخدمات اللي بتستخدم `rpcbind`، وأي خدمة ممكن تكون مدخل للاختراق!

---

## **📂 4. محاولة استغلال الخدمات المكتشفة**

#### ✅ **3. لو لقيت `NFS` شغال، جرب تستغل مشاركة الملفات!**

```bash
showmount -e target.com
```

🔹 ده هيعرض **كل المجلدات المتاحة على السيرفر**، ولو في حاجة مفتوحة، ممكن تعمل **mount** وتاخد الملفات.

#### ✅ **4. لو لقيت `mountd` شغال، جرب تفحصه!**

```bash
rpcclient -U "" target.com
```

🔹 ممكن تاخد **معلومات عن المستخدمين والمجلدات المشتركة**، وده ممكن يوصلك لحاجات حساسة.

🟢 **الخلاصة بالعامية المصرية:**  
لو لقينا `NFS` أو `mountd`، غالبًا هنلاقي **ملفات مشتركة أو بيانات مفيدة** ممكن تساعدنا في الاختراق!

---

## **💀 5. ازاي نقفل الثغرة؟**

✅ **1. اقفل `rpcbind` لو مش محتاجه**

```bash
systemctl disable rpcbind
systemctl stop rpcbind
```

✅ **2. استخدم `firewall` لمنع الاتصال بـ `rpcbind` من أجهزة غير موثوقة**

```bash
iptables -A INPUT -p tcp --dport 111 -j DROP
iptables -A INPUT -p udp --dport 111 -j DROP
```

✅ **3. استخدم `hosts.allow` و `hosts.deny` لتقييد الوصول**

```bash
echo "rpcbind: 192.168.1.0/24" >> /etc/hosts.allow
echo "rpcbind: ALL" >> /etc/hosts.deny
```

🟢 **الخلاصة بالعامية المصرية:**  
لو السيرفر مش محتاج `rpcbind`، **اقفله فورًا**! ولو لازم يشتغل، **حدد مين يقدر يوصل له علشان تحمي نفسك**.

---

## **🚀 خطوات عملية لاختبار اختراق الويب (Web Pentesting)**

### ✅ **1. جمع المعلومات (Reconnaissance)**

- استخدام `whois` لمعرفة معلومات عن الدومين.
- استخدام `nmap` لمسح البورتات والخدمات:
    
    ```bash
    nmap -sC -sV -oN scan.txt target.com
    ```
    
- البحث عن الدلائل المخفية:
    
    ```bash
    gobuster dir -u http://target.com -w /usr/share/wordlists/dirb/common.txt
    ```
    

### ✅ **2. تحليل الثغرات (Vulnerability Scanning)**

- استخدام `Nikto` لفحص الموقع:
    
    ```bash
    nikto -h http://target.com
    ```
    
- البحث عن SQL Injection باستخدام `sqlmap`:
    
    ```bash
    sqlmap -u "http://target.com/index.php?id=1" --dbs
    ```
    

### ✅ **3. استغلال الثغرات (Exploitation)**

- تجربة **XSS** في الحقول النصية أو الـ URLs.
- استغلال **File Upload** لو الموقع بيسمح برفع ملفات بدون تحقق.

### ✅ **4. استخراج البيانات (Data Exfiltration)**

- لو دخلت على قاعدة البيانات، استخرج الحسابات والمعلومات الحساسة.
- تحليل الكوكيز والجلسات لجمع أي بيانات مفيدة.

### ✅ **5. رفع الامتيازات (Privilege Escalation)**

- البحث عن **ملفات التكوين المهمة** زي `config.php` اللي ممكن تحتوي على **كلمات مرور**.
- تحليل **الأذونات** والبحث عن **ملفات قابلة للكتابة**.

### ✅ **6. الاحتفاظ بالوصول (Persistence)**

- إنشاء **Web Shell** لو تمكنت من رفع ملفات PHP ضارة.
- استخدام **SSH Keys** لو حصلت على بيانات دخول.

---

## **🔥 الخلاصة النهائية:**

1️⃣ **`rpcbind` هو سيرفر بيساعد خدمات الـ RPC تشتغل على الشبكة.**  
2️⃣ **بيشتغل على البورت 111، ونقدر نكشفه باستخدام `nmap`.**  
3️⃣ **`rpcinfo -p` بيعرض كل الخدمات اللي بتستخدمه وأرقام البورتات بتاعتها.**  
4️⃣ **لو لقيت `NFS` أو `mountd`، جرب تستغل مشاركة الملفات أو تاخد معلومات المستخدمين.**  
5️⃣ **لحماية السيرفر، اقفل `rpcbind` لو مش محتاجه، أو استخدم `firewall` لتقييد الوصول.**

🔥 **كده معاك شرح شامل لـ `rpcbind Enumeration` وأفضل الطرق لاستغلاله أو حمايته!** 💀💻