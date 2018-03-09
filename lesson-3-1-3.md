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
   * [jQuery.ajax()](http://api.jquery.com/jQuery.ajax/)
   * [window.fetch polyfill](https://github.com/github/fetch)
   * [Can I use Fetch](https://caniuse.com/#feat=fetch)
   * [Fetch API](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API)
   * [Using Fetch](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API/Using_Fetch)
   * [WindowOrWorkerGlobalScope.fetch()](https://developer.mozilla.org/en-US/docs/Web/API/WindowOrWorkerGlobalScope/fetch)
   * [Headers](https://developer.mozilla.org/en-US/docs/Web/API/Headers)
   * [fetch API](https://davidwalsh.name/fetch)


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

#### Recap

* To Send An Async Request
   1. create an XHR object with the XMLHttpRequest constructor function
   1. use the ``.open()`` method - set the HTTP method and the URL of the resource to be fetched
   1. set the ``.onload`` property - set this to a function that will run upon a successful fetch
   1. set the ``.onerror`` property - set this to a function that will run when an error occurs
   1. use the ``.send()`` method - send the request


* To Use The Response
   1. use the ``.responseText`` property - holds the text of the async request's response   


### Using AJAX with jQuery

#### ``ajax`` method
```JavaScript
$.ajax(<url-to-fetch>, <a-configuration-object>);

// or

$.ajax(<just a configuration object>);
```

#### Handling response

```JavaScript
function handleResponse(data) {
    console.log('the ajax request has finished!');
    console.log(data);
}

$.ajax({
    url: `https://api.unsplash.com/search/photos?page=1&query=${searchedForText}`
}).done(handleResponse);;
```

#### Request with Authorization Header

```JavaScript
$.ajax({
    url: 'https://api.unsplash.com/search/photos',
    headers: { Authorization: 'Client-ID 123acd456efg' }
}).done(handleResponse);;
```

#### Recap

  1. We do not need to create an XHR object.
  1. Instead of specifying that the request is a GET request, it defaults to that and we just provide the URL of the resource we're requesting.
  1. Instead of setting onload, we use the .done() method.
  1. Parameter is being converted from JSON to a JavaScript object, so JSON.parse() is no longer needed.
  1. jQuery's ajax method does a lot of things under the hood:
      1. creates a new XHR object each time it's called
      1. sets all of the XHR properties and methods
      1. sends the XHR request
  1. jQuery has a number of other methods that can be used to make asynchronous calls. Each one of these functions in turn calls jQuery's main .ajax() method:
      1. .get()
      1. .getJSON()
      1. .getScript()
      1. .post()
      1. .load()    

### Using AJAX with Fetch

* Fetch is promise-based.

* Fetch requests still need to obey the cross-origin protocol of how resources are shared. This means that, by default, you can only make requests for assets and data on the same domain as the site that will end up loading the data.

* The Fetch request takes the URL to the requested resource as the first argument, but the second argument is a configuration object. One of the options to this config object is a headers property.

* GET HTTP method is used for a Fetch request.

* Response object is new with the Fetch API and is what's returned when a Fetch request resolves.

#### Fetch requesting

```JavaScript
fetch('<URL-to-the-resource-that-is-being-requested>');

fetch(`https://api.unsplash.com/search/photos?page=1&query=${searchedForText}`, {
    method: 'POST'
});
```

#### Fetch handling response

```JavaScript
fetch(`https://api.unsplash.com/search/photos?page=1&query=${searchedForText}`, {
    headers: {
        Authorization: 'Client-ID abc123'
    }
}).then(function(response) {
    debugger; // work with the returned response
});

fetch(`https://api.unsplash.com/search/photos?page=1&query=${searchedForText}`, {
    headers: {
        Authorization: 'Client-ID abc123'
    }
}).then(function(response) {
    return response.json();
});

fetch(`https://api.unsplash.com/search/photos?page=1&query=${searchedForText}`, {
    headers: {
        Authorization: 'Client-ID abc123'
    }
}).then(function(response) {
    return response.json();
}).then(addImage);

function addImage(data) {
    debugger;
}
```

#### Arrow function with Fetch_API

```JavaScript
// without the arrow function
}).then(function(response) {
    return response.json();
})

// using the arrow function
}).then(response => response.json())

// Fetch request with arrow function
fetch(`https://api.unsplash.com/search/photos?page=1&query=${searchedForText}`, {
    headers: {
        Authorization: 'Client-ID abc123'
    }
}).then(response => response.json())
.then(addImage);

function addImage(data) {
    debugger;
}
```

#### Handling an handleError

```JavaScript
fetch(`https://api.unsplash.com/search/photos?page=1&query=${searchedForText}`, {
    headers: {
        Authorization: 'Client-ID abc123'
    }
}).then(response => response.json())
.then(addImage)
.catch(e => requestError(e, 'image'));

function addImage(data) {
    debugger;
}

function requestError(e, part) {
    console.log(e);
    responseContainer.insertAdjacentHTML('beforeend', `<p class="network-warning">Oh no! There was an error making a request for the ${part}.</p>`);
}
```
