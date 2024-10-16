<h2>CSRF</h2>

CSRF occur when a action occur on web application on behalf of other unsuspecting users with out the permission, that is orchestrated by
the hacker who manage to send a HTTP request to 
the main server tricking the server into thinking 
that the victim has issued this HTTP request.

<h2>Hunting for CSRF's</h2>

When you make a POST request the browser put the 
cookies in the html form

<h3>Step 1:Spot State-Changing Actions</h3>

state changing request are usually POST HTTP 
request capture them on burp.
state changing request under POST would
probably be changing email, password changing,
or name changeling. 

<h3>Step 2:Look for a lack of CSRF protections</h3>

look for any lack of csrf protection.


<h3>Step 3:Confirm the vulnerability</h3>

craft the html POC.