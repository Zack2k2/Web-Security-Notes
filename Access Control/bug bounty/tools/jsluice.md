# jsluice command-line tool

The `jsluice` command-line tool extracts URLs, paths, secrets, and other interesting bits
from JavaScript files.

Values are extracted based not just on how they *look*, but also based on how they are *used*.  

That means `jsluice` can find the path in this code:

```javascript
fetch('/api/users?id=' + userId + '&format=json', {
  method: "GET",
  headers: {
    "X-Env": "stage"
  }
})
```

But also the method, and headers:

```
▶ jsluice urls demo.js | jq
{
  "url": "/api/users?id=EXPR&format=json",
  "queryParams": ["id", "format"],
  "method": "GET",
  "headers": {
    "X-Env": "stage"
  },
  "type": "fetch"
}
```

Because `jsluice` is doing [static analysis](https://en.wikipedia.org/wiki/Static_program_analysis) it
can't know the value of that `userId` variable, but it *does* understand string concatenation. The value
of expressions like this are replaced with `EXPR` by default, but that can be changed with the
`-P`/`--placeholder` flag.

## Contents
* [Installation](#install)
* [Usage](#usage)
    * [Extracting URLs](#extracting-urls)
        * [Resolving Relative Paths](#resolving-relative-paths)
        * [Including Original Source](#including-original-source)
    * [Extracting Secrets](#extracting-secrets)
        * [Custom Secret Matchers](#custom-secret-matchers)
    * [Printing Syntax Trees](#printing-syntax-trees)
    * [Running Queries](#running-queries)
    * [Getting help](#help)

## Install

To install `jsluice` you need [Go](https://go.dev/doc/install).

Once Go is installed and configured, run:

```
▶ go install github.com/BishopFox/jsluice/cmd/jsluice@latest
```

If everything worked correctly, you should be able to run `jsluice --help` and
see the [help output](#help).


## Usage

Provide `jsluice` with a mode, any options, and a list of JavaScript files:

```
jsluice <mode> [options] [file...]
```

You can also provide files one-per-line on `stdin`:

```
find . -name '*.js' | jsluice <mode> [options]
```

`jsluice` has four modes:
* `urls` - for extracting URLs and paths
* `secrets` - for finding secrets and so on
* `tree` - for printing syntax trees
* `query` - for running tree-sitter queries

Output is in [JSONL](https://jsonlines.org/) format. Piping `jsluice` to a tool
like [jq](https://jqlang.github.io/jq/) allows for human-readable formatting,
filtering and further processing.

### Extracting URLs

In `urls` mode, `jsluice` extracts URLs and paths from several different places:

* Assignments to document.location, val.href, val.src etc
* Calls to location.replace, window.open, and fetch
* Uses of XMLHttpRequest
* Calls to jQuery's $.get, $.post, and $.ajax
* Any string literal that contains something that looks like a URL

If you want to ignore string-literal matches you can use the `-I`/`--ignore-strings` flag.

When possible, HTTP methods, headers etc are also extracted.

Here's a call to [jQuery](https://jquery.com/)'s `$.ajax` as an example:

```javascript
$.ajax({
    method: "PUT",
    url: "/api/v1/posts",
    data:{ postId: 324 },
    headers: {
        "Content-Type": "application/json",
        "x-backend": "prod"
    }},
    function(data, status){
        location.href = data.redirect;
    }
)
```

And the output from `jsluice`:

```
▶ jsluice urls jquery.js | jq
{
  "url": "/api/v1/posts",
  "queryParams": [],
  "bodyParams": [
    "postId"
  ],
  "method": "PUT",
  "headers": {
    "Content-Type": "application/json",
    "x-backend": "prod"
  },
  "type": "$.ajax",
  "filename": "jquery.js"
}
```

#### Resolving Relative Paths

Relative paths can be resolved using a base URL provided with the `-R`/`--resolve-paths` flag.

```
▶ cat location.js
document.location = '../../guestbook.html'

▶ jsluice urls location.js -I -R https://example.com/~tom/photos/2003/ | jq
{
  "url": "https://example.com/~tom/guestbook.html",
  "queryParams": [],
  "bodyParams": [],
  "method": "GET",
  "type": "locationAssignment",
  "filename": "location.js"
}
```

#### Including Original Source

Sometimes it's useful to be able to see the complete source code that a URL was extracted from.
Using the `-S`/`--include-source` flag adds a `source` field to the results containing that source code:

```
▶ jsluice urls location.js -I -S | jq
{
  "url": "../../guestbook.html",
  "queryParams": [],
  "bodyParams": [],
  "method": "GET",
  "type": "locationAssignment",
  "source": "document.location = '../../guestbook.html'",
  "filename": "testdata/relative-location.js"
}
```

### Extracting Secrets

The `secrets` mode is for extracting API keys, passwords, and other interesting bits of data.

There are built-in extractors for:

* AWS keys
* GCP keys
* GitHub keys
* Firebase configurations

That's not very many, so you can supply your own in a file specified with the `-p`/`--patterns` flag.

Here's an example of some JavaScript that contains an AWS key:

```javascript
var config = {
    bucket: "examplebucket",
    awsKey: "AKIAIOSFODNN7EXAMPLE",
    awsSecret: "wJalrXUtnFEMI/K7MDENG/bPxRfiCYEXAMPLEKEY",
    server: "someserver.example.com"
};
```

And the output of `jsluice secrets` when run against that file:

```
▶ jsluice secrets awskey.js | jq
{
  "kind": "AWSAccessKey",
  "data": {
    "key": "AKIAIOSFODNN7EXAMPLE",
    "secret": "wJalrXUtnFEMI/K7MDENG/bPxRfiCYEXAMPLEKEY"
  },
  "filename": "awskey.js",
  "severity": "high",
  "context": {
    "awsKey": "AKIAIOSFODNN7EXAMPLE",
    "awsSecret": "wJalrXUtnFEMI/K7MDENG/bPxRfiCYEXAMPLEKEY",
    "bucket": "examplebucket",
    "server": "someserver.example.com"
  }
}
```

The key and associated secret are put in the `data` field with predictable names to ease
the automation of, for example, checking the validity of found secrets.

The entire object in which the secret was found is included in the `content` field.

#### Custom Secret Matchers

A JSON file containing an array of pattern objects can be supplied using the `-p`/`--patterns` flag.

Here's an example of a basic patterns file:

```json
[ 
  { 
    "name": "base64", 
    "value": "(eyJ|YTo|Tzo|PD[89]|rO0)[%a-zA-Z0-9+/]+={0,2}", 
    "severity": "low" 
  }, 
  { 
    "name": "genericSecret", 
    "key": "(secret|private|key)", 
    "value": "[%a-zA-Z0-9+/]+" 
  },
  {
    "name": "firebaseConfig",
    "severity": "high",
    "object": [
      {"key": "apiKey", "value": "^AIza.+"},
      {"key": "authDomain"},
      {"key": "projectId"},
      {"key": "storageBucket"}
    ]
  }
] 
```

Each pattern can have the following fields:

* `name`, which is used in the output
* `severity`, which should be one of `info`, `low`, `medium`, or `high`
* `value`, a regular expression to match against string values
* `key`, a regular expression to match against key names
* `object`, an array of patterns to match against the keys and values of an entire object

All regular expressions use the [Go regex syntax](https://pkg.go.dev/regexp/syntax).

Here's a, somewhat silly, example JavaScript file to run the patterns file against:

```javascript
function getConfig(){ 
    let config = { 
        randomStr: "abc123xyz256", 
        secret: "I quite like PHP", 
    } 
    return "eyJsb2wiOiAic29tZSBKU09OISIsICJjb3VudCI6IDEyM30K" 
} 
```

Running `jsluice secrets` using the above patterns file (saved as `patterns.json`):

```
▶ jsluice secrets -p patterns.json simple-b64.js | jq
{
  "kind": "base64",
  "data": {
    "match": "eyJsb2wiOiAic29tZSBKU09OISIsICJjb3VudCI6IDEyM30K"
  },
  "filename": "simple-b64.js",
  "severity": "low",
  "context": null
}
{
  "kind": "genericSecret",
  "data": {
    "key": "secret",
    "value": "I quite like PHP"
  },
  "filename": "simple-b64.js",
  "severity": "info",
  "context": {
    "randomStr": "abc123xyz256",
    "secret": "I quite like PHP"
  }
}
```

Note that the `base64` matcher worked as expected, but the `genericSecret` matcher
returned a rather different sort of secret than expected. That's because the regular
expression lacks [anchors](https://www.regular-expressions.info/anchors.html):

```
[%a-zA-Z0-9+/]+
```

If you wanted the match against all of the value, the regex could be changed to:

```
^[%a-zA-Z0-9+/]+$
```

### Printing Syntax Trees

The `tree` mode prints a textual represenation of the syntax tree for each JavaScript file.
This is especially helpful when [writing queries](#running-queries).

The output can be quite long, so here's a tiny example program:

```javascript
console.log("Hello, world!")
```

And the output of `jsluice tree`:

```
▶ jsluice tree hello.js
hello.js:
program
  expression_statement
    call_expression
      function: member_expression
        object: identifier (console)
        property: property_identifier (log)
      arguments: arguments
        string ("Hello, world!")
```

### Running Queries

The `query` mode lets you run [Tree-sitter](https://tree-sitter.github.io/tree-sitter/) queries against JavaScript files.
The query syntax is fully documented [here on the Tree-sitter project site](https://tree-sitter.github.io/tree-sitter/using-parsers#query-syntax).

Just about the most simple query you could run extracts all of the string literals from the input files.

Here's an example file to try it with:

```javascript
const config = {
    stage: false,
    server: "example.com",
    ttl: 3600,
    dns: ["1.1.1.1", "8.8.8.8"],
    paths: {
        "home": "/",
        "blog": "/blog"
    }
}
```

And how to run the query:

```
▶ jsluice query -q '(string) @str' config.js
"example.com"
"1.1.1.1"
"8.8.8.8"
"home"
"/"
"blog"
"/blog"
```

The `@str` part of the query identifies which part of the query should be extracted.
In this case there is only one thing to match in the query, but it is still required.

`jsluice` tries to make the output valid JSONL where possible, and because it understands
objects, arrays, strings, etc: it's possible to get JSON represenations of those things
as output:

```
▶ jsluice query -q '(object) @match' config.js | jq
{
  "dns": [
    "1.1.1.1",
    "8.8.8.8"
  ],
  "paths": {
    "blog": "/blog",
    "home": "/"
  },
  "server": "example.com",
  "stage": false,
  "ttl": 3600
}
{
  "blog": "/blog",
  "home": "/"
}
```

If you don't want that to happen, you can use the `-r`/`--raw-output` flag.


### Help

You can see the `jsluice` help output with the `-h`/`--help` flag.

```
▶ jsluice --help
jsluice - Extract URLs, paths, and secrets from JavaScript files

Usage:
  jsluice <mode> [options] [file...]

Modes:
  urls      Extract URLs and paths
  secrets   Extract secrets and other interesting bits
  tree      Print syntax trees for input files
  query     Run tree-sitter a query against input files

Global options:
  -c, --concurrency int        Number of files to process concurrently (default 1)
  -P, --placeholder string     Set the expression placeholder to a custom string (default 'EXPR')

URLs mode:
  -I, --ignore-strings         Ignore matches from string literals
  -S, --include-source         Include the source code where the URL was found
  -R, --resolve-paths <url>    Resolve relative paths using the absolute URL provided

Secrets mode:
  -p, --patterns <file>        JSON file containing user-defined secret patterns to look for

Query mode:
  -q, --query <query>          Tree sitter query to run; e.g. '(string) @matches'
  -r, --raw-output             Do not JSON-encode query output

Examples:
  jsluice urls example.js
  jsluice query -q '(object) @m' one.js two.js
  find . -name *.js' | jsluice secrets -c 5 --patterns=apikeys.json
```
