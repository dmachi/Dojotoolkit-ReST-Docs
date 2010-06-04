.. _dojo/isAlien:

dojo.isAlien
============

:Status: Draft
:Version: 1.0
:Available: since V?

.. contents::
   :depth: 2

Checks if the parameter is a built-in function.

As with all dojo._base components, these functions are included within Dojo Base. You get this functionality by just including dojo.js or dojo.xd.js in your page.


=====
Usage
=====

Use this to test if a variable is a built-in function.

.. code-block :: javascript
  :linenos:

  dojo.isAlien(foo) 

Returns true if it is a built-in function or some other kind of oddball that *should* report as a function but doesn't.

.. code-block :: javascript
  :linenos:

  // Check, if variable "foo" is a built-in function:
  if(dojo.isAlien(foo)){ 
      // do something...
  }


========
Examples
========

Test against isAlien()
----------------------

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
            // Is t an built-in function?
            if (dojo.isAlien(t)) {
                // dojooo: t is a built-in function!
                dojo.attr(resultDiv, "innerHTML", 
                    "Yes, good choice: 't' is a built-in function.<br />Try another button.");

                // Change the backgroundColor:
                dojo.style(resultDiv, {
                    "backgroundColor": "#a4e672",
                    "color": "black"
                });
            } else {
                // no chance, this can't be a built-in function:
                dojo.attr(resultDiv, "innerHTML", 
                    "No chance: 't' can't be a built-in function with such a value " 
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

* :ref:`dojo.isString <dojo/isString>` - Checks if the parameter is a String
* :ref:`dojo.isArray <dojo/isArray>` - Checks if the parameter is an Array
* :ref:`dojo.isFunction <dojo/isFunction>` - Checks if the parameter is a Function
* :ref:`dojo.isObject <dojo/isObject>` - Checks if the parameter is an Object
* :ref:`dojo.isArrayLike <dojo/isArrayLike>` - Checks if the parameter is like an Array
