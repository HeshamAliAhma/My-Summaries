أحمد ناجه بعد ما بياخد السكوب بتاعه وليكن 
`*.example.com`
بيحطها في `Google Dork = site` وبيستخدم مثلا ال `filter` اللي اسمه `tools` عشان يجيب اخر subdomain تم طرحه علي قد الامكان وبعد ما يجع شوية صب دومينز من ال `google dorking` بيخش مثلا علي دومين عشوائي وياخد ال `copyrite` ويستخدمها علي `google dork = intitle` عشان يجيب المواقع اللي اتذكر فيها ال `copyrite` ده عشان لو فيه موقع مختلف يحطه في الاسكوب وبعد كدة بيخش علي السورس كود ويجمع شوية ملفات بنفسه --- طبعا بعمل لكل حاجه فايل يعني فايل لل `copyrights and links and subdomains` وهكذا وتشوف ملف ال `robots.txt and sitemap.xml` وتشوف اضافة  `shodan and wapplyzer` 

ممكن تدور كمان في ملفات ال `css and javascript` وملف ال `robots.txt` وتفتح ال `debtools` وتدور في كل حاجه فيهم


بندور في جيتهاب علي أي حاجه تخص التارجت --- حرفيا أي حاجه

أعمل فايل اسمه `functions` وأنا بدور في التارجت وبمشي عالخطوات اللي فوق دي واي فانكشن الاقيها بكتبها في الفايل عشان ابقي ارجعلها

نفتح ال `Burp` ونظبط الاعدادت بتاعتنا وبعد كدة نلف في التارجت كله ونحاول نفتح اكبر قدر من الصبدومينز اللي احنا جمعناها ونلف بأكبر قدر في التارجت بتاعنا عشان البيرب يحفظ ال `requests and responses` وفي التاب بتاعت `sitemap` هتلاقي كل حاجه محفوظة

اي هي الاعدادات اللي عايزين نعملها في ال `Burp` :
1) طبعا نظبط السكوب بتاعنا
2) ونروح عند `response modification rules` ونعلم علي اول تلاتة
3) نروح عند `match and replace` ونعدل في كام حاجه


لازم نفهم الشركة ونشوف المعلومات العامة اللي محطوطة عالنت عنها زي:
1) دور علي ويكيبديا
2) اسأل شات جي بي تي
3) افتح الويبسات وشوف صفحة ال `about us` 

شوف الشركة اللي بتعمل عليها هانت لازم تعرف **بلد المنشأ بتاعتها**

بعد كدة نبدأ نستخدم ادوات البحث بقي ف هو هنا استخدم الاداوت دي:

#### Subdomain Enumeration:

```c
Tools:
1) subfinder -d doit.com -all --recursive -t 200 -silent -o subfinder.txt
2) echo mars.com | assetfinder --subs-only >> subfile
3) subdominator -d doit.com -o subs.txt
4) sublist3r -d target.com -t 50 -o sublist3r.txt
5) curl -s https://crt.sh/\?q\=\doit.com\&output\=json | jq -r '.[].name_value' | sed 's/\*\.//g' | sort -u | tee -a subs_domain.txt
6) curl -s "https://urlscan.io/api/v1/search/?q=domain:domain.com&size=10000" | jq -r '.results[]?.page?.domain' | grep -E "^[a-zA-Z0-9.-]+\.domain\.com$" | sort -u | tee urlscan_subs.txt
7) curl -s "http://web.archive.org/cdx/search/cdx?url=*.domain.com/*&output=json&collapse=urlkey" | jq -r '.[1:][] | .[2]' | grep -Eo '([a-zA-Z0-9._-]+\.)?domain\.com' | sort -u | tee webarcive_subs.txt
8) knockpy -d doit.com --recon --bruteforce
9) amass enum -active -d doit.com -o subs.txt
10) chaos -d doit.com -o chaos_subs.txt
11) sslyze --ocsp doit.com
12) amass enum -passive -d target.com -o amass.txt
Websites:
1) https://securitytrails.com/
2) https://urlscan.io/
3) https://shrewdeye.app/
4) https://subdomainfinder.c99.nl/
------------
ممكن نستخدم موقع `crt.sh` عشان نجيب ال `subdomains` المسجلة بالطريقة دي
*) curl -s "https://crt.sh/?q=%25.doit.com&output=json" | jq -r '.[] | select(.name_value | startswith("*.") ) | .name_value' | sort -u
وعشان نجيب الشهادات المهنتية هنستخدم نفس الموقع ولكن بأمر تاني
*) curl -s "https://crt.sh/?q=%25.doit.com&output=json" | jq -r '.[] | select(.not_after < "'$(date -u +"%Y-%m-%d")'") | .name_value, .not_after'
------------
**) cat * | anew >> allSubdomains.txt
```

### httpx
```c
to see the working sites 
cat subs | httpx -o httpx.txt
cat subs | httpx -mc 404 -o httpx404   than try wayback or fuzz
cat subs | httpx -mc 403 -o httpx403   than try bypass
cat subs | httpx -mc 200 -o httpx200  than try join
```
### Port Scanning:

```c
Tools:
1) masscan -p1-65535 --rate=2000 -iL all_subs.txt -oL open_ports.txt
2) nmap -iL allsubs.txt -o nmap.txt
*) after finding open port as 22,21,25,111,139,445,etc...
3) nmap <ip> -sV -p22,21,55,111
	then search for exploit in google
*) to see the scripts of nmap
	# cd /usr/share/nmap/scripts
	# ls 
*) to grep only scripts related to ssh or anything else
	# ls | grep ssh
*) to use all scripts related to ssh
	# nmap 192.168.1.1 --scripts=ssh*
*) to use specific script for exmaple ssh-brute.nse
	# nmap 192.168.1.1 --script=ssh-brute.nse
*) to use all vulnerable scripts to check for vulnerabilities 
	# nmap 192.168.1.1 --script=vuln
	# nmap 192.168.1.1 --script=exploit
*) to bypass the firewall 
	# nmap -sS -Pn -n 192.168.1.1 
*) to use fragment mode to bypass the firewall 
	# nmap -f 192.168.1.1
```

### Gather Urls:

```c
Tools:
1) katana -u subdomains_alive.txt -d 5 -ps -pss waybackarchive,commoncrawl,alienvault -kf -jc -fx -ef woff,css,png,svg,jpg,woff2,jpeg,gif,svg -o allurls.txt
2) See The Source Code
3) cat urls.txt | grep -i
grep -i "\.asp" > asp.txt
grep -i "\.php" > php.txt
grep -i "\.jsp" > jsp.txt
grep -i "\.jspx" > jspx.txt
grep -i "\.aspx" > aspx.txt
grep -i "\.js" > js.txt
grep -i  "=" > inje

> and get the "jsfile" and anylze thim by three tools:
> 1) cat jsfiles.txt | manatra
> 2) cat jsfiles.txt | jsceret
```



### Subdomain Takeover:

```c
Tools:
1) subzy run --targets subnew --hide_fails --vuln  | grep -v -E "Akamai|xyz|available|\-"
	if you found any vulnerability then search on how to takeover subdomain
2) subjack -w all_subs.txt -a -o takeovers.txt && curl -X POST -d "Takeover found!" slack_webhook_url
```


### Content Discovery









---

### **أدوات الاستطلاع اليدوي:**

[FOFA, ZoomEye, Censys, Onyphe, Netlas, IntelX, BinaryEdge, SOCRadar, Grep.app, LeakIX]


----
ممكن نستخدم أداة `ffuf` عشان بالطريقة
```c
ffuf -w custom_wordlist.txt -u https://FUZZ.doers-map.internal.doit.com -mc 200,301 -o found_subs.txt
```

لتجاوز حماية **Cloudflare أو Akamai**، استخدم **CloudBrute**:
```c
CloudBrute -d doit.com -k doit -m storage -t 80 -T 10 -w "./data/storage_small.txt"
```


لو هو بيستخدم GraphQL فهنستخدم الأداة دي:
```c
inql -t https://doit.com/graphql
```

وبعدين فيه `Google Dork` ممكن نستعمله عشان نجيب ال `API` المخفية
```c
site:*.doit.com -inurl:(login signup) "api_key" filetype:json | yaml
```

---
من اهم ال `API Endpoints` اللي الواحد ممكن يدور عليها اللي بتبقي عاملة زي كدة
```http
GET /api/v1/user/123
```
عشان لو خليتها كدة 
```http
GET /api/v1/user/124
```
ممكن يجيبلك معلومات شخص تاني ويبقي دي ثغرة `IDOR` و**الخطورة بتاعتها عالية كمان**

---
وعشان نعرف نوع ال `waf` اللي الموقع بيتسخدموا عندنا طريقتين:
```c
wafw00f https://example.com
```

```c
whatwaf -u https://example.com
```

ومن المهم ايضا معرفة نوع واصدار السيرفر فعندنا سكربت بلغة بايثون ممكن نستخدمو في الموضوع ده:
```python
import requests

url = "https://doit.com"
r = requests.head(url, timeout=5)

for key, value in r.headers.items():
	print(f"{key}: {value}")
```

من أشهر الادوات اللي بتدور علي الثغرات هي `nuclei` ف ممكن بعد ما نجمع جميع ال `subdomains` نستخدم الأمر ده:
```c
nuclei -l subs.txt -t /home/zeusvuln/custom-nuclei-templates
```

> بعد ما نعرف نوع واصدار السيرفر - المفروض بندور علي `cve` للبحث عن ثغرات معروفة ممكن نستغلها

لو دورنا علي جيتهاب ولقينا الريبو بتاعت الشركة ف في أداة بتدور في الموضوع ده

```c
trufflehog git https://github.com/doit/repo --since_commit HEAD~100
```

عشان ندور علي بيانات متسربة ف في أداة اسمها `pastesearch` جميلة

```c
pastesearch -d doit.com -o leaks.txt
```

وممكن تحليل البيانات باستخدام الذكاء الاصطناعي

```c
spiderfoot -t doit.com -m all -o json > osint.json
```

---

من الطرق للبحث عن ال `subdomains` المخفية ممكن نستخدم الأمر ده

```c
cat subdomains.txt | dnsgen - | massdns -r resolvers.txt -o S -w dns_results.txt
```


وكمان نقدر نلاقي خوادت مخفية وخدمات داخلية غير مؤمنة بأستخدام الأمر ده
```c
dig @ns1.doit.com doit.com AXFR
```

وكمان عندنا الاداة دي `dnsrecon` عشان لو فيه اي `subdomain` بيشير الي خدمة سحابية ولا حاجه
```c
dnsrecon -d doit.com -t axfr
```

___
نقدر نحلل ال `http header` عن طريق الامر ده
```c
curl -I https://doit.com
```
وال `headers` اللي احنا نبص عليها دول : 
1) X-Powered-By: يكشف عن لغة البرمجة او الفريمورك
2) Server-Tokens: يكشف عن اصدار السيرفر
3) X-Debug-Token: ممكن يكون بيحتوي علي معولمات تصحيحية حساسة
ممكن نستخدم طريقة `TRACE` عشان تعيد البيانات المدخلة ليك بالطريقة دي
```c
curl -X TRACE -i https://doit.com
```
إذا أرجع الخادم **الرؤوس الداخلية (مثل `Authorization` أو `Cookie`)**، فهذا يكشف **معلومات حساسة يمكن استغلالها**!

ممكن افحص تمكين `WebDAV` وده امتداد ل `HTTP` يسمح برفع الملفات والتعديل عليها مباشرة على الخادم بالطريقة دي
```c
davtest -url https://doit.com
```
لو الناتج طلع ان فيه ملفات ينفع نكتبها علي السيرفر ف ده يشير الي ثغرة امنية

----

ممكن اعمل `content dicovery` بالطريقة دي عشان ابحث علي ملفات مخفية
```c
wfuzz -u "https://doit.com/FUZZ" -w wordlist.txt --filter "content =~ /.*\.(key|sql|bak|env|db|zip)$/"
```

في حاجه اسمها `HTTP Parameter Pollution (HPP)` وده عبارة عن اضافة نفس ال `parameter` اكثر من مرة عشان ممكن يكشف عن معلومات تانية هبقي محتاجها ف في اداة ممكن تساعدنا في الموضوع ده وهي `parameth` بالطريقة دي
```c
parameth -u "https://doit.com/page?id=1" -m GET
```

