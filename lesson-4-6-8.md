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

* Steps to deployment using __Heroku__:
   1. Check your server code into a new local Git repository.
   2. Sign up for a free Heroku account.
   3. [Heroku CLI](https://devcenter.heroku.com/articles/heroku-cli)
   4. Authenticate the Heroku CLI with your account: ```heroku login```
   5. Create configuration files ```Procfile```, ```requirements.txt```, and ```runtime.txt``` and check them into your Git repository.
   6. Modify your server to listen on a configurable port.
   7. Create your Heroku app: ```heroku create your-app-name```
   8. Push your code to Heroku with Git: ```git push heroku master```


* ```runtime.txt``` tells Heroku what version of Python you want to run. Check the currently supported runtimes in the Heroku documentation; this will change over time! As of early 2017, the currently supported version of Python 3 is python-3.6.0; so this file just needs to contain the text python-3.6.0.

* ```requirements.txt``` is used by Heroku (through pip) to install dependencies of your application that aren't in the Python standard library. The bookmark server has one of these: the requests module. We'd like a recent version of that, so this file can contain the text requests>=2.12. This will install version 2.12 or a later version, if one has been released.

* ```Procfile``` is used by Heroku to specify the command line for running your application. It can support running multiple servers, but in this case we're only going to run a web server. Check the Heroku documentation about process types for more details. If your bookmark server is in BookmarkServer.py, then the contents of Procfile should be web: python BookmarkServer.py.   

* Python code can access environment variables in the ```os.environ``` dictionary. To access ```os.environ```, you will also need to ```import os``` at the top of the file:

```python
if __name__ == '__main__':
    port = int(os.environ.get('PORT', 8000))   # Use PORT if it's there.
    server_address = ('', port)
    httpd = http.server.HTTPServer(server_address, Shortener)
    httpd.serve_forever()
```

* Built-in ```http.server.HTTPServer``` class can only handle a single request at once.

* HTTPServer that supports thread-based concurrency:

```python
import threading
from socketserver import ThreadingMixIn

class ThreadHTTPServer(ThreadingMixIn, http.server.HTTPServer):
    "This is an HTTPServer that supports thread-based concurrency."

if __name__ == '__main__':
    port = int(os.environ.get('PORT', 8000))
    server_address = ('', port)
    httpd = ThreadHTTPServer(server_address, Shortener)
    httpd.serve_forever()    
```

* Specialized web server programs — like _Apache_, _Nginx_, or _IIS_ — can serve static content from disk storage very quickly and efficiently. They can also provide access control, allowing only authenticated users to download particular static content.

* __Cookies__ are a way that a server can ask a browser to retain a piece of information, and send it back to the server when the browser makes subsequent requests. Every cookie has a __name__ and a __value__:

```python
from http.cookies import SimpleCookie, CookieError

out_cookie = SimpleCookie()
out_cookie["bearname"] = "Smokey Bear"
out_cookie["bearname"]["max-age"] = 600
out_cookie["bearname"]["httponly"] = True
```

* Send the cookie as a header from your request handler:

```python
self.send_header("Set-Cookie", out_cookie["bearname"].OutputString())
```

* To read incoming cookies, create a SimpleCookie from the Cookie header:

```python
in_cookie = SimpleCookie(self.headers["Cookie"])
in_data = in_cookie["bearname"].value
```

* __TLS__ - Transport Layer Security provides some important guarantees for web security:

    * It keeps the connection __private__ by encrypting everything sent over it. Only the server and browser should be able to read what's being sent.
    * It lets the browser __authenticate__ the server. For instance, when a user accesses https://www.udacity.com/, they can be sure that the response they're seeing is really from Udacity's servers and not from an impostor.
    * It helps protect the __integrity__ of the data sent over that connection — checking that it has not been (accidentally or deliberately) modified or replaced.

* TLS is also very often referred to by the older name __SSL__ (Secure Sockets Layer). Technically, SSL is an older version of the encryption protocol. This course will talk about TLS because that's the current standard.

* TLS includes two important pieces of data: a __private key__ and a __public certificate__.

* The server's certificate is issued by an organization called a __certificate authority (CA)__.

* [Public-key cryptography](https://en.wikipedia.org/wiki/Public-key_cryptography)

* Every request and response sent over a TLS connection is sent with a __message authentication code (MAC)__ that the other end of the connection can verify to make sure that the message hasn't been altered or damaged in transit.

* All of the other HTTP methods:

   * PUT for creating resources
   * DELETE for deleting things
   * PATCH for making changes
   * HEAD, OPTIONS, TRACE for debugging

* HTTP/2 - features:

   * HTTP/2 should load much faster than HTTP/1, if your browser is using it!
   * HTTP2 is multiplexing requests and responses over a single connection. The browser can send several requests all at once, and the server can send responses as quickly as it can get to them. There's no limit on how many can be in flight at once.
   * HTTP/2 has a feature called server push which allows the server to say, effectively, "If you're asking for index.html, I know you're going to ask for style.css too, so I'm going to send it along as well."
   * Early drafts of HTTP/2 proposed that encryption should be required for sites to use the new protocol. This ended up being removed from the official standard … but most of the browsers did it anyway! Chrome, Firefox, and other browsers will only attempt HTTP/2 with a site that is using TLS encryption.

* [HTTP Spy](https://chrome.google.com/webstore/detail/http-spy/agnoocojkneiphkobpcfoaenhpjnmifb?hl=en) is a neat little Chrome extension that will show you the headers and request information for every request your browser makes.

* [HTTP/2 standard](https://http2.github.io/)

* [Let’s Encrypt](https://letsencrypt.org/) is a free, automated, and open Certificate Authority.
