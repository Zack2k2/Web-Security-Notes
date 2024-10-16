<h3><u>What is stored XSS</u></h3>

A Stored XSS (also called first order and persistent xss)
arises when an application receives data from attacker 
and includes that data with in its later HTTP responses in a 
unsafe way. 
or to put more simply when a dangerous script or html is recived by 
the html it doesn't sanitize it and store it some, and 
that is also responded back to http respond which execute that 
script or htm

<h3>Difference between reflected XSS and stored XSS</h3>

<table>
	<tr>
		<td><b>Stored</b></td>
		<td><b>reflected</b></td>
</tr>
	<tr>
		<td>A stored XSS occur when a application 
			stores a http request with malicious payload
			and delivers that payload in its different http 
			responds.
		</td>
		<td>
			A reflected XSS occur when a Malicious http request
			with malicious payload is delivered to application 
			only for the application to respond with same 
			malicious payload in its http respond as a reflection
		</td>
	</tr>
	<tr>
		<td>
			A stored xss is more harmful then reflected XSS
			because it doesn't require user victim to click
			on any link or URI via social engineering.
			The script will execute once the user navigate 
			to the application compromised pages or features.
		</td>
		<td>
			A reflected xss require more social engineering to
			lure the victim in to clicking the compromised
			page URI so that malicious code can be executed 
			it his/her browser. This is less dangerous then
			stored XSS but history of hacking has shown 
			that the security is a good as the
			weakest link.so we must be cautious to reflected xss
			infact you can get paid alot of times to find
			reflected xss that can seriously compromise the 
			app.
		</td>
	</tr>
	<tr></tr>
</table>