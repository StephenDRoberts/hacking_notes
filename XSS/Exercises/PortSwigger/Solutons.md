# PortSwigger XSS Solutions

#### [Reflected XSS into HTML context with all tags blocked except custom ones](https://portswigger.net/web-security/cross-site-scripting/contexts/lab-html-context-with-all-standard-tags-blocked)

*Solution:*
Custom tags were allowed through, as could be seen from a Burp Intruder scan.
Add a custom tag (eg) "<xss id=x onfocus=alert(1) tabindex=1>#x>"

This inserts a custom tag into the DOM with an index of "x".
Giving it a tabindex of 1 (I think??!) makes it focusable.
We then have a call to "#x" which in effect tabs the DOM to the HTML element with and id of "x".

#### [Reflected XSS with some SVG markup allowed](https://portswigger.net/web-security/cross-site-scripting/contexts/lab-some-svg-markup-allowed)

*Solution:*
Again we can set up Burp Intruder to see what tags are allowed through.
We see that the **<discard>** tag is allowed through. Then we set up Intruder on the events that are allowed passed.
This shows an **onbegin** attribute. This is where we'll set out payload.

#### [Reflected XSS into attribute with angle brackets HTML-encoded](https://portswigger.net/web-security/cross-site-scripting/contexts/lab-attribute-angle-brackets-html-encoded)

*Solution:*
Putting in a search term we can see that our entry is reflected in the <input> element under the "value" attribute.

**" onfocus=alert(1) autofocus x="**

To get the XSS to fire, we need to close off the value attribute with speechmarks.
We then set out alert under the onfocus attribute of the input tag.
Then we set the input tag to autofocus.
Lastly we set **x="** to gracefully end the markup (as another speechmark will be appeneded on execution).


