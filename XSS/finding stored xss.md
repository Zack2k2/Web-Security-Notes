<h3><u>How to find stored XSS</u></h3>

1. Its is hard to find stored XSS, basically you need to find
   all relevant "entry points" via which attacker-controllable
   data can be entered in to application processing and storing
   units. you need to test all entry points.

2. and all exit points at which that data might appear in to
   application responds.

<b>Entry points:</b>
<ul>
	<li>
		parameters or other data within the URL query string and 
		message body
	</li>
	<li>
		http request headers that might not be exploitable in
		relation to reflected xss
	</li>
	<li>
		The URL file path.
	</li>
	<li>
		Any out-of-band routes via which an attacker can deliver data into the application. The routes that exist depend entirely on the functionality implemented by the application: a webmail application will process data received in emails; an application displaying a Twitter feed might process data contained in third-party tweets; and a news aggregator will include data originating on other web sites.
	</li>
</ul>

<b>Exit points:</b>

Exit points refers to point of output and all possible responds in all HTTP where entry points input are reflected. 

The first step into finding stored XSS attacks is to locate
the links between entry and exit points. the reason 
why this can be hard is:

<ul>
	<li>
		data submitted to any entry point could in principle
		be emitted from any point. For example your 
		user-supplied display names could appear within
		an obscure audit log that is only visible to
		some users.
	</li>
	<li>
		Data that is currently stored by application
		is often vuln to be over written.
		for example the most searched might be replaced from 
		other searches.
	</li>
</ul>

To identify stored XSS we would need to be very carefull
first we would need to submit XSS into every entry point 
and to identify it we must navigate to the correct exit 
point of page separately 

