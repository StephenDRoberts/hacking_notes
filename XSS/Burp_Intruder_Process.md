# Burp Intruder Process

*Taken from [portswigger XSS solution](https://portswigger.net/web-security/cross-site-scripting/contexts/lab-html-context-with-most-tags-and-attributes-blocked)*

* Use search functionality and send to Burp Intruder
* In Burp Intruder, in the Positions tab, click "Clear §"
* In the request template, replace the value of the search term with <>
* Place the cursor between the angle brackets and click "Add §" twice, to create 
a payload position. Teh value of the search term should now look like <§§>
* Visit the [XSS cheat sheet](https://portswigger.net/web-security/cross-site-scripting/cheat-sheet) and click "copy tags to clipboard"
* In Burp Intruder, in the Payloads tab, click "Paste" to paste the list of tags into the payloads list
* Click "Start Attack"
* Review the results for what came back with a 200 code

