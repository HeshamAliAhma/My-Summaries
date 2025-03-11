
طبعا نبدأ ببسم الله الرحمن الرحيم 

أفضل حاجه ان الواحد يعملها قبل ما يبدأ يهانت هي انه يقرا صفحة قران ويصلي ركعتين بنية ان ربنا يوفقه ويستغفر ويصلي عالنبي **عشر مرات**
----

بعد ما نختار تارجت أي كان بقي اكسترنال او علي منصة من المنصات [hackerone.com, intigriti.com, bugcrwod.com, bugbounty.sa] اوي اي منصة عمتا نبدأ بقي نمشي علي الخطوات اللي هقولها دي

> بداية كدة مينفعش تقعد علي التارجت نص ساعة وتسيبه وتقول انك مفهمتش حاجه منه **أقل حاجه يومين** تجرب تخش علي كل ال **subdomains** وتجرب كل حاجه في الموقع ينفع تعمل عليها **click** وتشوف كل الفانكشنز وتجربها وتشوف ال **Requests and Responses** وتاخد نوتس بأي حاجه لقيتها ولو تعرف تعمل تيست بشكل بسيط اوي علي ثغرة زي (xss, csrf, idor,busnisess logic, sqli,cors) وهكذا هيبقي كويس اوي واهو تشوف ال **response** هيتغير فيه حاجه ولا لا

بعد اليومين دول هنبدأ نقرر هل نكمل في التارجت ولا لا :
1) لو لا يبقي تشوف تارجت تاني وتمشي علي نفس الخطوات
2) لو اه يبقي يلا نخش علي الخطوات اللي بعد كدة

----
1) جمعلي بقي كدة كل الصب دومينز وال urls اللي ينفع تجمعهم :
	1) ينفع تستخدم المواقع والادوات دي للصب دومينز 
2) https://securitytrails.com/
3) https://urlscan.io/
4) https://otx.alienvault.com/
5) https://crt.sh/
6) Tools : [ assetfinder - amass - subfinder - anew]
7) https://subdomainfinder.c99.nl/
8) https://shrewdeye.app/
9) [https://archive.org](https://archive.org/)
10) subfinder -d mars.com -all --recursive -o subs.txt
11) echo mars.com | assetfinder --subs-only >> subfile
after collecting all subdomains in **subs.txt** then let's remove duplicate 
1) cat subfile | anew >> subnew
2) rm subfile
بعد كدة هتكون جمعت صب دومينز ياما ف هنستخدم أداة **httpx** عشان نطلع الصبدومينز السليمة بس
cat subs | httpx -o httpx.txt
cat subs | httpx -mc 404 -o httpx404   than try wayback or fuzz
cat subs | httpx -mc 403 -o httpx403   than try bypass
cat subs | httpx -mc 200 -o httpx200  than try join
**كدة انت هتبقي واثق انك جمعت عدد لا بأس به من الصب دومينز وهيجي بقي دور أنك تجمع ال URLS** 
ودول أدوات تستخدمهم في الموضوع ده :
1) Tools : [katana - waybackurls - gospider - anew - oru]
2) katana -list httpx.txt -o katana.txt
3) cat httpx.txt | waybackurls >> wayback.txt
4) gospider -S httpx.txt | sed -n 's/.*\(https:\/\/[^ ]*\)]*.*/\1/p' >> gospider.txt
5) cat katana.txt wayback.txt gospider.txt >> urls.txt
6) cat urls.txt | anew >> allurls.txt
7) rm urls.txt
بعد كدة بقي نبقي نستخدم ال **grep** عشان نطلع الملفات المهمة واللي هما يعتبر دول [php - jsp - jspx - aspx - asp - js] وهنعمل **grep** لل **=** وهنحطهم في فايل اسمه **injectio** 
---
كدة احنا معانا **subdomains and urls** وكمان عارفين الفانكشنز الموجودة وفهمنها بشكل بسيط يجي بقي دور ثغرتين خفاف نبدأ ندور عليهم [Broken Link Hijaking and Subdomain takeover]
 1) subzy run --targets subs.txt --hide_fails --vuln | grep -v -E "Akamai|xyz|available|\-"
	 1) if you found any vulnerability then search on how to takeover subdomain
 2) grep -oP '(?<=https?:\/\/(?:www\.)?(?:instagram\.com|facebook\.com|x\.com|twitter\.com|tiktok\.com|linkedin\.com)\/)[a-zA-Z0-9_.-]+' urls.txt
	 1) ونقدر كمان نعمل ملف بلغة بايثون
1) 
```python
import re
import sys

# التحقق من تمرير اسم الملف عند التشغيل
if len(sys.argv) < 2:
    print("❌ استخدم: python extract_urls.py <اسم_الملف>")
    sys.exit(1)

# الحصول على اسم الملف من سطر الأوامر
file_name = sys.argv[1]

# تعريف الـ Regex لاستخراج الروابط الكاملة
pattern = re.compile(r'(https?:\/\/(?:www\.)?(?:instagram\.com|facebook\.com|x\.com|twitter\.com|tiktok\.com|linkedin\.com)\/[a-zA-Z0-9_.-]+)')

try:
    # فتح الملف وقراءته سطرًا بسطر
    with open(file_name, 'r', encoding='utf-8') as file:
        for line in file:
            match = pattern.search(line.strip())
            if match:
                print(match.group(1))  # طباعة الرابط الكامل
except FileNotFoundError:
    print(f"❌ خطأ: الملف '{file_name}' غير موجود.")
    sys.exit(1)
```

كل فترة هنعمل نفس الكلام - كل اسبوعين مثلا

---

نبدأ بقي في الشغل الشعبي بتاعنا 
1) نشوف جميع ال **checklists** اللي احنا حطيناها هنا [[Chicklists]]
2) نستخدم أداة drip and dirsearch عشان نعمل content discovery
3) صح - لما نجمع ال js urls بنعمل تحليل لل js code
	1) 1- use mantra to get api , passwords , keys ... 
		1) cat js.txt | mantra | tee -a mantra.txt
	2) use nuclei to search for secrets
		1) nuclei -l js.txt -t nuclei-templates/http/exposures/ -o nucleijs.txt


الثغرات اللذيذة هي اللي ممكن تدور عليها بسرعة :
1) xss 
2) csrf
3) access control
4) information disc
5) race codition
6) priv esc
7) rate limit
8) broken link hijacking
9) subdomain takeover