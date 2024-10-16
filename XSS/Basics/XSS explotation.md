<h3><u>
Exploiting the XSS vulnerability
</u></h3>

One of the most common ways XSS can be exploited and 
can be used to demonstrate the severity of XSS in the POC report is session cookie stealing.

due to the stateless nature of HTTP; Session hijacking works because it is not possible to prevent it but it is possible to detect it.


<h3><u>Chaining low risk vulnerability in to high risk</u></h3>

Two or more low risk vulnerability can be combined to become a very high severity vulnerability.

for example in a website there was a stored XSS on welcome name for the user and another defective access control vulnerability existed that enabled us to change the user name of other users. Combining both of the weakness then it would catapole to a extremely high vulnerability that would enable a hacker to compromise the entire user base.
