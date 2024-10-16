because a lot of characters in ascii like ?,/,&,#,= and + have special meaning 
that is used by URL to specify some functionality:

? : start of query string
& : addition query
'+' : space in javascript
'#' : page segment 
'=' : assignment of parameter to a value
/ : directory path seperator
<, > in order to not be interperated as html tags 

and there are characters that represented to have special meaning 
so that why we need to encode these special characters if they 
appear in parameters.

<u><b><h3>obfucating input in URL</h3></b></u>


Occasionally, you may find that WAFs and suchlike fail to properly URL decode your input when checking it. In this case, you may be able to smuggle payloads to the back-end application simply by encoding any characters or words that are blacklisted. For example, in a SQL injection attack, you might encode the keywords, so `SELECT` becomes `%53%45%4C%45%43%54` and so on.


<u><b><h3>double url encoding</h3></b></u>

Let's say you're trying to inject a standard XSS PoC, such as `<img src=x onerror=alert(1)>`, via a query parameter. In this case, the URL might look something like this:

`[...]/?search=%3Cimg%20src%3Dx%20onerror%3Dalert(1)%3E`
if this url is properly decoded by WAF then it would be immediately block
but you could double encode url, there is a chance that WAF might only decode once and just let it slide to the server which performs double decode so the aURLttack would be dilevered to the server.

the `%` characters themselves are then replaced with `%25`:
`[...]/?search=%253Cimg%2520src%253Dx%2520onerror%253Dalert(1)%253E`
