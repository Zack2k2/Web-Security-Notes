<h2>Finding them</h2>

We have a valid bug when:
1. We can access something without being logged on.
2. Access something on our account when logged onto another account.
3. Access some admin functionality, even if we don't have permission. 
4. Access data from another tenant.


<b>key take always</b>

cookie maintain your identity on a website.
basically including cookie is the same as your server
realizing your account information.

if you login to a account in a server the server 
sets up a session id which is sent as a cookie
to the client; the client then saves it.
Every time a client makes a request it attaches
the cookies initially sent by the server in 
order to identify use identity. 

removing the cookie in your HTTP request 
is the same as logged out from your account

<h2>tips and tricks</h2>

IDOR's and Business logic are everywhere
in web apps and API's

1. Keep an eye out for long string,
   like some numerical words, hash word,
   ID. 
   API's are full of IDOR.

2. Test every end point
3. Use Firefox account containers
4. keep an eye out for weird things
5. cyber chef


