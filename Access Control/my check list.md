- [ ] request for admin panels and information like /robots.txt
      ,/admin.txt

- [ ] try fuzzing for admin panels or sensative information
      with directories `/usr/share/wordlists/wfuzz/general`

- [ ] check client side code extract links that might disclose 
      information about admin, following commands might extract 
      links
      
		      `rg -oP "setAttribute\('href',\s*'\K[^']+"  Unprotected\ admin\functionality\ with\ unpredictable\ URL.html`

				`cat Unprotected\ admin\ functionality\ with\ unpredictable\ URL.html | grep -oP '(?<=href="|src=")[^"]+'`

				`cat Unprotected\ admin\ functionality\ with\ unpredictable\ URL.html | grep -oP "setAttribute\('href',\s*'\K[^']+" `

	check out `link finder` from github


- [ ] try parameter tampering link, first use arjun to find
      all parameters and try to apply them to gain access


- [ ] try messing with cookies.

- [ ] use non-standard HTTP headers that can be used to override the URL in the original request, such as `X-Original-URL` and `X-Rewrite-URL` to issue commands like /admin/deleteUser
      `POST / HTTP/1.1`
      `X-Original-URL: /admin/deleteUser`


<br>


- [ ] Change the HTTP Method of request to POST or GET.
- [ ] perform user_id tempering 
- [ ] perfoem GUID guessing by looking for features that might display 
		a GUID.
- [ ] Give GUID or id to an endpoint even if some endpoint doesn't ask one
- [ ] check login page if it revels any information about admin.

- [ ] change the request file type.
- [ ] change the end point version.
      for example `/apiv2/user_id=12 --> /apiv1/user_id=12`

- [ ] try adding extension when requesting a
      file `/user/file01 --> /user/file01.json` 
      file `/user/uploads?file=user1 --> /user/uploads?file=user1.jpeg`

- [ ] add any extention to any action endpoint like
      `/admin/addUser` might be block but `/admin/addUser.anything`
      would work.
      I.E:
      `/admin/addUser` --> `/admin/addUser.json`

- [ ] try dehashing the GUID or ID in link to figure 
      out a pattern. 

- [ ] try blind IDOR that has endpoint that would 
      send personal information to email, phone

- [ ] try using arjun to find all the parameters 
      a endpoint will accept.

  
