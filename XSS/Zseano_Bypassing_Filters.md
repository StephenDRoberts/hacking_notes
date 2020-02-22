# XSS

Most commonly found bug.

[zseano: LiveHacker Mentoring: Understanding & Bypassing filters with @zseano](https://www.youtube.com/watch?v=jtFlbUUKL1Y)

Goal is to determine what is being filtered:
1. Are the basic "'<> allowed? What if you try them by themselves - differnet behaviour?
2. Is "on[]=" filtered or is it just the common 'onerror' etc? what about "onxxx"?
(you're testing whether they're just running through a blacklist of filters). Also try removing the equals sign
3. How to they handle unicode, double encoding, unusual encodings?

Once you find a filter bypass, it's usually common throughout.

If you try a special character on its own and it's encoded, then its probably safe from XSS.

You may be able to smuggle an XSS payload throough a parameter to get passed Akami WAF (around 17:30)

NB : "<sciprT/x>" is a valid script tag!

For blind XSS, try XSSHunter. For blind XSS you want to hit an admin panel, so consider what would be shown there (eg email, username, etc)
Always test your referrer field - shows where the users have come from. Could be blind XSS executed.

#### What about filters in general? (31mins)

Goal is to determine what is being filtered:
eg If they're checking the Origin: header, think how does it work?
eg website might think if origin isnt set - dont do anything
or: if it needs to be set to do something, you can set a null origin using base64 encoding

Most foilters are If/Else statements & regex

#### Tools for finding XSS
* Burp Repeater
* Burp Match & Replace
* Burp Intruder
* Input Scanner (by zseano) with a list of various js attributes & html tags
Mostly use manual testing

Use automation when you find something and think you might want to scan the whole site for that issue.
eg scan for lots of endpoints and try that winner on all of them for more reports.

NB (41mins) CSRF protection may be in place where it checks if the Origin is set to a whitelister set of domains
and if it isnt, don't allow it. This could be bypassed by changing the request from a POST to a GET.
This could lead you to think what else isnt checked between POSTS/GETS.

(51mins, includes examples over next few minutes)
If you tried "<script>" and it didnt work but then try "<script" and that worked, then you know the developer is filtering for
a fully formed script tag.

If everything is encoded then it just might not be vulnerable

Look for interesting endpoints. Try all the features to test.

Try inputting Hex encoded payloads - does it do anything with them or are they simply reflected back?

Try svg tag instead of img src. Try "onbeforescriptexecute"

Test for XSS everywhere then look/search for it later.
