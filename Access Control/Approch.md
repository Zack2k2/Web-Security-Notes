<h2>Approach</h2>

<h3>How to find Access control?</h3>

<h3>1. check for unprotected functionality.</h3>
check for robots.txt check for 
admin panel through simple URL 
by using burate force wordlist to 
sensetive functionality. use wfuzz 

`https://insecure-website.com/admin`

<h3>2. check the client side code</h3>

like html, css, js to look for 
sensitive functionality url that might be hidden 
in html or js file by using commands like 

`rg -oP "setAttribute\('href',\s*'\K[^']+"  Unprotected\ admin\ functionality\ with\ unpredictable\ URL.html`

`cat Unprotected\ admin\ functionality\ with\ unpredictable\ URL.html | grep -oP '(?<=href="|src=")[^"]+'`

`cat Unprotected\ admin\ functionality\ with\ unpredictable\ URL.html | grep -oP "setAttribute\('href',\s*'\K[^']+" `

^ this commands would return the links and paths.


<h3>3. HTTP headers Parameter based</h3>

check for http parameters in burp and 
modify the requests to get privilege 
escalation for example a website:
```
-   Browse to `/admin` and observe that you can't access the admin panel.
-   Browse to the login page.
-   In Burp Proxy, turn interception on and enable response interception.
-   Complete and submit the login page, and forward the resulting request in Burp.
-   Observe that the response sets the cookie `Admin=false`. Change it to `Admin=true`.
```


<h3>4. tempering with state changing HTTP POST fuctions
</h3>

for example in a webapp a email changing functions
(or any changing functionality in a web app)

```
-   Log in using the supplied credentials and access your account page.
-   Use the provided feature to update the email address associated with your account.
-   Observe that the response contains your role ID.
-   Send the email submission request to Burp Repeater, add `"roleid":2` into the JSON in the request body, and resend it.
-   Observe that the response shows your `roleid` has changed to 2
```
now with role ID of 2 you can now act as admin.

<h3>5. Testing for bypassing Authorization 
Schema</h3>

Some Application restrict action by restricting 
access to specific URLs and HTTP methods like

`DENY: POST, /admin/deleteUser, managers`

This rule denies access to the `POST` method on the URL `/admin/deleteUser`, for users in the managers group.

you can perhaps circumvent this block by
using non standard HTTP headers that could over write
URL like such as `X-Original-URL` and `X-Rewrite-URL`.
like puting the action.

```
POST / HTTP/1.1 
X-Original-URL: /admin/deleteUser
```
but some times it might block the POST request.
You could use get but parameters.

<h3>6. Changing the URL method</h3>

Some websites are tolerant of other HTTP 
verbs like POST can be replaced like GET

```
GET /?username=carlos HTTP/1.1 
X-Original-URL: /admin/deleteUser
```

keep an eye out for `X-Original-Url` and `X-Rewrite-Url`

<h3>7. URL-matching discrepancies</h3>

When routing incoming requests, website backends
could vary in terms of strictness. It might be 
the case that is very lenient like not
case sensitive enough and `/admin/deleteUser` 
could be blocked but not `/ADMIN/DELETEUSER`

in rare cases in spring framework, what just
might happen would be tolarent of any 
extention like in old spring framework.

so consider this:
a request to `/admin/deleteUser.anything`
would map to `/admin/deleteUser`

or sometimes just appending a slash like `/`
would cause a bypass like consider this 
requesting `/admin/deleteUser/` would map
to `/admin/deleteUser` that might not 
have access control.

<h3>8. Horizontal Privilege escalation</h3>
<b>8.1 Simple Numric IDOR</b>
horizontal privilage escalation happens when
one account is able to take over other
account. there privileges and features.

horizontal escalation might have similar type of
exploit as vertical escalation.
for example a very rare HPE is numeric IDOR


`https://insecure-website.com/myaccount?id=123`

where a attacker could change id=125
and get access to id.

the parameter id or other parameter can be modified.
to get an IDOR it might be some predictable value.


<b>8.2 Unintentionally Disclosed ID</b>

In some applications, the exploitable parameters
doesn't have predictable values. but they could 
have been reviled 

<b>8.3 Sensitive data in login page.</b>

sometimes when you are redirected into a login page. 
how ever login page might revel some sensitive
information.

<h3>9. HPE to VPE</h3>

a horizontal privilege escalation could become vertical
privilege escalation if an application have 
a HPE bug and utilizing that bug attacker compromises
an admin or an account with higher privilege then 
it could become vertical privilege escalation.

for example if HPE exist in an app due to parameter 
tampering then an attacker could target the app admin page
which would cause VPE. The admin page might even
disclose the admin page password.

<h3>10. IDOR</h3>

[[IDOR]]



<h3>11. Access control vulnerabilities in 
multi-step processes</h3>

Many websites will implement important functions
over a series of steps: step 1, 2 and 3.
1 and 2 would have serious access control 
but 3 wouldn't so a attacker could just 
issue a HTTP request with required parameters
to step 3 end point skipping the first two.
because web app would
assume that only way to reach step 3 would 
be to first go through step 1 and step 2.

so just try to submit the insecure step HTTP
request with other parameters. 

<h3>12. Referer-based access control</h3>

many web sites check would only referrer to authenticate the identity of a user 

for example
```
GET /users/id023 HTTP1.1    -> 403 Forbidden
Referer: example.com/users/id01
```

```
GET /USER/id023 HTTP1.1      -> 200 OK
Referer: example.com/users/id023
```
