# Lesson 3.1 - 3.3 - AJAX with XHR, jQuery and Fetch


* Abbreviations:
   * __AJAX__ - _Asynchronous Javascript and XML_
   * __XHR__ - _XMLHttpRequest_
   * __API__ - _Application Programming Interface_
   * __CORS__ - _Cross-Origin Resource Sharing_


* XMLHttpRequest can be used to retrieve any type of data, not just XML, and it supports protocols other than HTTP (including file and ftp).


* The XHR's ``.open()`` method does not actually send the request! It sets the stage and gives the object the info it will need when the request is actually sent.


* Passing ``false`` as the third option makes the XHR request become a synchronous one. This will cause the JavaScript engine to pause and wait until the request is returned before continuing - this "pause and wait" is also called "blocking". This is a terrible idea and completely defeats the purpose for having an asynchronous behavior. Make sure you never set your XHR objects this way! Instead, either pass ``true`` as the 3rd argument or leave it blank (which makes it default to ``true``).'


* XHR method to add a header to the request is ``.setRequestHeader()``.


* Articles and links:
   * [Ajax: A New Approach to Web Applications (2005 year)](https://web.archive.org/web/20080702075113/http://www.adaptivepath.com/ideas/essays/archives/000385.php)
   * [XMLHttpRequest - MDN](https://developer.mozilla.org/en-US/docs/Web/API/XMLHttpRequest)
   * [XMLHttpRequest - Standard ](https://xhr.spec.whatwg.org/)
   * [XMLHttpRequest - W3](https://www.w3.org/TR/XMLHttpRequest/)
   * [XMLHttpRequestEventTarget.onload](https://developer.mozilla.org/en-US/docs/Web/API/XMLHttpRequestEventTarget/onload)
   * [Google's APIs](https://developers.google.com/apis-explorer/)
   * [Search the Largest API Directory on the Web](https://www.programmableweb.com/apis/directory)
   * [Unsplash - Beautiful, free photos. Gifted by the world’s most generous community of photographers.](https://unsplash.com/)
   * [Same-origin policy](https://developer.mozilla.org/en-US/docs/Web/Security/Same-origin_policy)
   * [Cross-Origin Resource Sharing (CORS)](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS)
   * [New Tricks in XMLHttpRequest2](https://www.html5rocks.com/en/tutorials/file/xhr2/)


### Using AJAX with XHR


```Javascript
function handleSuccess () {
    // in the function, the `this` value is the XHR object
    // this.responseText holds the response from the server
    console.log( this.responseText );
}

function handleError () {
    // in the function, the `this` value is the XHR object
    console.log( 'An error occurred' );
}

function handleSuccessAsJSON () {
    // convert data from JSON to a JavaScript object
    const data = JSON.parse( this.responseText );
    console.log( data );
}

const asyncRequestObject = new XMLHttpRequest();

asyncRequestObject.onload = handleSuccess;
asyncRequestObject.onerror = handleError;

asyncRequestObject.open('GET', 'https://unsplash.com');
asyncRequestObject.send();

asyncRequestObject.open('GET', 'https://unsplash.com');
asyncRequestObject.setRequestHeader('Authorization', 'Client-ID ***');
asyncRequestObject.send();
```

### Recap

* To Send An Async Request
   1. create an XHR object with the XMLHttpRequest constructor function
   1. use the ``.open()`` method - set the HTTP method and the URL of the resource to be fetched
   1. set the ``.onload`` property - set this to a function that will run upon a successful fetch
   1. set the ``.onerror`` property - set this to a function that will run when an error occurs
   1. use the ``.send()`` method - send the request


* To Use The Response
   1. use the ``.responseText`` property - holds the text of the async request's response   
