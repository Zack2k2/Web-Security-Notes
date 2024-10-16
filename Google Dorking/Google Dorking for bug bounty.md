<h2><u>Bug Bounty for google dorking</u></h2>

- PHP extension w/ parameters
    
    [site:example.com ext:php inurl:?](https://www.google.com/search?q=site%3Aexample.com%20ext%3Aphp%20inurl%3A%3F)
- Disclosed XSS and Open Redirects
    
    [site:openbugbounty.org inurl:reports intext:"example.com"](https://www.google.com/search?q=site%3Aopenbugbounty.org%20inurl%3Areports%20intext%3A%22example.com%22)
- Juicy Extensions

`inurl:example.com intitle:"index of"`  
`inurl:example.com intitle:"index of /" "*key.pem"`  
`inurl:example.com ext:log`  
`inurl:example.com intitle:"index of" ext:sql|xls|xml|json|csv`  
`inurl:example.com "MYSQL_ROOT_PASSWORD:" ext:env OR ext:yml -git`  
`inurl:example.com intitle:"index of" "config.db"`  
`inurl:example.com allintext:"API_SECRET*" ext:env | ext:yml`  
`inurl:example.com intext:admin ext:sql inurl:admin`  
`inurl:example.com allintext:username,password filetype:log`  
`site:example.com "-----BEGIN RSA PRIVATE KEY-----" inurl:id_rsa`  
`site:*.gov.* "responsible disclosure"`



[site:"example[.]com" ext:log | ext:txt | ext:conf | ext:cnf | ext:ini | ext:env | ext:sh | ext:bak | ext:backup | ext:swp | ext:old | ext:~ | ext:git | ext:svn | ext:htpasswd | ext:htaccess](https://www.google.com/search?q=site%3A%22example%5B.%5Dcom%22%20ext%3Alog%20%7C%20ext%3Atxt%20%7C%20ext%3Aconf%20%7C%20ext%3Acnf%20%7C%20ext%3Aini%20%7C%20ext%3Aenv%20%7C%20ext%3Ash%20%7C%20ext%3Abak%20%7C%20ext%3Abackup%20%7C%20ext%3Aswp%20%7C%20ext%3Aold%20%7C%20ext%3A~%20%7C%20ext%3Agit%20%7C%20ext%3Asvn%20%7C%20ext%3Ahtpasswd%20%7C%20ext%3Ahtaccess)

- XSS prone links search

**advanced google dorking**

Dorks I always use

site:*.host.com ext:asp

site:*.host.com ext:jsp

site:*.host.com ext:aspx

site:*.host.com ext:jspx

site:*.host.com ext:do

site:*.host.com ext:action

site:*.host.com ext:php

- some other dorks
    [inurl:q= | inurl:s= | inurl:search= | inurl:query= | inurl:keyword= | inurl:lang= inurl:& site:example.com](https://www.google.com/search?q=inurl%3Aq%3D%20%7C%20inurl%3As%3D%20%7C%20inurl%3Asearch%3D%20%7C%20inurl%3Aquery%3D%20%7C%20inurl%3Akeyword%3D%20%7C%20inurl%3Alang%3D%20inurl%3A%26%20site%3Aexample.com)
- Open Redirect prone parameters
    
    [inurl:url= | inurl:return= | inurl:next= | inurl:redir= inurl:http site:example.com](https://www.google.com/search?q=inurl%3Aurl%3D%20%7C%20inurl%3Areturn%3D%20%7C%20inurl%3Anext%3D%20%7C%20inurl%3Aredir%3D%20inurl%3Ahttp%20site%3Aexample.com)
- SQLi Prone Parameters
    
    [inurl:id= | inurl:pid= | inurl:category= | inurl:cat= | inurl:action= | inurl:sid= | inurl:dir= inurl:& site:example.com](https://www.google.com/search?q=inurl%3Aid%3D%20%7C%20inurl%3Apid%3D%20%7C%20inurl%3Acategory%3D%20%7C%20inurl%3Acat%3D%20%7C%20inurl%3Aaction%3D%20%7C%20inurl%3Asid%3D%20%7C%20inurl%3Adir%3D%20inurl%3A%26%20site%3Aexample.com)
- SSRF Prone Parameters
    
    [inurl:http | inurl:url= | inurl:path= | inurl:dest= | inurl:html= | inurl:data= | inurl:domain= | inurl:page= inurl:& site:example.com](https://www.google.com/search?q=inurl%3Ahttp%20%7C%20inurl%3Aurl%3D%20%7C%20inurl%3Apath%3D%20%7C%20inurl%3Adest%3D%20%7C%20inurl%3Ahtml%3D%20%7C%20inurl%3Adata%3D%20%7C%20inurl%3Adomain%3D%20%20%7C%20inurl%3Apage%3D%20inurl%3A%26%20site%3Aexample.com)
- LFI Prone Parameters
    
    [inurl:include | inurl:dir | inurl:detail= | inurl:file= | inurl:folder= | inurl:inc= | inurl:locate= | inurl:doc= | inurl:conf= inurl:& site:example.com](https://www.google.com/search?q=inurl%3Ainclude%20%7C%20inurl%3Adir%20%7C%20inurl%3Adetail%3D%20%7C%20inurl%3Afile%3D%20%7C%20inurl%3Afolder%3D%20%7C%20inurl%3Ainc%3D%20%7C%20inurl%3Alocate%3D%20%7C%20inurl%3Adoc%3D%20%7C%20inurl%3Aconf%3D%20inurl%3A%26%20site%3Aexample.com)
- RCE Prone Parameters
    
    [inurl:cmd | inurl:exec= | inurl:query= | inurl:code= | inurl:do= | inurl:run= | inurl:read= | inurl:ping= inurl:& site:example.com](https://www.google.com/search?q=inurl%3Acmd%20%7C%20inurl%3Aexec%3D%20%7C%20inurl%3Aquery%3D%20%7C%20inurl%3Acode%3D%20%7C%20inurl%3Ado%3D%20%7C%20inurl%3Arun%3D%20%7C%20inurl%3Aread%3D%20%20%7C%20inurl%3Aping%3D%20inurl%3A%26%20site%3Aexample.com)
- High % inurl keywords
    
    [inurl:config | inurl:env | inurl:setting | inurl:backup | inurl:admin | inurl:php site:example[.]com](https://www.google.com/search?q=inurl%3Aconfig%20%7C%20inurl%3Aenv%20%7C%20inurl%3Asetting%20%7C%20inurl%3Abackup%20%7C%20inurl%3Aadmin%20%7C%20inurl%3Aphp%20site%3Aexample%5B.%5Dcom)
- Sensitive Parameters
    
    [inurl:email= | inurl:phone= | inurl:password= | inurl:secret= inurl:& site:example[.]com](https://www.google.com/search?q=inurl%3Aemail%3D%20%7C%20inurl%3Aphone%3D%20%7C%20inurl%3Apassword%3D%20%7C%20inurl%3Asecret%3D%20inurl%3A%26%20site%3Aexample%5B.%5Dcom)