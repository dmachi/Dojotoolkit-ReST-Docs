.. _dojo/hash:

=========
dojo.hash
=========

:Status: Draft
:Version: 1.4
:Available: 1.4

.. contents::
    :depth: 3

dojo.hash provides methods for monitoring and updating the hash in the browser URL as well as back/forward support and bookmarkability for dynamic web applications.

Background
==========

HTML5 defines an onhashchange event that fires when the value of the hash segment of the URL changes. 

http://www.w3.org/TR/html5/history.html#history-traversal

dojo.hash uses the native onhashchange event if the browser supports it, and emulates this support for older browsers using a polling loop.

Initialization
==============

To use dojo.hash to listen for hash changes:

1. Add the appropriate require statement:

   .. code-block :: javascript

      dojo.require("dojo.hash");
   ..

2. Subscribe to /dojo/hashchange event:

   .. code-block :: javascript

       dojo.subscribe("/dojo/hashchange", context, callback);
   ..

   Whenever the hash changes, your callback will be called with the new hash value passed as the first parameter.


Manipulating the URL
====================

Dojo.hash() provides mechanisms for getting and setting the URL's hash value. 

Setter
------
  To update the hash value, call dojo.hash with the new hash value as the first parameter. This will create a new entry in the back history and will notify any /dojo/hashchange subscribers:

  .. code-block :: javascript

      dojo.hash("someHashValue");
  ..

  To update the hash without creating a new entry in the back history, pass true as the second (replace) param. This will update the URL to the new hash, and will replace the current history state:

  .. code-block :: javascript

      dojo.hash("someHashValue",true);
  ..

Getter
------
  dojo.hash() with no parameters returns the current hash value.

  .. code-block :: javascript

      var hashValue = dojo.hash();
  ..

Examples
========

1) Subscribe to the hashchange event:

   .. code-block :: javascript

       dojo.subscribe("/dojo/hashchange", context, callback);
   ..

2) Set the hash using query notation:

   .. code-block :: javascript

       function() {
           var obj = {
               firstParam: true,
               secondParam: false
           }
           dojo.hash(dojo.objectToQuery(obj));
       }

   ..

  Hash: #firstParam=true&secondParam=false


3) Parse this hash and add a parameter using query notation:

 Initial hash: #firstParam=true&secondParam=false

 .. code-block :: javascript

     function updateHash() {
         var obj = dojo.queryToObject(dojo.hash());  //get
         obj.someNewParam = true;  
         dojo.hash(dojo.objectToQuery(obj));  //set
     }

 ..

 End hash: #firstParam=true&secondParam=false&someNewParam=true

4) React to hashchange event (hash passed to callback as first param):

   .. code-block :: javascript

       function callback(hash) {
           //hashchange event!
           var obj = dojo.queryToObject(hash);
           if (obj.firstParam) {
                // do something
           }
       }
   ..

5) Get and set the hash using slash notation:

 Initial hash:  #/firstSegment/secondSegment

 .. code-block :: javascript

     function updateHash() {
         var obj = dojo.hash().split("/");
         obj.push("trailingSegment");
         dojo.hash(obj.join("/"));
     }

 ..

 End hash:  #/firstSegment/secondSegment/trailingSegment

Advanced
========

Customizing the polling loop frequency
 For browsers that don't support the onhashchange event natively, a polling loops monitors the URL for changes. The default duration of this polling loop is 100 ms.  To customize this value, add "hashPollFrequency" to dojo config.

 .. code-block :: javascript

  var djConfig = { hashPollFrequency: 200 };

 ..

Encoding/Decoding
 dojo.hash does not attempt to do any encoding or decoding.  There are many cases where consumers of dojo.hash want unencoded slashes, etc, so it's up to the consumer to encode and decode where appropriate. Anything with HTML encoding (i.e. &amp;) must be encoded with encodeURIComponent before being passed into dojo.hash due to discrepancies between browsers (Firefox decodes HTML encoding automatically before setting the URL, IE does not).


 .. code-block :: javascript

   dojo.hash(encodeURIComponent("hash with &amp; HTML encoding"))

 ..

XD Dojo
 If you're using cross-domain Dojo, you must specify a local copy of a blank HTML page via 'dojoBlankHtmlUrl' configuration parameter.  If you don't, dojo.hash will not work in IE 6 or IE 7.

 .. code-block :: javascript

  var djConfig = { dojoBlankHtmlUrl: '/blank.html' };

 ..
