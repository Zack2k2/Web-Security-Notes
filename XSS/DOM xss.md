Client side javascript can access and parse the URL and identify the parameters value, because the client-side javascript can access the DOM (document object model) and can determine the URL to load the current page. A script used with in the page can be used to determine the parameters of that URL. The script parses some parameter to dynamically generate some response in the page. when an app does this it may be vulnerable to DOM xss.

DOM based vulnerability is some what similar to reflected XSS because it still requires attacker to use social engineering to click on the link with malicious parameter; but the main difference is the when server responds with the webpages it does not have any malicious script
instead the malicious script is feed through url its the client side script that execute malicious parameter.


<b>DOM XSS are very hard to detect or prevent from server side as no malicious code is present from Server POV.</b>