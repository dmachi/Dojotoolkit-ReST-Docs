.. _dojo/contentHandlers:

dojo.contentHandlers
====================

:Project owner: Peter Higgins
:Available: since V1.4

.. contents::
   :depth: 2

dojo.contentHandlers is an object containing several pre-defined "handlers" for Ajax traffic, exposed as a public API to allow your own custom handlers to be mixed in.


=====
Usage
=====

The most common usage of contentHandlers is indirect. When making an Ajax call, the "handleAs" attribute is used to lookup in the dojo.contentHandlers dictionary. The function defined in the dictionary with a matching key is called, passing the XHR object used for the Ajax call. The **return** value from a contentHandler function is then passed to any "load", "handle" or callback functions.


Default contentHandler
----------------------

The default contentHandler is text, and requires no action:

.. code-block :: javascript
 :linenos:

 <script type="text/javascript">
     dojo.xhrGet({
         url:"foo.txt",
         load: function(data){
             // this uses the default dojo.contentHandlers.text. It simply returns plaintext.
         }
     });
 </script>


Available Handlers
------------------

There are several pre-defined contentHandlers available to use. The value represents the key in the handlers map. 

* **text** (default) - Simply returns the response text
* **json** - Converts response text into a JSON object
* **xml** - Returns a XML document
* **javascript** - Evaluates the response text
* **json-comment-filtered** - A (arguably unsafe) handler to preventing JavaScript hijacking
* **json-comment-optional** - A handler which detects the presence of a filtered response and toggles between json or json-comment-filtered appropriately.


========
Examples
========
  
Using a pre-defined handler
---------------------------

This example shows, how to use the pre-defined json contentHandler:

.. code-block :: javascript
  :linenos:

  dojo.xhrGet({
      url:"foo.json",
      // here comes the contentHandler:
      handleAs: "json",
      load: function(data){
          if(data && !data.error){
             // see if our response contains an `error` member. { error:"Something is wrong" } for example
          }else{
             // something went wrong :)
          }
      }
  });


Creating a custom handler
-------------------------

To create a custom contentHandler, simply mix a new key into the dojo.contentHandlers object defining the 'handleAs' value. The XHR object is passed to this function. For example: 

.. code-block :: javascript
  :linenos:

  dojo.mixin(dojo.contentHandlers, {
      "makeUpper": function(xhr){
           return xhr.responseText.toUpper();
       }
  });

  // then later:
  dojo.xhrPost({
      url:"foo.php", 
      handleAs:"makeUpper",
      load: function(data){
          // data is a CAPS version of the original responseText
      }
  });

One can create any number of content handlers, and can do about anything they choose within the provided API. For instance, the original args used to create the XHR object are stored on the object itself as `ioArgs` (eg: xhr.ioArgs) and can be used to mix custom attributes and instructions to the handler. 

For instance, we can create a handler that will populate a node with the response text automatically:

.. code-block :: javascript
  :linenos:

  // you don't need to mix(), you can just set the object directly if you prefer:
  dojo.contentHandlers.loadNode = function(xhr){
      var n = dojo.byId(xhr.ioArgs.node);
      n && n.innerHTML = xhr.responseText;
  }

  // to use:
  dojo.xhrGet({
       url:"foo.html", 
       handleAs:"loadNode",
       node: "someId"
  });

This will inject foo.html content into a node with id="someId". A side effect of the above example would be any callbacks passed to something handled by the "loadNode" contentHandler would not also get a copy of the content. You should return a value from a contentHandler.


Using dual handlers
-------------------

The other contentHandlers are all functions. If you like, you can define a new handler that acts as if it were another handler and doing something else. Simply call the other contentHandler passing the xhr reference you were passed in your custom handler:

.. code-block :: javascript
 :linenos:

    dojo.contentHandlers.wrappedJSON = function(xhr){
        // like handleAs:"json", but mixes an additional bit into the response always.
        var json = dojo.contentHandles.json(xhr);
        return dojo.mixin(json, { _wrapped_by_app:true });
    };

    dojo.xhrGet({
        url:"users.json",
        handleAs:"wrappedJSON",
        load: function(data){
            if(data._wrapped_by_app){
                console.log("neat!");
            }
        }
    });


Overwriting a handler
---------------------

Standard AOP techniques apply. If you find yourself needing to *replace* a contentHandler but preserve the original beahvior, simply duck-punch around it:

.. code-block :: javascript
 :linenos:

    // a handler that always escapes html fragments. not exceptionally useful though:
    var oldtext = dojo.contentHandlers.text;
    dojo.contentHandles.text = function(xhr){
        return oldtext.apply(this, arguments).reaplce("<", "&lt;");
    };


=====
Notes
=====

This functionality is "new" in Dojo 1.4. An alias to the "private" dojo._contentHandlers will remain in place until 2.0. Version prior to 1.4 can use the "private" alias the same way as outlined in this document. 
