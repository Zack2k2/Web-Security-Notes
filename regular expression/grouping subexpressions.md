to use operators like * ,+ and ? you need to use a group 
by enclosing elements between parentheses.  
A part of a regular expression that is enclosed in 
parentheses counts as a single element.




**Capturing group:** 
(x) Matches x and remembers the 
match. For example, `/(foo)/` matches and remembers "foo" in "foo bar".

(?<name>x): **Named capturing group:** Matches "x" and stores it on the groups property of the returned matches under the name specified by `<Name>`. The angle brackets (`<` and `>`) are required for group name.



For example, to extract the United States area code from a phone number, we could use `/\((?<area>\d\d\d)\)/`. The resulting number would appear under `matches.groups.area`.

// Define the regular expression
const pattern = /(?<year>\d{4})-(?<month>\d{2})-(?<day>\d{2})/;

// Apply the regular expression to an input string
const m = pattern.exec('Date: 2022-12-15');

// Check if a match was found
if (m) {
  // Print the values of the named capturing groups
  console.log(`Year: ${m.groups.year}`);
  console.log(`Month: ${m.groups.month}`);
  console.log(`Day: ${m.groups.day}`);
} else {
  console.log('No match found');
}