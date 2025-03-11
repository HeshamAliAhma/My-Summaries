
ده لينك الرايتب [writeup](# **Are You Missing Subdomains? The Resolver Trick You Need to Know!**)
وبيتكلم عن طريقة بتسمح تجمعلك اكبر قدر من الصب دومينز وهي استخدام `SLD DNS` 

أستخدم اداة `dnscan` بالطريقة دي

1) **البحث باستخدام خادم مزود الخدمة (ISP DNS Resolver)**
```Linux
python dnscan.py -d exmaple.com -w wordlist.txt
```

1) **البحث باستخدام خادم DNS الرسمي (Authoritative DNS)**
```Linux
python dnscan.py -d exmaple.com -R 193.222.76.54,138.190.34.204,138.190.34.196 -w wordlist.txt
```

> وطبعا كلما زاد عدد الصب دومينز في الووردليست كلما كان احتمال ان الاداة تطلع أكتر


## Writeups:
1) [writeup](# **Are You Missing Subdomains? The Resolver Trick You Need to Know!**)
2) [zuz](https://medium.com/@mahmodziad40/how-i-found-5-reflected-xss-in-a-public-program-44b168ae6526)




## Ideas

### to subdomains enum
```
1) python dnscan.py -d exmaple.com -w wordlist.txt
	1) البحث باستخدام خادم مزود الخدمة (ISP DNS Resolver)
2) python dnscan.py -d exmaple.com -R 193.222.76.54,138.190.34.204,138.190.34.196 -w wordlist.txt
	1) البحث باستخدام خادم DNS الرسمي (Authoritative DNS)
3) bbot -d example.com -o subbdomain.txt
```

### Filtering Out the Subdomains
```
1) cat subdomains.txt | httpx -silent -o livesubs.txt
2)
```
### Crawling Endpoints
```
katana -l livesubs.txt -o endpoints.txt
```

### Extracting Urls
```
cat livesubs.txt | waybackurls | tee -a urls.txt
# i prefer to use tee command to see the output of the urls on the screen
```










#### best wordlist for subdomain Enum:

1. **dns-Jhaddix.txt**  
    📌 **المسار:** `SecLists/Discovery/DNS/dns-Jhaddix.txt`  
    ✅ واحدة من أفضل القوائم، تحتوي على نطاقات فرعية شائعة، ومناسبة للبحث السريع.  
    🛠️ تم تحديثها لتشمل نطاقات مستخدمة حقيقيًا بناءً على بيانات تجريبية.
    
2. **subdomains-top1million-110000.txt**  
    📌 **المسار:** `SecLists/Discovery/DNS/subdomains-top1million-110000.txt`  
    ✅ قائمة تحتوي على **أكثر من 110,000 نطاق فرعي** مستخدمة في أفضل مليون موقع عالميًا.  
    🔥 **مفيدة جدًا عند اختبار المواقع الشهيرة.**
    
3. **subdomains-top1million-5000.txt**  
    📌 **المسار:** `SecLists/Discovery/DNS/subdomains-top1million-5000.txt`  
    ✅ نفس الفكرة، ولكن مع قائمة أصغر تضم **5000 نطاق فرعي شائع فقط**.  
    🏃‍♂️ أسرع في البحث، ومناسبة عند الحاجة لنتائج سريعة.
    
4. **dns-big.txt**  
    📌 **المسار:** `SecLists/Discovery/DNS/dns-big.txt`  
    ✅ تحتوي على عدد كبير جدًا من النطاقات الفرعية، لكن عملية البحث بها قد تكون بطيئة.  
    🛠️ مفيدة في عمليات المسح العميق (Deep Enumeration).
    
5. **fierce-hostlist.txt**  
    📌 **المسار:** `SecLists/Discovery/DNS/fierce-hostlist.txt`  
    ✅ قائمة متوازنة بين الحجم والسرعة، مفيدة جدًا عند البحث عن نطاقات فرعية محتملة.