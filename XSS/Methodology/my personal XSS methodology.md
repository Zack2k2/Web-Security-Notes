<h2><u>My persoanl XSS methodology</u></h2>

1. take one subdomain. like www.example.com.
2. perform google dorking to find potentially vulnerable xss links with parameters.
3. perform gau or waybackurl on there links to
   find other parameters.
4. try getting hidden parameters by using `x8`
   by appropriate wordlists
5. try putting `xss` payloads:
	1. try simple payload
	2. obfuscate payload
	3. try URL encoding
	4. try double URL encoding
	5. try exotic payloads.
	6. obfuscate non-alphanumerical characters with unicode escaping and special alphanumerical in side javascript  characters with octal encoding.
	7. keep in mind about base64.
	8. keep in mind about html encoding.
	9. mix html encoding and octal escaping 
	   then URL encode the payload.
 