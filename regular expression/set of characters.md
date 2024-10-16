Regular expressions allow us us to match highly complicated patterns. 
In a regular expression, say putting a set of characters; that allow us to match numbers. 
putting a set of characters between two squares brackets:

1. /[0123456789]/g will match all digit that occur in the string.
2. /[aeiou]/ will match all vowels in the string.
3. /[^aeiou]/  will match all the consonants. 
4. /[a-z]/ will match all lower case English alphabet. 
5. /[0-9]/ equivalent to /[0123456789]/

The hyphen(-) between the two characters indicate range of characters. Where the ordering is determined by Unicode numbers. 

**Short cuts groups**

There are shortcut groups with in regex that are built in.

\d: any digit character.
\w: an alphanumeric character("word character")
\s: any white spaces line space, newline, tab.
\D: a character that is not a digit (equivalent to[ ^\d ])
\W: a non-alphanumerical character.
\S: a non-whitespace character.
.  Any character execpt for new line
\b: word boundary  