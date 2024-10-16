to confirm some kind of XSS the current practice
is to use print() function in javascript.
The print function of the javascript open a print preview 
which will tell the browser that the web pages are ment to be printed on papers.
For a long time POC XSS is was proven with alert(1),
you can solve alot of xss ctf by using alert(1).
but from mid of 2021 chrome added cross-origin iframes
are prevented from calling alert(), we would need to learn some alternative payloads for xss.