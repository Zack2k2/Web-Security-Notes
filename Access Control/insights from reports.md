<h2><u>Analysis and points from reports</u></h2>

1. Keep an Keen eye out on API

2. keep an eye out for numbers in path and/or in 
   query for url. like: `/user/msgs/43933` or
   `/user/chat/msgs_id=43933`
3. Look deep inside the POST and GET requests.
4. tinker with all the DELETE requests.
5. look and tempter with query/parameter like
   with `id` in it `skill_id` and `profile_id`
6. things unseen in UI app button might appear 
   in burp suit  
7. look deep inside graphql data you might find 
   some enumerable value which you could enumerate.
8. Look carefully into parameters for base64 numbers.
9. look PUT requests.
10. look into requests date because some parameters might be base64 encoded.
11. IDOR can be wraped with in functionalities 
    so test with every permutations of enable and disable.
12. Try changing some numerical attribute from attacker
    http request to victim numerical attribute.
13. look carefully into http request because it
    might have some base64 parameters/values, that
    could potentially lead to access control.
14. look into identification parameters. 
15. `DELETE` http request might be vulnerable.
16. look in to functionality in API/web that is 
    used to display some information and replace the 
    identifier of that information, like `newsletterid`
    to the victims ones.(might be base64 encoded)
17. some `id` parameter might be buried inside the
    `POST` or `GET` requests, mostly POST request.
18. you could retrive the victims `id` by simply 
    browsing to there profile.
19. test for functions that are meant to viewed as private 
    information.
20. you should browse to your victim profile to get 
    there paramter id that is required to get an IDOR.
21. look for state changing functionalities.
22. if you can change the email address of something, you can potentially have a account take over.
23. test for any update or change features.
24. generate UUID by uuid generators.
    it is also called GUID.
25. you can actually find IDOR even in application
    you can host locally in your computer.
26. look for API documentations and espacially find endpoints that aren't documented.
27. might wanna make three accounts.
28. temper with multiple parameters.
29. try changing the version of API.
30. use match and replace features.