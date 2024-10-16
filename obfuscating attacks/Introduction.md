both client and servers uses encoding and decoding to read and exchange data between devices.

when they want to read(use) the data they would first decode it first
the sequence of decoding steps depends on the context they appear in.


<b><h3>
Context specific decoding
</h3></b>
when constructing attacks, you should know about the location of payload, how it will be decoded in server side based on context, you can potentially 
**identitfy alternative ways** to represent the same payload

<mark>Note: keep track of how server decodes the payload because you can use it to represent the same payload in alternative ways</mark>

<b><h3>Decoding discrepancies</h3></b>

injection attacks involve injecting payloads that are recognizable.
such as html tags,javascript code or SQL statements.
websites often block suspecious payloads.

<mark>how ever this kinds of blocking/filters often fails because there input is not properly decoded. <b>it's vital that decoding performed when checking the input is the same as decoding performed by the end-server. basically for the filter must have the same decoder as the back end server. any discrepansies would allow hackers to sneak in harm full payloads.</b> </mark>

<b><h3>url encoding / perecentage encoding</h3></b>

Percentage encoding, also known as URL encoding, is a mechanism for encoding information in a Uniform Resource Identifier (URI) under certain circumstances. It involves replacing certain characters with a percent sign (%) followed by two hexadecimal digits that represent the character in the ASCII table.

For example, the space character is not allowed in a URI, so it must be encoded. The ASCII value of the space character is 32, which can be represented in hexadecimal as 20. Therefore, a space would be represented as %20 in a URI.

Percentage encoding is used to encode characters that are not allowed in a URI, to encode characters that have a special meaning in a URI, or to encode characters that could be misinterpreted or changed during the transmission of a URI.



<u><b><h3>obfuscation via html encoding</h3></b></u>

some times html characters are represented as starting with '&'
to present them as characters. for example 
`&#x61` is character 'a' 

& is entity reference An entity reference is a way to represent special characters in HTML that have a reserved meaning

The `#x` notation is used in character references to represent a Unicode character in hexadecimal notation. The `#x` notation is used to specify a Unicode code point in hexadecimal form.

61 is a in hexadecimal.

you can specify unicode character in decimal to for example

`&#00058` is `:` 

![[Pasted image 20230107065817.png]]


Interestingly, when using decimal or hex-style HTML encoding, you can optionally include an arbitrary number of leading zeros in the code points. Some WAFs and other input filters fail to adequately account for this.

the output in browser
![[Pasted image 20230107065949.png]]

![[Pasted image 20230107070011.png]]


<br>
<br>
<b><u><h3>
	Obfuscation via XML encoding
</h3></u></b>

XML is like HTML, it also supports character encoding and escape sequences.
this often causes you to include special characters in text content of elements without breaking it.
This causes you to  inject xss via XML input, XML accepts input in plain ascii and with encoding too, so i can use this behavior to bypass WAF
because if you put encoded characters in the XML entity then there is 
probability that it would be encoded by the server not the client app.

<b><h3><u>Obfuscating using hex escaping</u></h3></b>

Just like unicode escapes, these will be decoded client-side as long as the input is evaluated as a string:

`eval("\x61lert")`

Note that you can sometimes also obfuscate SQL statements in a similar manner using the prefix `0x`. For example, `0x53454c454354` may be decoded to form the `SELECT` keyword.

<b><h3><u>Obfuscating using oct escaping</u></h3></b>



