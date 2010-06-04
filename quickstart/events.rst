.. _quickstart/events:

JavaScript events and Dojo
==========================

:Status: Draft
:Version: 1.0
:Authors: Matt Bowen, Peter Higgins, Nikolai Onken
:Developers: ?-
:Available: since V?

.. contents::
    :depth: 2

Dojo's event system offers a refreshing alternative to the normal JavaScript events. With Dojo, you connect functions to one another, creating a link that calls one function when another fires. 

This means that you can connect a function of your own to:

* a DOM event, such as when a link is clicked
* an event of an object, such as an animation starting or stopping;
* a function call of your own, such as bar();
* a topic, which acts as a queue that other objects can publish objects to.

Your connected function is called when the event occurs. With simple events, when it calls your function, dojo passes your function a normalized event object, so that it can respond correctly, responding to keystrokes or stopping default behavior. With topics, Dojo passes any subscribed functions the object that was published. Dojo happily abstracts away all of the difficulty of cross-browser event systems, offering programmers a coherent event system that acts consistently across browsers.

Dojo's event system is flexible and gives you a few options for connecting your functions. In the core package, you have both simple events (which use a signal and slot system, similar to Qt's) and topics. In this section, you'll learn the following:

* how to connect functions to one another with :ref:`dojo.connect <dojo/connect>`
* what comes with an event object
* how to connect functions with topics and even publish your own objects to the topic
* how to enjoy event-based programming


==================
Simple Connections
==================

Dojo provides a pair of functions to handle many of your event-handling needs: ``dojo.connect`` and ``dojo.disconnect``. With :ref:`dojo.connect <dojo/connect>`, you can link one function to fire when another does, handling DOM events and custom functions with a single mechanism. Additionally, :ref:`dojo.connect <dojo/connect>` allows you to connect as many functions to an event as necessary. With :ref:`dojo.disconnect <dojo/disconnect>`, you can cancel a connection, assuming you've kept some reference to it. 

How does it work?
-----------------

Imagine that you're hungry and have decided to cook a pizza in your oven. The pizza will take 17 minutes, so you set a timer. You have better things to do than sit around your kitchen hanging out by the timer though, so you get your brother and tell him, "When you hear the oven timer, take the pizza out of the oven and bring me a slice." Your brother can only keep track of one thing at a time, and you don't want your house to burn down, so you tell your sister, "When you hear the over timer, turn off the oven." Because you're a little worried that your dirty oven might start to smoke, you tell them both, "If you hear the smoke alarm, come get me and then go outside." After you get your pizza, you tell your brother and sister that they don't have to worry about the oven alarm now and that they can go play until you call for them again. You then set the oven alarm to wake you up from a nap.

In this example, your siblings are functions. Your telling them to respond to certain events, such as "onPizzaDone" and "onHouseOnFire" performs the same function as :ref:`dojo.connect <dojo/connect>` - it sets up your siblings (functions) to listen for an event and perform their tasks when they receive notice. The various alarms are similar to event objects; they inform your siblings of important details about the situation (such as what is beeping). Telling your siblings that they don't need to worry about the oven alarm anymore is similar to :ref:`dojo.disconnect <dojo/disconnect>`; the next time the oven alarm goes off, it means that you need to wake up, and you don't want your brother hunting for a pizza needlessly, so you've told him to stop listening to that event.

Syntax
------
:ref:`dojo.connect <dojo/connect>` takes a variety of forms of arguments, depending on how you are planning to use it. This section will cover those various forms, based on use cases for them. You can think of it as a more in-depth version of the overview from the introdction provided in :ref:`Functions Used Everywhere <quickstart/dojo-basics>`.

dojo.connect has the following signature (acceptable types in square brackets):

.. code-block :: javascript 

  handle = dojo.connect(Scope of Event [object or null], Event [string], Context of Linked Method [string or null], Linked Method [string or function], Don't Fix Flag [boolean])

All of the options for calling ``dojo.connect`` are explored further below.

Example Code for Reference
--------------------------

Sometimes, it is easier to see an example first:

.. code-block :: html

     <html>
     <head>
       <title>Dojo Events are Great</title>
       <script src="dojo/dojo.js" type="text/javascript"></script>
       <script type="text/javascript">
          function foo() { console.debug("A click upon your houses!"); }
          function globalGuy() { console.debug("Global Guy fired!"); }
          var someObject = {
             bar: function() { console.debug("Bar fired!"); return 7; },
             baz: function() { console.debug("Baz fired!"); return 14; }
          }

          var anotherObject = {
              afterBaz: function () { console.debug("afterBaz fired!"); }
          }
       </script>
     </head>
     <body>
          <p><a id="firstLink" href="http://dojotoolkit.org/">Dojo</a> is an excellent tool kit.</p>
     </body>
     </html>

Connecting to a DOM Event
-------------------------

To connect a function to a DOM event with Dojo, you first need to get the node that you want to connect to. Here, I'll use the venerable 
:ref:`dojo.byId <dojo/byId>`.

.. code-block :: javascript

  firstLinkNode = dojo.byId("firstLink");


Now, to fire foo when a user clicks ``#firstLink``, and I have the node, so I just need to use dojo.connect for the heavy lifting:

.. code-block :: javascript

  firstLinkConnections = [];
  firstLinkConnections.push(dojo.connect(firstLinkNode, 'onclick', foo));


In this example, I passed ``dojo.connect`` the object I want my function to listen to (in this case, a DOM node), the name of the function that should trigger my function's call (in this case, the "onclick" event), and the name of my function. Note that I keep a reference to the connection by setting firstLinkConnections[0] to the return value of ``dojo.connect``. This will allow me to disconnect the listener later, if I desire. Now, when a user clicks "Dojo," a message appears in the log Because my function is global in scope, I can pass it directly to connect. The following, however, are equivalent:

.. code-block :: javascript 

  firstLinkConnections[0] = dojo.connect(firstLinkNode, 'onclick', null, foo);


**and**

.. code-block :: javascript
 
  firstLinkConnections[0] = dojo.connect(firstLinkNode, 'onclick', null, "foo");


Now, if I also want to connect someObject.bar() to #firstLink, we can do that too:

.. code-block :: javascript

  firstLinkConnections.push(dojo.connect(firstLinkNode, 'onclick', someObject, "bar"));

Because I've used Dojo's event handling, I can connect an arbitrary number of functions to fire on an event.

To stop listening to all the registered event handlers stored in ``firstLinkConnections``, pass the values in the Array to :ref:`dojo.disconnect <dojo/disconnect>`

.. code-block :: javascript

   dojo.forEach(firstLinkConnections, dojo.disconnect);

*note:* Notice the lack of () on dojo.disconnect. Here, we've passed ``forEach`` a function *reference*, which will be called forEach value in the Array. 

Events available for Connection
-------------------------------

Using dojo.connect on Dom Events is only the beginning or the power contained within. As a convenience, here is a quick list of normalized Dom Events

* onclick - the user clicked a node
* onfocus - a node received focus
* onblur - a node was 'blurred', or otherwise lost focus
* onchange - an input value was changed
* onkeypress - 
* onkeydown - fired when the user presses a key
* onkeyup - fired when the user releases a key
* onmouseover - a node was hovered (*warning:* may fire more than you'd like because of bubbling)
* onmouseout - a node was un-hovered
* onmouseenter - a normalized version of onmouseover that *wont* fire more than you'd like (only on first enter)
* onmouseleave - a normalized version of onmouseout that *wont* fire more than you'd like (only once when leaving)
* onsubmit - a form has been submitted

``TODOC: more?``

*A note about the event names:* Event names now are lower case, except in special cases (e.g., some Mozilla DOM events). Dojo will add "on" to your event name if you leave it off (e.g., 'click' and 'onclick' are the same thing to dojo). This differs from **Widget Events** in the sense Dijit uses mixedCase event names, to avoid potential conflicts. 

.. code-block :: javascript

  // connect to domEvent "onclick"
  var node = dojo.byId("foo");
  dojo.connect(node, "onclick", function(){ 

  });
  // connect to dijit event "onClick"
  var widget = dijit.byId("foo");
  dojo.connect(widget, "onClick", function(){

  });
  // and finally, connect to the domEvent "onclick" as it bubbles to our widget's domNode
  dojo.connect(widget.domNode, "onclick", function(){
      // if dojo.byId("foo") is inside this widget, both these functions will run
  });

The big difference being dojo.byId versus dijit.byId -- dojo.connect can connect to any function, method, or event. using dijit.byId, we're passed a reference to the Widget, and are connecting to it's pre-fabricated 'onClick' stub. 

**A note about return values:** Any value returned by a function called by ``dojo.connect`` will be lost.

Connecting to MouseWheel events
-------------------------------

One event not mentioned above, though entirely useful: onmousewheel (okay, it's two events, which is the reason for pointing this out ... )
All Mozilla based browsers use ``DOMMouseScroll``, and the rest ``onmousewheel`` ... You can quickly connect to whichever is needed using Dojo's :ref:`isSomething <quickstart/browser-sniffing>` variables:

.. code-block :: javascript

  var node = dojo.byId("foobar");  
  dojo.connect(node, (!dojo.isMozilla ? "onmousewheel" : "DOMMouseScroll"), function(e){
     // except the direction is REVERSED, and the event isn't normalized! one more line to normalize that:
     var scroll = e[(!dojo.isMozilla ? "wheelDelta" : "detail")] * (!dojo.isMozilla ? 1 : -1);
     console.log(scroll);
  });

Here we've fixed the event based on the Event Object provided, and are returning a number greater than 1 for scrolling up, and a negative value for scrolling down. 

Connecting Functions to One Another
-----------------------------------

Connecting functions to one another is even simpler than connecting them to DOM events; because you already have a reference to the function, you don't need to do any byId or query work. To have anotherObject.afterBaz fire after someObject.baz fires, use the following:

.. code-block :: javascript 
  
  objectConnections = [];
  objectConnections[0] = dojo.connect(someObject, "baz", anotherObject, "afterBaz");

In the above code, the first argument is the context of "baz," the second argument is the event (in this case, when baz fires), the third argument is the context of your listener function, and the fourth argument is the listener function itself. Connecting two global functions is even easier:

.. code-block :: javascript

  objectConnections[1] = dojo.connect(foo, globalGuy);

Now, whenever foo is called, globalGuy will also fire. As you might expect, connecting a method to a global function, or vice versa, is logical and simple:

.. code-block :: javascript

  objectConnections[2] = dojo.connect(foo, anotherObject, "afterBaz");
  objectConnections[3] = dojo.connect(someObject, "baz", globalGuy);

Disconnecting
-------------

To disconnect listeners from events, you simply pass the connection handle (the return value of ``dojo.connect`` to ``dojo.disconnect``. To disconnect globalGuy from someObject.baz, I use the following code:

.. code-block :: javascript

  dojo.disconnect(objectConnections[3]);

Or, by using :ref:`dojo.forEach <dojo/forEach>`, passing dojo.disconnect as a function reference as illustrated earlier:

.. code-block :: javascript
 
  dojo.forEach(objectConnections, dojo.disconnect);


================
The Event Object
================

When you connect a function to a DOM event with <cite>dojo.connect</cite>, Dojo passes your function a **normalized** event object. This means that, regardless of the client's browser, you can count on a set of standard attributes about the event and a set of methods to manipulate the event.

Assume that your function has been called by dojo.connect and takes an argument named <em>event</em>, like:

.. code-block :: javascript
  
  dojo.connect(dojo.byId("node"), "onclick", function(event){
     // the var 'event' is available, and is the normalized object
  });

Dojo provides the following attributes with an event object:

* event.target - the element that generated the event 
* event.currentTarget - the current target 
* event.layerX - the <em>x</em> coordinate, relative to the ``event.currentTarget``
* event.layerY - the <em>y</em> coordinate, relative to the ``event.currentTarget``
* event.pageX - the <em>x</em> coordinate, relative to the view port 
* event.pageY - the <em>y</em> coordinate, relative to the view port
* event.relatedTarget - For ``onmouseover`` and ``onmouseout``, the object that the mouse pointer is moving to or out of
* event.charCode - For keypress events, the character code of the key pressed
* event.keyCode - for keypress events, handles special keys like ENTER and spacebar.
* event.charOrCode - a normalized version of charCode and keyCode, which can be used for direct comparison for alpha keys and special keys together. (added in 1.1)

Dojo normalizes the following methods with an event object:

* event.preventDefault &mdash; prevent an event's default behavior (e.g., a link from loading a new page)
* event.stopPropagation &mdash; prevent an event from triggering a parent node's event

Additionally, :ref:`dojo.stopEvent(event) <dojo/stopEvent>` will prevent both default behavior any any propagation (bubbling) of an event.

Using a Dojo Event Object
-------------------------

In the example code, we have two functions that are connected to two different events. Echo sends the key code of any key typed in the text input field to the console. It does so by using the ``charCode`` property of the normalized event object. Foo is connected to the #link and cause it to send "The link was clicked" to the console instead of changing the browser's location; by using the preventDefault method of the normalized event object, connections to change the default behavior of DOM objects.

Now, imagine that you want to detect for the down arrow key in the text box. To do this, we just need to attach a new event listener to the text box and check to see if each keycode is the keycode for the down arrow. And how do you know what the keycode for the down arrow is, you may ask? Well, Dojo provides constants for every non-alpha-numeric key (see: :ref:`Key code constants <dojo/keys>`). In our case, we are interested in dojo.keys.DOWN_ARROW. So, assuming that you want to just log when the down arrow is pressed, the following code should do the job:

.. code-block :: javascript

  dojo.connect(interactiveNode, 'onkeypress', function (evt) {
    	key = evt.keyCode;
    	if(key == dojo.keys.DOWN_ARROW) {
		console.debug("The user pressed the down arrow!");
	}
  });

Using the new charOrCode, you can get a keys value directly with no need for checking null on the other:

.. code-block :: javascript

  dojo.connect(someInput, "onkeypress", function(e){
      switch(e.charOrCode){
         case dojo.keys.ENTER : submitForm(); break; 
         default: console.log('you typed: ', e.charOrCode); 
      }     
  });

*note:* We've used ``event``, ``evt``, and ``e`` for our event name in the callback function. You can name it whatever you like, it is the same object either way.


====================
Page Load and Unload
====================

``TODOC``


==================
Topic Based Events
==================

In addition to the simple event system created by <cite>dojo.connect</cite>, dojo offers support for anonymous publication and subscription of objects, via <cite>dojo.publish</cite> and <cite>dojo.subcribe</cite>. These methods allow a function to broadcast objects to any other function that has subscribed. This is dojo's topic system, and it makes it very easy to allow separate components to communicate without explicit knowledge of one another's internals.

There are three functions that you need to understand to use dojo's topic system: <cite>dojo.publish</cite>, <cite>dojo.subscribe</cite>, and <cite>dojo.unsubscribe</cite> <cite>Dojo.publish</cite> calls any functions that are connected to the topic via <cite>dojo.subscribe</cite>, passing to those subscribed functions arguments that are published (see syntax for details). As one might expect, <cite>dojo.unsubscribe</cite> will cause a previously subscribed function to no longer be called when <cite>dojo.publish</cite> is called in the future

How does it work?
-----------------

Imagine that you run a running a conference, and there will be updates throughout the day. You could collect contact information for everyone at the beginning of the day, along with each person's interests. However, this would be a lot of logistical work. Instead, you decide to use your facility's Public Address System. When there is an update to the schedule, you announce "This is an update to the schedule: the Dojo training is full and we have added yet a third time slot for it tomorrow." When there is meal information, you announce "This is an update about food: we will be serving free ice cream in the main hall in five minutes." This way, anyone interested in your information can pay attention to any updates that could change their behavior. You don't need to know who is subscribing, and they don't need to fill out a bunch of paper work &mdash; it's a win-win.

Example Code for Reference
--------------------------

.. code-block :: javascript

  function globalGuy(arg) { console.debug("Global Guy fired with arg " + arg); }
    var someObject = {
      bar: function(first, second) { console.debug("Bar fired with first of "+first+" and second of "+second); return 7; },
    }
  }

Subscribing and Publishing Topics
---------------------------------

To connect globalGuy to the topic "globalEvents" and someObject.bar to "fullNames", you simply use ``dojo.subscribe``, as follows:

.. code-block :: javascript

  topics = [];
  topics[0] = dojo.subscribe("globalEvents", null, globalGuy);
  topics[1] = dojo.subscribe("fullNames", someObject, bar);

Note that the following alternative form would also work:

.. code-block :: javascript

  topics = [];
  topics[0] = dojo.subscribe("globalEvents", globalGuy);
  topics[1] = dojo.subscribe("fullNames", someObject, "bar");

To publish information to both of these topics, you pass ``dojo.publish`` the topic names and arrays of the arguments that you want to pass to subscribed functions, as follows

.. code-block :: javascript

  dojo.publish("globalEvents", ["data from an interesting source"]);
  dojo.publish("fullNames", ["Alex", "Russell"]);

To disconnect someObject.bar from its topic, you use ``dojo.unsubscribe``, just as you would ``dojo.disconnect``:

.. code-block :: javascript
  
  dojo.unsubscribe(topics[1]);


=================
Events with Dijit
=================

``TODOC``
