.. _dojo/isFunction:

dojo.isFunction
===============

:Status: Draft
:Version: 1.0
:Available: since V0.9

.. contents::
   :depth: 2

Checks if the parameter is a Function.

As with all dojo._base components, these functions are included within Dojo Base. You get this functionality by just including dojo.js or dojo.xd.js in your page.


=====
Usage
=====

.. code-block :: javascript
 :linenos:

  var ref = function(a,b){ return a };
  if(dojo.isFunction(ref)){
    ref();
  }

This also works with classes created by :ref:`dojo.declare <dojo/declare>`

.. code-block :: javascript
  :linenos:

  dojo.declare("Thing", null, { constructor: function(){ });
  var ref = Thing;
  if(dojo.isFunction(ref)){
     var mine = new ref();
  }

A common pattern is to have a a string version of a fully declared class. You can use dojo.isFunction to tell if you need to convert it to a function or not:

.. code-block :: javascript
  :linenos:

  // some dynamic class to use:
  var thing = "dijit.Dialog";
  // check it out first:
  if(!dojo.isFunction(thing)){
    thing = dojo.getObject(thing);
  }
  var dialog = new thing({ title:"bar" });

One could use :ref:`dojo.isString <dojo/isString>` for a similar check.


========
Examples
========

Test against isFunction()
-------------------------

.. cv-compound::

  .. cv:: css

     <style type="text/css">
         .style1 { background: #f1f1f1; padding: 10px; }
     </style>

  .. cv:: javascript

    <script type="text/javascript">
        dojo.require("dijit.form.Button");

        // test variable t:
        var t;

        function testIt() {
            // resultDiv is the spanning DIV around the result:
            var resultDiv = dojo.byId('resultDiv');

            // Here comes the test:
            // Is t a Function?
            if (dojo.isFunction(t)) {
                // dojooo: t is a function!
                dojo.attr(resultDiv, "innerHTML", 
                    "Yes, good choice: 't' is a function.<br />Try another button.");

                // Change the backgroundColor:
                dojo.style(resultDiv, {
                    "backgroundColor": "#a4e672",
                    "color": "black"
                });
            } else {
                // no chance, this can't be an array:
                dojo.attr(resultDiv, "innerHTML", 
                    "No chance: 't' can't be a function with such a value " 
                     + "('t' seems to be a " + typeof t + ").<br />"
                     + "Try another button.");

                // Change the backgroundColor:
                dojo.style(resultDiv, {
                    "backgroundColor": "#e67272",
                    "color": "white"
                });
            }
        }
    </script>

  .. cv:: html

    <div style="height: 100px;">
        <button dojoType="dijit.form.Button">
            t = 1000;
            <script type="dojo/method" event="onClick" args="evt">
                // Set t:
                t = 1000;

                // Test the type of t:
                testIt();
            </script>
        </button>
        <button dojoType="dijit.form.Button">
            t = "text";
            <script type="dojo/method" event="onClick" args="evt">
                // Set t:
                t = "text";

                // Test the type of t:
                testIt();
            </script>
        </button>
        <button dojoType="dijit.form.Button">
            t = [1, 2, 3];
            <script type="dojo/method" event="onClick" args="evt">
                // Set t:
                t = [1, 2, 3];

                // Test the type of t:
                testIt();
            </script>
        </button>
        <button dojoType="dijit.form.Button">
            t = { "property": 'value' };
            <script type="dojo/method" event="onClick" args="evt">
                // Set t:
                t = { "property": 'value' };

                // Test the type of t:
                testIt();
            </script>
        </button>
        <button dojoType="dijit.form.Button">
            t = function(a, b){ return a };
            <script type="dojo/method" event="onClick" args="evt">
                // Set t:
                t = function(a, b){ return a } ;

                // Test the type of t:
                testIt();
            </script>
        </button>

        <div id="resultDiv" class="style1">
            Click on a button, to test the associated value.
        </div>
    </div>


========
See also
========

* :ref:`dojo.getObject <dojo/getObject>` - Returns the property of an object from a dot-separated string such as "A.B.C"
* :ref:`dojo.isString <dojo/isString>` - Checks if the parameter is a String
* :ref:`dojo.isArray <dojo/isArray>` - Checks if the parameter is an Array
* :ref:`dojo.isObject <dojo/isObject>` - Checks if the parameter is an Object
* :ref:`dojo.isArrayLike <dojo/isArrayLike>` - Checks if the parameter is like an Array
* :ref:`dojo.isAlien <dojo/isAlien>` - Checks if the parameter is a built-in function
