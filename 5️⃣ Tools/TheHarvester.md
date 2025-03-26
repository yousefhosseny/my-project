### **📌 ما هي أداة TheHarvester؟**

أداة **TheHarvester** تُستخدم لجمع المعلومات (OSINT) حول الشركات، النطاقات، وعناوين البريد الإلكتروني، وبيانات DNS وعناوين IP من مصادر مفتوحة مثل:  
✅ **Google**  
✅ **Bing**  
✅ **LinkedIn**  
✅ **Shodan**  
✅ **Hunter.io**  
✅ **DuckDuckGo**

تساعد هذه الأداة مختبري الاختراق في **جمع المعلومات الأولية عن الأهداف** أثناء عمليات **الاستطلاع (Reconnaissance)**.

---

## ✅ **1. تثبيت TheHarvester**

### **🔹 على Kali Linux (مُثبتة مسبقًا)**

```bash
sudo apt update && sudo apt install theharvester
```

### **🔹 على أي نظام آخر باستخدام Python**

```bash
git clone https://github.com/laramies/theHarvester.git
cd theHarvester
pip install -r requirements.txt
python3 theHarvester.py -h
```

---
## علشان اشغلها 

```bash
theHarvester
```
## ✅ **2. أوامر TheHarvester الأساسية**

### 📍 **2.1 جمع الإيميلات والمعلومات من Google**

```bash
theHarvester -d example.com -b google
```

🔹 **يجمع الإيميلات والدومينات وعناوين IP الخاصة بالمجال (example.com) من Google.**

---

### 📍 **2.2 البحث عن أسماء نطاقات فرعية (Subdomains)**

```bash
theHarvester -d example.com -b bing
```

🔹 **يجمع النطاقات الفرعية من محرك بحث Bing.**

---

### 📍 **2.3 البحث عن البريد الإلكتروني من LinkedIn**

```bash
theHarvester -d example.com -b linkedin
```

🔹 **يعثر على البريد الإلكتروني لموظفي الشركة المستهدفين من LinkedIn (يتطلب أحيانًا API).**

---

### 📍 **2.4 البحث عن عناوين IP المرتبطة بالمجال**

```bash
theHarvester -d example.com -b shodan
```

🔹 **يُظهر عناوين IP المرتبطة بـ example.com من قاعدة بيانات Shodan.**

---

### 📍 **2.5 البحث عن بيانات من جميع المصادر دفعة واحدة**

```bash
theHarvester -d example.com -b all
```

🔹 **يجمع المعلومات من كل المصادر المتاحة دفعة واحدة.**

---

## ✅ **3. أوامر متقدمة في TheHarvester**

### 📍 **3.1 حفظ النتائج في ملف نصي**

```bash
theHarvester -d example.com -b google -f results.txt
```

🔹 **يحفظ النتائج في ملف نصي باسم `results.txt` لقراءتها لاحقًا.**

---

### 📍 **3.2 استخدام عدد صفحات محدد من Google**

```bash
theHarvester -d example.com -b google -l 100
```

🔹 **يجلب بيانات من أول 100 نتيجة بحث فقط، مما يزيد من دقة البحث.**

---

### 📍 **3.3 البحث عن كلمات مفتاحية محددة داخل نطاق معين**

```bash
theHarvester -d example.com -b google -k "admin"
```

🔹 **يبحث عن الإيميلات أو البيانات التي تحتوي على كلمة "admin".**

---

### 📍 **3.4 البحث في Hunter.io لجلب الإيميلات**

```bash
theHarvester -d example.com -b hunter
```

🔹 **يجمع الإيميلات من Hunter.io (يتطلب API Key).**

---

### 📍 **3.5 البحث عن أسماء الشركات والموظفين**

```bash
theHarvester -d example.com -b linkedin -l 500
```

🔹 **يعثر على أسماء الموظفين أو الشركات المرتبطة بالمجال (يحتاج API).**

---

## 🚀 **🔥 الخلاصة النهائية**

✅ **TheHarvester** أداة قوية في **جمع المعلومات (OSINT)** عن الأهداف.  
✅ تُستخدم في **البحث عن البريد الإلكتروني، النطاقات الفرعية، عناوين IP، وبيانات الشركات.**  
✅ تعمل مع مصادر مثل **Google, Bing, LinkedIn, Shodan, Hunter.io** لجلب أكبر كم ممكن من المعلومات.  
✅ يمكن حفظ النتائج في ملفات وإجراء عمليات بحث متقدمة باستخدام API.

🚨 **⚠️ تنبيه قانوني:** لا تستخدم هذه الأوامر لجمع بيانات غير مصرح بها، لأن بعض المعلومات قد تكون محمية بقوانين الخصوصية.