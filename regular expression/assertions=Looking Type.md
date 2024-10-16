**Lookahead assertion:**

`x(?=y)`

Matches "x" only if "x" is followed by "y". For example, /`Jack(?=Sprat)/` matches "Jack" only if it is followed by "Sprat".  
`/Jack(?=Sprat|Frost)/` matches "Jack" only if it is followed by "Sprat" or "Frost". However, neither "Sprat" nor "Frost" is part of the match results.

**Negative lookahead assertion:**
`x(?!y)`

Matches "x" only if "x" is not followed by "y". For example, `/\d+(?!\.)/` matches a number only if it is not followed by a decimal point. `/\d+(?!\.)/.exec('3.141')` matches "141" but not "3".


**Lookbehind assertion:** 

`(?<=y)x`

Matches "x" only if "x" is preceded by "y". For example, `/(?<=Jack)Sprat/` matches "Sprat" only if it is preceded by "Jack". `/(?<=Jack|Tom)Sprat/` matches "Sprat" only if it is preceded by "Jack" or "Tom". However, neither "Jack" nor "Tom" is part of the match results.

**Negative lookbehind assertion:**

`(?<!y)x`

Matches "x" only if "x" is not preceded by "y". For example, `/(?<!-)\d+/` matches a number only if it is not preceded by a minus sign. `/(?<!-)\d+/.exec('3')` matches "3". `/(?<!-)\d+/.exec('-3')` match is not found because the number is preceded by the minus sign.