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

* Examples of URI: ```https://en.wikipedia.org/wiki/Fish#Element```, ```https://en.wikipedia.org/wiki/Fish?Query=Test```

   * _https_ is the scheme;
   * _en.wikipedia.org_ is the hostname;
   * _/wiki/Fish_ is the path;
   * _Element_ is the fragment;
   * _?Query=Test_ is the query.

* [Uniform Resource Identifier (URI) Schemes](http://www.iana.org/assignments/uri-schemes/uri-schemes.xhtml) e.g.: https, http, file etc.

* Relative URI reference - URIs which doesn't have a scheme, or a hostname — just a path. 

* [Uniform_Resource_Identifier Syntax](https://en.wikipedia.org/wiki/Uniform_Resource_Identifier#Syntax)

## The Web from Python
## HTTP in the Real World
