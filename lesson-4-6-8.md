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

* Run a web service in Python:
   * Import ```http.server```.
   * Create a subclass of ```http.server.BaseHTTPRequestHandler``` as a handler class.
   * Define a method on the handler class for each HTTP verb you want to handle.
      * The method for GET requests has to be called ```do_GET```.
      * Inside the method, call built-in methods of the handler class to read the HTTP request and write the response.
   * Create an instance of ```http.server.HTTPServer```, giving it your handler class and server information (e.g. the port number).
   * Call the ```HTTPServer``` instance's ```run_forever``` method.
   
```python
from http.server import HTTPServer, BaseHTTPRequestHandler


class HelloHandler(BaseHTTPRequestHandler):
    def do_GET(self):
        # First, send a 200 OK response.
        self.send_response(200)

        # Then send headers.
        self.send_header('Content-type', 'text/plain; charset=utf-8')
        self.end_headers()

        # Now, write the response body.
        self.wfile.write("Hello, HTTP!\n".encode())

if __name__ == '__main__':
    server_address = ('', 8000)  # Serve on all addresses, port 8000.
    httpd = HTTPServer(server_address, HelloHandler)
    httpd.serve_forever()
```

* [Exercise code for the Udacity course "HTTP and Web Servers" written in portable Python 3 and HTML.](https://github.com/udacity/course-ud303)

* ```.encode()``` - if you want to send a string over the HTTP connection, you have to encode the string into a bytes object. The encode method on strings translates the string into a bytes object, which is suitable for sending over the network. 

* [UTF-8](https://en.wikipedia.org/wiki/UTF-8) - the most common encoding, which is supported by all major and minor browsers and operating systems, and it supports characters for almost all the world's languages. In UTF-8, a single character may be represented as anywhere from one to four bytes, depending on language. UTF-8 is the default encoding in Python. 

* In ```BaseHTTPRequestHandler``` parent class in http.server ```path``` is instance variable that contains the request path.

* Fragments aren't sent to the server as part of an HTTP GET request e.g. ```stardust``` in ```http://localhost:8000/spiders_from_mars#stardust```.

* Simple echo server, which always writes the same message taken from the request path:

```python
self.wfile.write(self.path[1:].encode())
```

what is the same as:

```python
message = self.path[1:]  # Extract 'bears' from '/bears', for instance
message_bytes = message.encode()  # Make bytes from the string
self.wfile.write(message_bytes)  # Send it over the network
```

* __Query__ - part of the URI after the ? mark. Conventionally, query parameters are written as ```key=value``` and separated by ```&``` signs. 

* [urllib.parse](https://docs.python.org/3/library/urllib.parse.html) - Python library that knows how to unpack query parameters and other parts of an HTTP URL. 

```python
>>> from urllib.parse import urlparse, parse_qs
>>> address = 'https://www.google.com/search?q=gray+squirrel&tbm=isch'
>>> parts = urlparse(address)
>>> print(parts)
ParseResult(scheme='https', netloc='www.google.com', path='/search', params='', query='q=gray+squirrel&tbm=isch', fragment='')
>>> print(parts.query)
q=gray+squirrel&tbm=isch
>>> query = parse_qs(parts.query)
>>> query
{'q': ['gray squirrel'], 'tbm': ['isch']}
```

* HTTP URLs aren't allowed to contain spaces or certain other characters. So if you want to send these characters in an HTTP request, they have to be translated into a __URL-safe__ or __URL-quoted__ format. __Quoting__ in this sense doesn't have to do with quotation marks. It means translating a string into a form that doesn't have any special characters in it, but in a way that can be reversed (unquoted) later. And if that isn't confusing enough, it's sometimes also referred to as URL-encoding or URL-escaping. One of the features of the URL-quoted format is that spaces are sometimes translated into plus signs. Other special characters are translated into hexadecimal codes that begin with the percent sign.

* ```HTTP GET``` methods are good for search forms and other actions that are intended to look something up or ask the server for a copy of some resource. But ```GET``` is not recommended for actions that are intended to alter or create a resource. For this sort of action, HTTP has a different verb, ```POST```.

* __idempotent__ - an action is idempotent if doing it twice (or more) produces the same result as doing it once. ```POST``` requests are not idempotent.

* When a browser submits a form as a ```POST``` request, it doesn't encode the form data in the URI path, the way it does with a ```GET``` request. Instead, it sends the form data in the request body, underneath the headers. The request also includes ```Content-Type``` and ```Content-Length``` headers, which we've previously only seen on HTTP responses.

* The names of HTTP headers are case-insensitive. So there's no difference between writing ```Content-Length``` or ```content-length``` or even ```ConTent-LeNgTh```.

## HTTP in the Real World
