A regular expression is just pattern, specifies a set of 
strings.
A simple way to specify a finite set of strings is to list its elements or members. 


all pattern must be between / and /. like /PATTERN/
all normal alphabetic and numerical characters matching must be kept simple.
for global fetching of patterns put 'g' at the end of two forward backslash  /PATTERN/g, will fetch all occurrences of PATTERN in strings.

/blue earth/  : This will simply pattern match the first occurrence of words 'blue earth' 
/blue earth/g : This will match all occurrences of words 'blue earth'

use backslash to disable some special character meaning
like +,*,. and ? but most of the time \ are used to denote special
characters

/18\+/ : This will match string '18+' in string
/18+/ : This will match "18", "188", "1888" and so on but not "1"

because + denotes that one or more occurrences of preceding element
For example, /ab+c/ matches "abc", "abbc", "abbbc", and so on, but not "ac".

  
because * : The asterisk indicates zero or more occurrences of the preceding element. For example, ab*c matches "ac", "abc", "abbc", "abbbc", and so on.

/today\?/g : matches every occurrences of "today?"

The question mark ? makes the preceding element optional meaning that it may occur zero times or one time.



