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

* If there's a request body at all, the browser will send the length of the request body in the ```Content-Length``` header.

* Example of reading POST requests:

```python
from http.server import HTTPServer, BaseHTTPRequestHandler
from urllib.parse import parse_qs


class MessageHandler(BaseHTTPRequestHandler):
    def do_POST(self):
        # 1. How long was the message?
        length = int(self.headers.get('Content-length', 0))

        # 2. Read the correct amount of data from the request.
        data = self.rfile.read(length).decode()

        # 3. Extract the "message" field from the request data.
        message = parse_qs(data)["message"][0]

        # Send the "message" field back as the response.
        self.send_response(200)
        self.send_header('Content-type', 'text/plain; charset=utf-8')
        self.end_headers()
        self.wfile.write(message.encode())

if __name__ == '__main__':
    server_address = ('', 8000)
    httpd = HTTPServer(server_address, MessageHandler)
    httpd.serve_forever()
```

* Very common design pattern for interactive HTTP applications and APIs, called the __PRG__ or __Post-Redirect-Get__ pattern. A client POSTs to a server to create or update a resource; on success, the server replies not with a 200 OK but with a 303 redirect. The redirect causes the client to GET the created or updated resource.

```python
def do_POST(self):
    # How long was the message?
    length = int(self.headers.get('Content-length', 0))

    # Read and parse the message
    data = self.rfile.read(length).decode()
    message = parse_qs(data)["message"][0]

    # Escape HTML tags in the message so users can't break world+dog.
    message = message.replace("<", "&lt;")

    # Store it in memory.
    memory.append(message)

    # Send a 303 back to the root page
    self.send_response(303)  # redirect via GET
    self.send_header('Location', '/')
    self.end_headers()
```

* Python [requests](http://docs.python-requests.org/en/master/user/quickstart/) library usage:

```python
requests.get('https://api.github.com/events')
requests.post('http://httpbin.org/post', data = {'key':'value'})
requests.put('http://httpbin.org/put', data = {'key':'value'})
requests.delete('http://httpbin.org/delete')
requests.head('http://httpbin.org/get')
requests.options('http://httpbin.org/get')

r = requests.get('https://api.github.com/events')
r.text

r = requests.get('http://swapi.co/api/people/1/')
r.json()['name']
```

* ```r.content``` is a bytes object representing the literal binary data that the server sent. ```r.text``` is the same data but interpreted as a str object, a Unicode string.

* If the ```requests.get``` call can reach an HTTP server at all, it will give you a ```Response``` object. If the request failed, the ```Response``` object has a ```status_code``` data member — either 200, or 404, or some other code. But if it wasn't able to get to an HTTP server, for instance because the site doesn't exist, then ```requests.get``` will raise an exception.

## HTTP in the Real World
