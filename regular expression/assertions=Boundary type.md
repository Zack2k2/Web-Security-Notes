`^` : Matches the beginning of input. If the multiline flag is set to true, also matches immediately after a line break character. For example, `/^A/` does not match the "A" in "an A", but does match the first "A" in "An A".

`$` : Matches the end of input. If the multiline flag is set to true, also matches immediately before a line break character. For example, `/t$/` does not match the "t" in "eater", but does match it in "eat".

`\b` : Matches a word boundary. This is the position where a word character is not followed or preceded by another word-character, such as between a letter and a space. Note that a matched word boundary is not included in the match. In other words, the length of a matched word boundary is zero.

Examples:

-   `/\bm/` matches the "m" in "moon".
-   `/oo\b/` does not match the "oo" in "moon", because "oo" is followed by "n" which is a word character.
-   `/oon\b/` matches the "oon" in "moon", because "oon" is the end of the string, thus not followed by a word character.
-   `/\w\b\w/` will never match anything, because a word character can never be followed by both a non-word and a word character.

To match a backspace character (`[\b]`)

`\B` : Matches a non-word boundary. This is a position where the previous and next character are of the same type: Either both must be words, or both must be non-words, for example between two letters or between two spaces. The beginning and end of a string are considered non-words. Same as the matched word boundary, the matched non-word boundary is also not included in the match. For example, `/\Bon/` matches "on" in "at noon", and `/ye\B/` matches "ye" in "possibly yesterday".