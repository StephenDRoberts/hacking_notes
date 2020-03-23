[What is XSS - Lesson 1](https://gracefulsecurity.com/xss-what-is-cross-site-scripting-lesson-1-basics/)  

## Defending against XSS

The vast majority of XSS can be fixed in a relatively simple way, by ecoding all dangerous characters that originated as user input using *HTML entity encoding*. The benfit of HTML entities is that the browser will show the correct character to the user but it'll prevent an attacker from injection scipts. 

[Contexts](https://gracefulsecurity.com/xss-cross-site-scripting-lesson-2-contexts/)

## Contexts

### Within the page body

(eg) `<p>Hello <script>alert(1)</script><script>document.write('Holly!)</script></p>`

### Within an HTML tag

(eg) `<div id="" onmouseover=alert(1) ">`

### Within an HREF attribute

(eg) `<a href="javascript:alert(1)">`

### Within an existing script

 (eg) `<p>Hello <script>document.write('');alert(1);//!')</script></p>`

 All dangerous characters should be encoded to prevent attacks. this should include:

 `< > " ' / ; ( )`

 [An Introduction to DOM-XSS](https://gracefulsecurity.com/an-introduction-to-dom-xss/)

 DOM XSS is a type of XSS where instead of payloads being stored or reflected by the remote web server and appearing inthe response HTML, the payload is instead stored in the DOM and processed insecurely by Javascript.
 
 If you're using Burp Suite, it doesn't parse or execute Javascript and therefore it won't be too much help. It will however look for DOM-XSS through static analysis and pick up on isses such as  location.hash ending up in document.write.

 This is the idea of 'sinks' and 'sources', where a vulnerabiity may occur if an attacker is able to control a source and the data retrieved makes it into a sink without filtering, validation or encoding.

 Firefox can show the DOM-Source by highlighting an area of the page and selecting 'View Selection Source'. 
