# HTTP & Web Servers

## Requests & Responses

* Running simple web server using __ncat__ (part of __nmap__):

```
ncat -l 9999 
```

* Running simple web client using __ncat__:

```
ncat localhost 9999
```

* Running simple web server using Python:

```
python3 -m http.server 8000
```

* __URI__ - Uniform Resource Identifier

* __URL__ - Uniform Resource Locat

* URL is a URI for a resource on the network

* Examples of URI: ```https://en.wikipedia.org:8080/wiki/Fish#Element```, ```https://en.wikipedia.org/wiki/Fish?Query=Test```

   * _https_ is the scheme;
   * _en.wikipedia.org_ is the hostname;
   * _8080_ is the port;
   * _/wiki/Fish_ is the path;
   * _Element_ is the fragment;
   * _?Query=Test_ is the query.

* [Uniform Resource Identifier (URI) Schemes](http://www.iana.org/assignments/uri-schemes/uri-schemes.xhtml) e.g.: https, http, file etc.

* Relative URI reference - URIs which doesn't have a scheme, or a hostname — just a path. 

* [Uniform Resource Identifier Syntax](https://en.wikipedia.org/wiki/Uniform_Resource_Identifier#Syntax)

* Example of logs from web server: ```127.0.0.1 - - [03/Oct/2016 15:45:50] "GET /readme.png HTTP/1.1" 200 -```

   * _GET_ is the method or HTTP verb being used;
   * _/readme.png_ is the path of the resource being requested;
   * _HTTP/1.1_ is the protocol of the request.
   
* Send a request by hand:

```
ncat 127.0.0.1 8000 

GET / HTTP/1.1
Host: localhost
```

If your server works, you'll see a status line that says HTTP/1.0 200 OK, then several lines of headers including the date as well as some other information, and a piece of HTML code. These parts make up the HTTP response that the server sends.

* Example of status line:

```
HTTP/1.0 200 OK
HTTP/1.1 301 Moved Permanently
```

* HTTP status codes:

   * __1xx__ — Informational. The request is in progress or there's another step to take.
   * __2xx__ — Success! The request succeeded. The server is sending the data the client asked for.
   * __3xx__ — Redirection. The server is telling the client a different URI it should redirect to. The headers will usually contain a Location header with the updated URI. Different codes tell the client whether a redirect is permanent or temporary.
   * __4xx__ — Client error. The server didn't understand the client's request, or can't or won't fill it. Different codes tell the client whether it was a bad URI, a permissions problem, or another sort of error.
   * __5xx__ — Server error. Something went wrong on the server side.

* [List of HTTP status codes](https://en.wikipedia.org/wiki/List_of_HTTP_status_codes)

* [HTTP Status Code Definitions](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html)

* Example of HTTP header:

```
Content-type: text/html; charset=utf-8
```

## The Web from Python
## HTTP in the Real World
