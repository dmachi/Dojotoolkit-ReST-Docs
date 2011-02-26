.. _quickstart/Animation:

Animations and Effects with Dojo
================================

:Status: Draft
:Version: 1.0

.. contents::
   :depth: 2

Dojo provides several layers of Animation helpers, starting with Base Dojo (dojo.js), and adding in levels of incremental additions through the module system. All Animations in Dojo revolve around a single class: dojo.Animation, which acts as the underlying control mechanism for the flexible FX API Dojo provides.

==============================
Getting to know dojo.Animation
==============================

As mentioned, dojo.Animation is the foundation class for all Dojo animations. It provides several simple methods good for controlling your animation, such as `play`, `pause`, `stop`, and `gotoPercent`. The most simple method which is required of all animations is `play`:

.. code-block :: javascript
	:linenos:
	
	var animation = dojo.fadeOut({ // returns a dojo._Animation 
		// this is an Object containing properties used to define the animation
		node:"aStringId"
	});
	// call play() on the returned _Animation instance:
	animation.play();
	
You can simplify the above code using chaining, if you don't need to keep the animation object around for later use as follows:

.. code-block :: javascript
	:linenos:
	
	dojo.fadeOut({ node:"someId" }).play();
	
All animations in Dojo (with the exception of dojo.anim, introduced in Dojo 1.2) use predefined animation properties on the Object parameter to specify the animation settings. The `node:` property is the most important, and points to a node in the DOM on which to apply the animation. `node` can be a String ID of a DOM node, or a direct reference to a DOM node you already have:

.. code-block :: javascript
	:linenos:
		
	var target = dojo.byId("someId").parentNode;
	dojo.fadeOut({ node: target }).play();

Animation Properties
--------------------

The standard set of properties for specifying animation settings (via the Object parameter to the animation function) are:

:node:
  The domNode reference or string id of a node to apply the animation effects to. **required**

:delay:
  Delay, in milliseconds, before the animation starts.  The default is 0ms. **optional**

:duration:
  How long, in milliseconds, the animation will run.  The default is 350 milliseconds (.35 seconds) **optional** 

:easing:
  An easing (timing) function to apply to the effect, such as exponential curve, bounce, etc.  Dojo provides a number of easing functions in the
  :ref:`dojo.fx.easing <dojo/fx/easing>` module. **optional**

:rate:
  By default dojo runs its animations with 50 frames/second. This can be too fast in certain scenarios when want the whole animation to run a lot 
  slower. To change the framerate you use the rate property which defines the pause/delay between each frame. Ex. if you want 5 frames per second you 
  should specify a rate of 200 (miliseconds between each frame **optional**

:repeat:
  How many times the animation will be played.  Default: 0. **optional**

:curve:
  An array two values, or an instance of a `dojo._Line`. Used as the start and end points for a given animation. Typically not used directly by 
  end-users, though allows usage of the Animation class outside of Node effects

Animation Events
----------------

Performing custom behavior at specific points during an animation is done using callback functions (also set via the Object parameter to the animation function).  These functions will be executed at various stages during an animation's life-cycle. 

The standard set of events that are fired during stages of an animation are:

+-------------------------------+--------------------------------------------------------------------------------------------+
+**Property**                   |**Description**                                                                             |
+-------------------------------+--------------------------------------------------------------------------------------------+
| beforeBegin                   |A callback function which will be executed synchronously before playing the animation.      |
|                               |                                                                                            |
|                               |**optional** **new in 1.4**: passed node reference for the animation                        |
+-------------------------------+--------------------------------------------------------------------------------------------+
| onBegin                       |A callback function which will be executed asynchronously immediately after starting the    |
|                               |animation.                                                                                  |
|                               |                                                                                            |
|                               |**optional**                                                                                |
+-------------------------------+--------------------------------------------------------------------------------------------+
| onEnd                         |A callback function which will be executed synchronously when the animation ends.           |
|                               |                                                                                            |
|                               |**optional**  **new in 1.4**: passed node reference for the animation                       |
+-------------------------------+--------------------------------------------------------------------------------------------+
| onPlay                        |A callback function which will be executed synchronously when the animation is played.      |
|                               |                                                                                            |
|                               |**optional**                                                                                |
+-------------------------------+--------------------------------------------------------------------------------------------+
| onAnimate                     |A callback function fired for every step of the animation, passing                          |
|                               |a value from a dojo._Line for this animation.                                               |
|                               |                                                                                            |
|                               |**optional**                                                                                |
+-------------------------------+--------------------------------------------------------------------------------------------+

Consider this simple fade animation, and all the potential callbacks registered:

.. code-block :: javascript
  :linenos:

  dojo.fadeOut({ 

	// some node, by id to animate:
	node:"someId",
	
	beforeBegin: function(){
		// executed synchronously before playing
	},
	onBegin: function(){
		// executed asynchronously immediately after starting
	},
	onEnd: function(){
	 	// executed when the animation is done
	},
	onPlay: function(){
		// executed when the animation is played
	},
	onAnimate: function(values){
		// fired for every step of the animation, passing
		// a value from a dojo._Line for this animation
	}

  }).play();

You can define these callback functions as part of the Object parameter used to define the animation initially (as seen above) or use :ref:`dojo.connect <dojo/connect>` to connect directly to the instance and listen for the function calls.

.. code-block :: javascript
	:linenos:
	
	var animation = dojo.fadeOut({ node:"someNodebyId" });
	dojo.connect(animation, "onEnd", function(){
	 	// connect externally to this animation instance's onEnd function
	});
	animation.play(); // start it up

**new in Dojo 1.4** - The onEnd and beforeBegin events are fired passing a reference to the node being animated so that you may more easily manipulate a node immediately before or after an animation:

.. code-block :: javascript
    :linenos:

    dojo.fadeOut({
        node:"foo",
        onEnd: function(n){
             n.innerHTML = "";
        },
        beforeBegin: function(n){
             n.innerHTML = "Bye!";
        }
    }).play();


===============	
Base Animations
===============

Base Dojo provides the animation framework as well as several simple helper animations for fading, and one incredibly useful function `dojo.animateProperty` (the workhorse of most CSS-based animations). All use the same Object parameter format for specifying properties of the animation, and several additional options are used in advanced cases. 

Fading Example
--------------

To fade out a node, alter it's contents, and fade it back in:

.. code-block :: javascript
	:linenos:
	
	var node = dojo.byId("someId");
	dojo.fadeOut({
		node: node,
		onEnd: function(){
			node.innerHTML = "<p>Like magic!</p>"
			dojo.fadeIn({
				node: node
			}).play()
		}
	}).play();

Here, we've created a fadeOut animation, and run it immediately. At the end of the animation (set here to use the default duration by omitting the `duration:` parameter), we set the node reference's `.innerHTML` property to something new, and fade it back in, again using the default duration. 

Animating CSS Properties
------------------------

In addition to generic animations, Dojo provides shorthand helper functions for animating CSS properties via the :ref:`animateProperty <dojo/animateProperty>` API. An example where this specialized animation API simplifies specifying animation would be when you need to fade a background color property from red to green to indicate status changes.

=================================
Core Animations: Advanced helpers
=================================

Above the Base Animations (those contained entirely within dojo.js), there are several modules 
available within the toolkit for advanced animation control. 

To use these extended functions, you must include the `dojo.fx` module:

.. code-block :: javascript
	:linenos:
	
	dojo.require("dojo.fx");

The namespace `dojo.fx` has been reserved for all these animation, including `dojo.fx.chain` and `dojo.fx.combine`. 


=================================
Chaining and Combining Animations
=================================

Two convenience functions provided in the `dojo.fx` module named `combine` and `chain` create an animation from a series of animations in an array. 

`combine` merges the array of animations them into one animation instance to control them all in parallel, whereas `chain` merges the animations into a single animation, playing back each of the animations in series, or one right after the other.

To fade out two nodes simultaneously:

.. code-block :: javascript
	:linenos:
	
	dojo.require("dojo.fx");
	dojo.addOnLoad(function(){
		// create two animations
		var anim1 = dojo.fadeOut({ node: "someId" });
		var anim2 = dojo.fadeOut({ node: "someOtherId" });
		// and play them at the same moment
		dojo.fx.combine([anim1, anim2]).play();
	});

(Notice we wrapped the animation call in and addOnLoad function this time. This is required always, as you cannot modify the DOM before the DOM is ready, which :ref:`addOnLoad <dojo/addOnLoad>` alerts us to. Also, we need to ensure the `dojo.fx` module has been loaded properly)

Javascript is rather flexible about return values and where functions are called. The above example can alternatively be written in a shorthand like:

.. code-block :: javascript
	:linenos:

	dojo.require("dojo.fx");
	dojo.addOnLoad(function(){
		// create and play two fade animations at the same moment
		dojo.fx.combine([
			dojo.fadeOut({ node: "someId" }),
			dojo.fadeOut({ node: "someOtherId" })
		]).play();
	});

The same rules apply to a combined animation as do a normal animation, though with no direct way to mix event callbacks into the combine() call, you are left using the `dojo.connect` method to attach event handlers:

.. code-block :: javascript
	:linenos:
	
	var anim = dojo.fx.combine([
		dojo.fadeOut({ node: "id", duration:1000 }),
		dojo.fadeIn({ node: "otherId", duration:2000 })
	]);
	dojo.connect(anim, "onEnd", function(){
		// fired after the full 2000ms
	});

Alternately, you can mix event handlers into your individual animations passed to dojo.fx.combine:

.. code-block :: javascript
	:linenos:
	
	var animA = dojo.fadeOut({
		node:"someNode",
		duration: 500,
		onEnd: function(){
			// fired after 500ms
		}
	});
	var animB = dojo.fadeIn({ node:"otherNode" });
	dojo.fx.combine([animA, animB]).play();

Chain works in much the same way - though plays each animation one right after the other:

.. code-block :: javascript
	:linenos:
	
	dojo.fx.chain([
		dojo.fadeIn({ node: "foo" }), 
		dojo.fadeIn({ node: "bar" })
	]).play();

All of the same patterns apply to chain as to other animation instances. A good article covering `advanced usage of combine and chain <http://dojocampus.org/content/2008/04/11/staggering-animations/>`_ is available at DojoCampus. 

combine and chain accept an Array, and will work on a one-element array. This is interesting because you can manually create animations, pushing each into the array, and chain or combine the resulting set of animations. This is useful when you need to conditionally exclude some Animations from being created:

.. code-block :: javascript
	:linenos:
	
	// create the array
	var anims = [];
	// simulated condition, an array of id's:
	dojo.forEach(["one", "two", "three"], function(id){
		if(id !== "two"){
			// only animate id="one" and id="three"
			anims.push(dojo.fadeOut({ node: id }));
		}
	});
	// combine and play any available animations waiting
	dojo.fx.combine(anims).play();

Obviously, any logic for determining if a node should participate in an animation sequence is in the realm of the developer, but the syntax should be clear. Create an empty Array, push whichever style and types of animations you want into the Array, and call combine() on the list. 


================
Animation Easing
================

Have you ever wanted to perform an animated effect such as fade out, fade in, wipe in, but apply the effect in a non-linear way? For example, wouldn't it be cool to have a fade in accelerate the rate at which the node appears the further along in the animation duration it is, or provide a bit of bounce to your slide in animation? The functions which control the timing of the animation is handled through the 'easing' property of the arguments passed to the animation creation functions.

Instead of having to write the easing function yourself, dojo provides a collection of standard easing functions to use as this parameter to get a variety of effects.  See :ref:`Easing functions <dojo/fx/easing>` for more information on the easing function provided out of the box.

============
Text Effects
============

As mentioned above, the dojox/fx module provides additional effects over and beyond these basic animation capabilities.  On of the effects in the dojox package that is especially neat is effects that can operate on text directly, which can allow you to easily do animations such as exploding all the characters in a paragraph all over your page.  Make sure to check out these additional text effects once you understand the basics.

=============================
Animation in Dojo 1.5 widgets
=============================

Using the latest in CSS3 along with the Dojo APIs increases the performance of animation and makes it easier for designers to code the animation using CSS3. 

See details on application of animation in specific Digits in :ref:`Themes and theming <dijit/themes>`.
