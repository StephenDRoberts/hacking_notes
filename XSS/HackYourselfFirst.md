[Hack Yourself First - How to go on the Cyber-Offense](https://app.pluralsight.com/library/courses/hack-yourself-first/table-of-contents)

### Understanding untrusted data and sanitisation

Untrusted data:
* The integrity is not verifiable
* The intent may be malicious
* The data may include payloads such as:
    * SQL injection
    * Cross site scripting
    * Binaries containing malware

Common sources:
* From the user:
    * in the url via a query string or route
    * posted via a form
* From the browser:
    * in cookies
    * in the request headers (eg in the user agent)
* From any number of other locations
    * external services
    * your own database

Characters requiring sanitisation:
* <>'/"`;

Explicity rejecting specfic characters is called a blacklist approach.
Explicity accepting only approved characters is called a whitelist.
Blacklits may not cover everything you actually should - what if a new attack vector is discovered? Whitelists explicitly say "This is what we know is good so we're only going to allow these".

### Establishing input sanitisation practices

We want to find gaps in the sanitisation process.  
If your search term is shown in the URL then it's being sent to the server as part of a GET request rather than a POST. The easiest way to perform reflective XSS is throught the URL parameters.

Try inputting special characters into your search box to see which characters are being sanitised (eg a "/" may turn into %2F as forward slashes are sanitised). Sometimes maybe only a "<" character is sanitised, which wouldn't be accurate in saefty.

### Understanding XSS and output encoding

#### Reflective

* User makes a HTTP request with untrusted data
* Server responds and untrusted data is reflected (this is common in search features)
* Here the untrusted data is being put into the markup

#### Identification:

Request: `www.mysite.com/Search?q=ferrari`
Response: markup with: `You searched for <strong>ferrari</strong`

#### Exploitation:

Request: `www.mysite.com/Search?q=ferrari<i>enzo</i>`
Response: `You searched for <strong>ferrari<i>enzo</i></strong>`

Here we're able to break out of the *data* context and enter the *markup* context. We could possibly close out the </strong> tag, then break out of the paragraph, and enter our own data in. Or we could inject our own script tag in.

What makes this risk possible is a lack of **output encoding**.

#### Output Encoding

XSS attacks are possible because the app allows an XS payload to break out of the data context and change the markup context. To mitigate the risk of XSS, we want to make sure the serach term appears on the screen *exactly* as it was entered.

So how do we write markup to display "<i>enzo</i>" on the screen?

**Output Encoding**. Using HTML encodig we would get:

`&lt;i&gt;enzo&ltl/i&gt;`

this means that the data can now not break out of the data context and intor the markup context!

The way data needs to be encoded changes depending on the ***context***:

Context | Encoding
--- | ---
HTML | `&lt;i&gt;enzo&ltl/i&gt;`
CSS | `<\i\>enzo\</i\>
JavaScript | '\x3ci\ix3eeenzo\x3c\x2fi\x3e'
URL | \00003Ci\00003Eenzo\00003C\00002Fi\00003E
LDAP distinguished name | \<i\>enzo\</i\> 

Also differnetfor HTML attribute, HTML form URL, LDAP filter, RL path, XML, XML attribute. 

We're basically just looking for when encoding hasn't been done properly. Maybe data has been HTML encoded when it should have been Javascript encoded.  You may see developers writing their own encoding systems. This is dangerous! We should use a trusted library of a native application for encoding that has experience in encoding untrusted data.

### Identifying the use of output encoding

When you enter a payload you may want to use the developer tools to see where it's being reflected. This may show that certain characters are actually just being stripped out without any form of encoding. 

The dev tools may also show characters in their natural form. However, if you were to look at the ***view page source*** section, those characters may actually be encoded. This is a discrepancy with the dev tools compared to actual source code. The dev tools are good for showing you what is rendered on page.

(Eg) The dev tools may show `'` however the actual source code shows `&#47` instead, so it's being properly encoded (this is HTML encoding for a forward slash).

When you're looking for risks on a page, be wary of this discrepancy. The data in the HTML ***source*** is a far truer representation of what is being returned by the server.

You want to see where data is being returned by the server -> Input a normal search term and look for it in the source code. This can show that it's being brought into both the HTML and the JavaScript (for instance). This then begs the question:

> ***Is the output encoding being correctly aplied in both locations?***

For instance, HTML may be properly encoded, but we may not see any encoding in the JavaScript when we view the source. In this case, we may want to break out of the JavaScript context and inject our own code in there.

### Delivering a payload via reflected XSS

In this example we are able to pass in untrusted data non-encoded into the JavaScript context:

> `$('#serchTerm').val('enzo/>');`

### Testing fo rthe risk of persistent XSS

### The X-XSS-Protection header
