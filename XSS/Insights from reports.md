<h2><u>Insights from reports.</u><h2>

1. XSS also happens on parameter. 
2. injection might occurs on `onsubmit` 
3. use url-encoding over js-fuck payload.
4. if the injectable exist with in js, try breaking 
   out with every way possible.you might have to break
   out of `'` quotes `"` closing functions or round brackets.
5. You might wanna try parameter pollution.
6. even parameters inside cookies might be reflected.
7. stored XSS might be found bay inserting payloads in title
   and second part of payload on the content of webpages.
8. escaping out of XSS payload might require you to write 
   come phrases of javascript like: 
	```
    ?utm_source=abc`;return false});});alert`xss`; 
	```
9. use URL encoding because some parameters might not 
   have been properly escaped for url encoding.
10. xss happens inside graphql so you need to first escape out of the gql and start writing the javascript.
11. If the graphql has a xss then try escaping to script section.
    Payload of:
	abc%60%3breturn+false%7d%29%3b%7d%29%3balert%60xss%60;%3c%2f%73%63%72%69%70%74%3e
	
	```
	is url encoded for
	```
	abc`;return+false});});alert`xss`;</script>
	```
	which is used like
	```
	abc`;                       Finish the string
	return+false});      Finish the jQuery click function
	});                            Finish the jQuery ready function
	alert\`xss\`;              Here we can execute our code
	</script> 

12. A xss might exist with in a object method of framework
    you might have to look up its documentations to 
    redefine its parameters

13. use several obfuscation techniques like html entity and
    base64 to encode your javascript code in side your payloads.

14. look for unsanitized reflected strings with in javascript tags.
15. look if your payload is being rendered with html tags or 
    with in javascript read [context of javascript](obsidian://open?vault=Web-hacking-techniques&file=XSS%2FXSS%20context)
    
16. XSS payload don't always execute immediately after
    they are submitted.  Because a payload could be used 
    in multiple locations on a site, be sure to visit each location where the payload is rendered. 

17. When the site sanitize the user input by modifying it 
    instead of encoding or escaping values, you should continue testing it and try crafting a payload that would take advantage of the modification engine to get a xss.

18. always be look out for URL parameters that might be 
    reflected in the web page because you have control over them and also use arjun to discover other parameters that might be reflected in the web page.

19. look into javascript every parameter in javascript front 
    end code. look into `window.location.href`


20. sources for link parameter to look how they are treated:
    1. `document.url`
    2. `document.referer`
    3. `location`
    4. `location.href`
    5. `location.search`
    6. `location.hash`
    7. `location.pathname`

21. check if any information in url path is being reflected 
    in webpage.

22. use `<img>` tag to circumvent the xss filtering
23. escape html comments its rare but keep it in mind.
24. alot of xss are found in login webpage.
25. look for xss on error pages.
26. always look into URL parameters
27. <mark>always and always look out for the URL
    parameters for the webapp and consider the
    context with in which they are reflected.</mark>
28. more parameters === more likey change of getting valid xss
29. throw `"><img src="E" onerror=javascript:alert(1)>`
    every where.