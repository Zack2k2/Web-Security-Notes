
use wfuzz to find sub domains

use grep to find links in js files

```
cat index.html | grep -oP "setAttribute\('href',\s*'\K[^']+" 
 
cat index.html | grep -oP '(?<=href="|src=")[^"]+
```

use arjun to find hidden parameter

`python3 arjun.py -u http://insecure-site.com -o result.json`

