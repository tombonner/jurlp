JQuery URL parser.
==================

JQuery URL parser plugin for parsing, manipulating, filtering and monitoring URLs in "href" and "src" attributes within arbitrary elements (including document.location.href), as well as creating anchor elements from URLs found in HTML/text.

**Version**

1.0.4

**License**

Copyright (C) 2012, Thomas James Bonner (tom.bonner@gmail.com).

**MIT License**

Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated documentation files (the "Software"), to deal in the Software without restriction, including without limitation the rights to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the Software, and to permit persons to whom the Software is furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT.  IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.

Documentation
-------------

View the full [HTML documentation](http://tombonner.github.com/jurlp/doc/index.html).

Unit test
---------

Run the [QUnit test suite](http://tombonner.github.com/jurlp/jurlp.html).

Files
-----

    / ................. Project root.
    |-jurlp.js ........ JQuery URL parser plugin source.
    |-jurlp.min.js .... jurlp.js compressed using YUI Compressor.
    |-jurlp.html ...... JQuery URL parser plugin QUnit test suite.
    |-README.md ....... This file.
    |-doc/ ............ Natural Docs output.
    |-doc/index.html .. Documentation main-page.

Requirements
------------

[JQuery](http://jquery.com/)

URL naming scheme
-----------------

A quick quide to URL nomenclature in this plugin.

Throughout this plugin, URLs are segmented and refered to in the following manner;

    http://username:password@www.example.com:443/path/file.name?query=string#anchor
    |_____||______| |______| |_____________| |_||_____________||___________||_____|
       |       |       |           |          |         |             |         |
    scheme   user   password      host       port      path         query   fragment
    |______________________________________________________________________________|
                                           |
                                          url

* **Scheme** - Contains the protocol identifier (i.e.  "https://", "ftp://").
* **User** - Conains the username to use when connecting to the host server. This segment may be empty.
* **Password** - Contains the password to use in conjunction with the username when connecting to the remote server. This segment may be empty (and cannot be set without a user name).
* **Host** - Contains the name or IP address of the host server (i.e.  "www.example.com", or "127.0.0.1").
* **Port** - Contains the listening port number for the host server (i.e.  "80", or "8080").  Note that an empty port value implies the default port (80).
* **Path** - Contains the file path (i.e.  "/index.html", or "/").
* **Query** - Contains any parameters passed in the query (i.e.  "?param1=value1&param2=value2").  This segment may be empty.
* **Fragment** - Contains any anchors/hash tags (i.e.  "#elementname").  This segment may be empty.

URL Objects
-----------

URL object definition.

For the purposes of this plugin, URLs can be represented either as a string, for example "http://www.example.com:8080/path/file.name?query=string#anchor", or as an object;

``` javascript

{
    scheme: "http://"
    user: "username",
    password: "password",
    host: "www.example.com",
    port: "8080",
    path: "/path/file.name",
    query: "?query=string",
    fragment: "#anchor"
}

```

Therefore, wherever URLs are supplied as a parameter to the plugin via the url or proxy methods, either a string or object representation or the URL may be supplied.

URL objects that have been returned via the parser interface can easily be converted to a string by calling the objects toString() method.

Example:

``` javascript

// Parse the document.location.href URL, and convert it back to a string again.
$(document).jurlp("url").toString();

```

Quick overview
--------------

Useful example code.

``` javascript

// Parse and set the element(s) URL
$("a").jurlp("url");
$("a").jurlp("url", "http://www.example.com/");

// Get or set individual URL segments for the element(s)
$("a").jurlp("scheme");
$("a").jurlp("scheme", "https://");

$("a").jurlp("host");
$("a").jurlp("host", "www.example.com");

$("a").jurlp("port");
$("a").jurlp("port", "8080");

$("a").jurlp("path");
$("a").jurlp("path", "../file.name");

$("a").jurlp("query");
$("a").jurlp("query", {"param":"value"});

$("a").jurlp("fragment");
$("a").jurlp("fragment", "elementid");

// Filter on URL segments
$("a").jurlp("filter", "scheme", "^=", "http")
      .jurlp("filter", "host", "=", "www.example.com")
      .jurlp("filter", "port", "!=", "8080")
      .jurlp("filter", "path", "$=", ".html")
      .jurlp("filter", "query", "*=", "param=value")
      .jurlp("filter", "fragment", "regex", /(\#)/);

// Watch a selector for new nodes
$("a:eq(0)").jurlp("watch", function(element, selector){})
            .jurlp("filter", "host", "=", "www.example.com")
            .jurlp("query",{"found":"example"});

$("body").prepend("<a href='http://www.example.com/'></a>");

$("a:eq(0)").jurlp("unwatch");

// Parse an element's text for URLs and create/return anchor elements
$("<div>www.example.com</div>").jurlp();

// Get an interface for parsing/manipulating the supplied URL
url = $.jurlp("http://www.example.com:80/path/file.name?param1=value1#fragment");

// Parse the URL to an object.
url.url();

// Get the URL scheme.
url.scheme();

// Get the URL user name.
url.user();

// Get the URL password.
url.password();

// Get the URL host.
url.host();

// Get the URL port.
url.port();

// Get the URL path.
url.path();

// Get the URL query.
url.query();

// Get a specific parameter value from the URL query.
url.query().param1;

// Get the URL fragment.
url.fragment();

// Set the full URL.
url.url("http://www.example.com:80/path/file.name?param1=value1#fragment");

// Set the URL scheme.
url.scheme("https://");

// Set the URL user name.
url.user("user");

// Set the URL password.
url.password("password");

// Set the URL host.
url.host("www.newexample.com");

// Set the URL port.
url.port("80");

// Set the URL path.
url.path("/newpath/newfile.file");

// Append to the URL path.
url.path("./newfile.file");

// Remove two path elements and append to the URL path.
url.path("../../newfile.file");

// Set the URL query.
url.query("?param=value");

// Append/modify the URL query (string or object)
url.query("param=value");
url.query({"param":"value"});

// Remove the URL query
url.query("");
url.query({});

// Set the URL fragment.
url.fragment("#newfragment");

```

Parsing document.location.href
------------------------------

Parsing the document URL.

The document URL (document.location.href) can be parsed by specifying the HTML document element to the parser in the following manner;

``` javascript

// Parse the document.location.href URL string into a URL object
$(document).jurlp("url");

```

Similarly, the document URL can be modified by the plugin, but it is worth noting that changes will not be directly applied to document.location.href until goto is explicitly called on the element, and instead, a working copy of the URL is stored under the documents "data-href" attribute.

``` javascript

// Does not modify document.location.href (updates $(document).data("href"))
$(document).jurlp("url", "www.example.com");

// Does modify document.location.href (from $(document).data("href"))
$(document).jurlp("goto");

```

Parsing elements with an "href" or "src" attribute.
---------------------------------------------------

Parsing "href" or "src" attributes.

Elements with an "href" or "src" attribute (i.e. `<a href="">`, `<base href="">`, `<link href="">`, `<img src="">`, `<script src="">` or `<iframe src="">`), can be parsed by specifying the element(s) to the parser in the following manner;

``` javascript

// Parse all anchor element URLs into an array
$("a").jurlp("url");

```

Any modifications made to the URL will modify the relevant "href" or "src" attribute directly.  If you want to visit the URL within an elements "href" or "src" attribute, it is possible to call goto on the element.

``` javascript

// Directly set the first anchor elements URL, and then goto it!
$("a:eq(0)").jurlp("url", "www.example.com").jurlp("goto");

```

Parsing element text/HTML.
--------------------------

Parsing text/HTML for URLs.

It is possible for the URL parser to find URLs within text/HTML, and convert them into HTML anchor elements.

``` javascript

// Parse the HTML for URLs, and convert all URLs found in the text to anchors.
$("<div>Here are URLs: www.example1.com, www.example2.com</div>").jurlp();

// HTML becomes:
// <div>
//     Here are URLs:
//     <a href="http://www.example1.com/" class="jurlp-no-watch">www.example1.com</a>,
//     <a href="http://www.example2.com/" class="jurlp-no-watch">www.example2.com</a>
// </div>

```

Parsing URL strings directly.
-----------------------------

How to directly parse, modify or monitor an arbitrary URL string.

``` javascript

// Get an interface for parsing the document URL...
var url = $.jurlp();

// .. or get an interface for parsing your own URL.
url = $.jurlp("www.example.com");

// Parse the URL to an object.
url.url();

// Get the URL scheme.
url.scheme();

// Get the URL host.
url.host();

// Get the URL port.
url.port();

// Get the URL path.
url.path();

// Get the URL query.
url.query();

// Get a specific parameter value from the URL query.
url.query().parameter;

// Get the URL fragment.
url.fragment();

// Create a watch for new URLs that contain "example.com" in the host name
var watch = $.jurlp("example.com").watch(function(element, selector){
    console.log("Found example.com URL!", element, selector);
});

// We can even apply filters to the watch to be sure!
watch.jurlp("filter", "host", "*=", "example.com");

// Append a new URL, which will trigger the watch
$("body").append("<a href=\"www.example.com\"></a>");

// Stop watching for "example.com" URLs.
watch.jurlp("unwatch");

```

Unknown URLs.
-------------

Overview of unknown URL parsing.

**Unknown scheme**

The parser will attempt to parse any type of URL it encounters based on its scheme.  However, not all URLs are parsable, for example “spotify:track:<trackid>”.  In this case, the following URL object is returned;

``` javascript

{
    scheme: "spotify:",
    url: "track:<trackid>"
}

```

The unknown URL object will always contain the scheme (if present), for filtering purposes, and also contains a toString() method, which will convert the URL object back to the original URL string.

**mailto**

”mailto:” URLs are parsable in the same manner as a regular HTTP URL.  For example, the following URL object is returned for a URL with a “mailto:” scheme;

``` javascript

{
    scheme: "mailto:"
    user: "username",
    password: "",
    host: "www.example.com",
    port: "",
    path: "",
    query: "?subject=subject&body=body",
    fragment: ""
}

```

Therefore, “mailto:” URLs can be fully parsed using this parser, but note that it is not possible to set the password, port or fragment strings on a “mailto:” URL.

**javascript**

”javascript” URLs are parsable in the same manner as a regular HTTP URL.  For example, the following URL object is returned for a URL with a “javasrcipt:” scheme;

``` javascript

{
    scheme: "javascript:"
    user: "",
    password: "",
    host: "www.example.com",
    port: "",
    path: "/",
    query: "",
    fragment: "",
    javascript: "alert('!');"
}

```

Therefore, “javascript:” URLs can be fully parsed using this parser, but note that the current “document.location.href” will always be parsed/returned as the main URL object.