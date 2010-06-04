.. _dojo/formToObject:

dojo.formToObject
==================

:Available: since V0.9

.. contents::
   :depth: 2

Helper function for converting a HTML form to a JavaScript Object


============
Introduction
============

This function is one of many helpers used by the dojo Xhr subsystem for handling AJAX style requests. This particular function takes a HTML form node and converts the form elements into a JavaScript object of name/value pairs. Disabled form elements, buttons, and other non-valued HTML elements are skipped. The resulting object can then be processed however you like. Note that the form can contain dijit.form.* items.


=====
Usage
=====

Usage is simple and straightforward, you pass the form node or the string id of a form node to convert:

.. code-block :: javascript
 :linenos:

   var formObj = dojo.formToObject("myId");.

The function will hand back a JavaScript object of the name/value pairs from the form elements. The values are either strings or arrays. If a checkbox group has a single item selected, the function will return a string value; if several, an array.


========
Examples
========

Create an object version of a form
----------------------------------

.. code-example::
  
  .. javascript::

    <script type="text/javascript">
      dojo.require("dijit.form.Button");

      dojo.addOnLoad(function(){
        dojo.connect(dijit.byId("convertForm"), "onClick", function(){
           var formObject1 =  dojo.formToObject("myform");

           // Attach it into the dom as pretty-printed text.
           dojo.byId("formObject").innerHTML = dojo.toJson(formObject1, true);
        });
      });
    </script>

  .. html:: 

    <p>The FORM</p>
    <form id="myform">
       <p>text1: <input type="text" name="text1" value="value1"><br>
       text2: <input type="text" name="text2" value="value2"><br>
       cb_group.foo: <input id="f6_checkbox1" type="checkbox" name="cb_group" value="foo" checked><br>
       cb_group.boo: <input id="f6_checkbox2" type="checkbox" name="cb_group" value="boo"><br>
       radio_group.baz: <input id="f6_radio1" type="radio" name="radio_group" value="baz"><br>
       radio_group.bam: <input id="f6_radio2" type="radio" name="radio_group" value="bam" checked><br>
       radio_group.baf: <input id="f6_radio3" type="radio" name="radio_group" value="baf"></p>
    </form>
    <p><button id="convertForm" dojoType="dijit.form.Button">Click to convert the form to an object</button></p>
    <p>The form as an object:</p>
    <pre id="formObject"></pre>


Create an object version of a form with Dijit widgets
-----------------------------------------------------

.. code-example::
  
  .. javascript::

    <script type="text/javascript">
      dojo.require("dijit.form.TextBox");
      dojo.require("dijit.form.CheckBox");
      dojo.require("dijit.form.Button");

      dojo.addOnLoad(function(){
        dojo.connect(dijit.byId("convertFormDigits"), "onClick", function(){
           var formObject2 =  dojo.formToObject("myform2");

           // Attach it into the dom as pretty-printed text.
           dojo.byId("formObject2").innerHTML = dojo.toJson(formObject2, true);
        });
      });
    </script>

  .. html:: 

    <p>The FORM</p>
    <form id="myform2">
       <p><input type="text" dojoType="dijit.form.TextBox" name="field1" value="value1"><br>
       <input type="text" dojoType="dijit.form.TextBox" name="field2" value="value2"><br>
       <input id="mycheck" dojoType="dijit.form.CheckBox" name="field3" value="agreed" checked> <label for="mycheck">I agree</label></p>
    </form>
    <p><button id="convertFormDigits" dojoType="dijit.form.Button">Click to convert the form to an object</button></p>
    <p>The form as an object:</p>
    <pre id="formObject2"></pre>


========
See also
========

* :ref:`dojo.objectToQuery <dojo/objectToQuery>`
* :ref:`dojo.queryToObject <dojo/queryToObject>`
* :ref:`dojo.formToQuery <dojo/formToQuery>`
* :ref:`dojo.formToJson <dojo/formToJson>`
