# [General Info & the Fundamentals of Web apps](https://fullstackopen.com/en/part0)


Always keep the Developer Console open and stick to Chrome or Firefox's Developer Edition.

In the Network tab, check `Disable cache` and `Preserve log` (saves logs printed by app when the page is loaded).

> "The mechanism of invoking event handlers is very common in JavaScript. Event handler functions are called callback functions. The application code does not invoke the functions itself, but the runtime environment - the browser, invokes the function at an appropriate time, when the event has occurred."

> "Document Object Model, or DOM is an Application Programming Interface, (an API), which enables programmatic modification of the element trees corresponding to web-pages."


https://developer.mozilla.org/en-US/docs/Web/HTTP/Methods/POST


> AJAX (Asynchronous JavaScript and XML) is a term introduced in February 2005 on the back of advancements in browser technology to describe a new revolutionary approach that enabled the fetching of content to web pages using JavaScript included within the HTML, without the need to rerender the page.

> The thing termed AJAX is now so commonplace that it's taken for granted. The term has faded into oblivion, and the new generation has not even heard of it.

[A brief history of JavaScript libraries](https://fullstackopen.com/en/part0/fundamentals_of_web_apps#java-script-libraries)


Forms:

A form's "action attribute defines the location (URL) where the form's collected data should be sent when it is submitted." —[MDN](https://developer.mozilla.org/en-US/docs/Learn/Forms/Your_first_form)

## Exercises:

### 0.4: new note
> **Create a similar diagram** depicting the situation where the user creates a new note on page [https://studies.cs.helsinki.fi/exampleapp/notes](https://studies.cs.helsinki.fi/exampleapp/notes) by writing something into the text field and clicking the _submit_ button. 
> &nbsp;
> If necessary, show operations on the browser or on the server as comments on the diagram.
> &nbsp;
> The diagram does not have to be a sequence diagram. Any sensible way of presenting the events is fine.

<img src="https://www.websequencediagrams.com/cgi-bin/cdraw?lz=bm90ZSBvdmVyIGJyb3dzZXI6ClVzZXIgd3JpdGVzIHNvbWV0aGluZyBpbnRvIHRoZSB0ZXh0IGZpZWxkCmFuZCBjbGlja3MAFQVzdWJtaXQgYnV0dG9uCmVuZCBub3RlCgBSBy0-c2VydmVyOiBIVFRQIFBPU1QgaHR0cHM6Ly9zdHVkaWVzLmNzLmhlbHNpbmtpLmZpL2V4YW1wbGVhcHAvbmV3XF8ASgUAQAYtLT4AgSkIIEhUTUwtY29kAFMYR0UAPixtYWluLmNzcwBWEwASCQAfSWoAThlqcwoKAIMKEwCDHwcgc3RhcnRzIGV4ZWN1dGluZyBqcwCBfQZ0aGF0IHJlcXVlc3RzIEpTT04gZGF0YSBmcm9tIACCfAYgAIMSCgCBbUVkYXRhLmpzb24AgSYLAINlBwCDJwcgc2VuZACBAgxiYWNrIHRoYXQKaW5jbHVkZQCEPAYAhFcFdGhlIHUAgV8FAIRLBXRlZACEQgoAg2sSW3sgY29udGVudDogIkhUTUwgaXMgZWFzeSIsIGRhdGU6ICIyMDE5LTA1LTIzIiB9LCAuLi5dAII_HQCCTwYAgQwHZXZlbnQgaGFuZGxlcgCCVghuZGVycwCFWQVzIHRvIGRpc3BsYXkAhWoJ&s=default" alt="diagramed answer for creating a new note">

note over browser:
User writes something into the text field
and clicks the submit button
end note
browser->server: HTTP POST https://studies.cs.helsinki.fi/exampleapp/new\_note
server-->browser: HTML-code
browser->server: HTTP GET https://studies.cs.helsinki.fi/exampleapp/main.css
server-->browser: main.css
browser->server: HTTP GET https://studies.cs.helsinki.fi/exampleapp/main.js
server-->browser: main.js

note over browser:
browser starts executing js-code
that requests JSON data from server 
end note

browser->server: HTTP GET https://studies.cs.helsinki.fi/exampleapp/data.json
note over server:
server sends JSON data back that
includes the text the user submitted
end note
server-->browser: [{ content: "HTML is easy", date: "2019-05-23" }, ...]

note over browser:
browser executes the event handler
that renders notes to display
end note


### 0.5: Single page app
>   Create a diagram depicting the situation where the user goes to the [single page app](https://fullstackopen.com/en/part0/fundamentals_of_web_apps#single-page-app) version of the notes app at [https://studies.cs.helsinki.fi/exampleapp/spa](https://studies.cs.helsinki.fi/exampleapp/spa).

<img src="https://www.websequencediagrams.com/cgi-bin/cdraw?lz=dGl0bGUgVmlzaXRpbmcgU2luZ2xlIHBhZ2UgYXBwCgpicm93c2VyLT5zZXJ2ZXI6IEhUVFAgR0VUIGh0dHBzOi8vc3R1ZGllcy5jcy5oZWxzaW5raS5maS9leGFtcGxlYXBwL3NwYQoAOQYtLT4ASgc6IEhUTUwtY29kZQAfRW1haW4uY3NzAFYTABIJAIEFRy5qAFIUABIHCm5vdGUgb3ZlciAAgWIIAII8CCBzdGFydHMgZXhlY3UAgmkFanMAgXsGdGhhdCByZXF1ZXN0cyBKU09OIGRhdGEgZnJvbSAAgnMGIAplbmQgbm90ZQCCTkZkYXRhLmpzb24AgwcTW3sgY29udGVudDogIkhUTUwgaXMgZWFzeSIsIGRhdGU6ICIyMDE5LTA1LTIzIiB9LCAuLi5dAIFeHQCBbgZlcyByZWRyYXdOb3RlcygpCnJlbmRlcmluZwCBVQVzAIFrBgCBEgkgdG8gZGlzcGxheQCBdQk&s=default" alt="diagramed answer for loading the single page app">

title Visiting Single page app

browser->server: HTTP GET https://studies.cs.helsinki.fi/exampleapp/spa
server-->browser: HTML-code
browser->server: HTTP GET https://studies.cs.helsinki.fi/exampleapp/main.css
server-->browser: main.css
browser->server: HTTP GET https://studies.cs.helsinki.fi/exampleapp/spa.js
server-->browser: spa.js

note over browser:
browser starts executing js-code
that requests JSON data from server 
end note

browser->server: HTTP GET https://studies.cs.helsinki.fi/exampleapp/data.json
server-->browser: [{ content: "HTML is easy", date: "2019-05-23" }, ...]

note over browser:
browser executes redrawNotes()
rendering notes from data.json to display
end note


### 0.6: New note
> Create a diagram depicting the situation where the user creates a new note using the single page version of the app.

<img src="https://www.websequencediagrams.com/cgi-bin/cdraw?lz=dGl0bGUgU2luZ2xlIHBhZ2UgYXBwOiBOZXcgTm90ZQoKYnJvd3Nlci0-c2VydmVyOiBIVFRQIEdFVCBodHRwczovL3N0dWRpZXMuY3MuaGVsc2lua2kuZmkvZXhhbXBsZWFwcC9zcGEKADkGLS0-AEoHOiBIVE1MLWNvZGUAH0VtYWluLmNzcwBWEwASCQCBBUcuagBSFAASBwpub3RlIG92ZXIgAIFiCACCPAggc3RhcnRzIGV4ZWN1dGluZyBqcwCBewZ0aGF0IHJlcXVlc3RzIEpTT04gZGF0YSBmcm9tIACCcwYgCmVuZCBuAIJLSWRhdGEuanNvbgCDBxNbeyBjb250ZW50OiAiSFRNTCBpcyBlYXN5IiwgZGF0ZTogIjIwMTktMDUtMjMiIH0sIC4uLl0AgV4dAIFuBmVzIHJlZHJhd05vdGVzKCkKcmVuZGVyaW5nAIFVBXMAgWsGAIESCSB0byBkaXNwbGF5AIFzCwCCThN1AF4FbnRlcnMgdGV4dCBpbnRvIGZpZWxkIGFuZCBjbGlja3Mgc2F2ZQoKQW4gZXZlbnQgaGFuZGxlciBhZGRzIHRoZQCCWQUgdG8ABwVsaXN0OgCDSAVzLnB1c2gobm90ZSkKCkFuZCB0aGVuACwFADoOUE9TVABCB2V3AIMlBgBGBwCDPAd3aXRoIGEgQwCCVgYtVHlwZSBoZWFkZXIgb2YKYXBwbGljYXRpb24vanNvbjsgY2hhcnNldD11dGYtOACDXyFQT1MAhmksbmV3X25vdGVfAIcIFnN0YXR1cyBjb2RlIDIwMSBjcmVhdGVkCg&s=default" alt="diagramed answer for creating a new note in the single page app"

title Single page app: New Note

browser->server: HTTP GET https://studies.cs.helsinki.fi/exampleapp/spa
server-->browser: HTML-code
browser->server: HTTP GET https://studies.cs.helsinki.fi/exampleapp/main.css
server-->browser: main.css
browser->server: HTTP GET https://studies.cs.helsinki.fi/exampleapp/spa.js
server-->browser: spa.js

note over browser:
browser starts executing js-code
that requests JSON data from server 
end note

browser->server: HTTP GET https://studies.cs.helsinki.fi/exampleapp/data.json
server-->browser: [{ content: "HTML is easy", date: "2019-05-23" }, ...]

note over browser:
browser executes redrawNotes()
rendering notes from data.json to display
end note

note over browser:
user enters text into field and clicks save

An event handler adds the note to the list:
notes.push(note)

And then the event handler POSTs the new note
to the server with a Content-Type header of
application/json; charset=utf-8
end note

browser->server: HTTP POST https://studies.cs.helsinki.fi/exampleapp/new_note_spa
server-->browser: status code 201 created