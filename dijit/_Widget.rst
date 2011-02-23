.. _dijit/_Widget:

dijit._Widget
=============

:Authors: Bill Keese, Nikolai Onken
:Project owner: Bill Keese
:Available: since V0.9

.. contents::
   :depth: 2


============
Introduction
============

dijit._Widget is the base class for all widgets in dijit, and in general is the base class for all dojo based widgets. Usually widgets also extend other mixins such as :ref:`dijit._Templated <dijit/_Templated>`.

Note that the underscore in the name implies not that _Widget is a private class, but rather that it's a base class, rather than a widget directly usable.test


=====
Usage
=====

All widgets are created by calling dojo.declare(), extending from _Widget:

.. code-block :: javascript
 :linenos:

 <script type="text/javascript">
   dojo.declare("MyWidget", [dijit._Widget], { ... });
 </script>

and then redefining a number of methods for the widget lifecycle...


=========
Lifecycle
=========

The lifecycle of a widget decribes the phases of its creation and destruction which you can hook into. It's useful to understand exactly what happens when. Whether you are sub-classing an existing widget, using dojo/method script blocks, or passing in method overrides to the constructor, these are your entry points for making a widget do what you want it to do.

Widgets are classes, created with dojo.declare. All widgets inherit from dijit._Widget, and most get the _Templated mixin. That provides you the following extension points (methods) you can override and provide implementation for:

- constructor
     Your constructor method will be called before the parameters are mixed into the widget, and can be used to initialize arrays, etc.

- parameters are mixed into the widget instance
     This is when attributes in the markup (ex: <button iconClass=...>) are mixed in or, if you are instantiating directly, the properties object you passed into the constructor (ex: new dijit.form.Button({label: "hi"})). This step itself is not overridable, but you can play with the result in...

- postMixInProperties
     If you provide a postMixInProperties method for your widget, it will be invoked before rendering occurs, and before any dom nodes are created. If you need to add or change the instance's properties before the widget is rendered - this is the place to do it.

- buildRendering
     :ref:`dijit._Templated <dijit/_Templated>` provides an implementation of buildRendering that most times will do what you need. The template is fetched/read, nodes created and events hooked up during buildRendering. The end result is assigned to this.domNode. If you don't mixin :ref:`dijit._Templated <dijit/_Templated>` (and most OOTB dijits do) and want to handle rendering yourself (e.g. to really streamline a simple widget, or even use a different templating system) this is where you'd do it.

- setters are called
     All attributes listed in attributeMap are applied to the DOM, and attributes for which there are custom setters (see :ref:`attributes <quickstart/writingWidgets/attributes>`, those custom setters are called

- postCreate
   This is typically the workhorse of a custom widget. The widget has been rendered (but note that sub-widgets in the containerNode have not!). The widget though may not be attached to the DOM yet so *you shouldn't do any sizing calculations in this method*.

- startup
    If you need to be sure parsing and creation of any child widgets has completed, use startup. This is often used for layout widgets like BorderContainer. If the widget does JS sizing, then startup() should call resize(), which does the sizing. 

- destroy
     Implement destroy if you have special tear-down work to do (the superclasses will take care of most of it for you.

Other methods
-------------

- resize
    All widgets that do JS sizing should have a method called resize(), that lays out the widget. Resize() should be called from startup() and will also be called by parent widgets like :ref:`dijit.layout.ContentPane <dijit/layout/ContentPane>`.

this.inherited()
----------------

In all cases its good practice to assume that you are overriding a method that may do something important in a class up the inheritance chain. So, call this.inherited() before or after your own code. E.g.

.. code-block :: javascript

  postCreate: function() {
     // do my stuff, then...
     this.inherited(arguments);
  }


==========
Attributes
==========

Perhaps the most important feature of _Widget is the ability to set attributes at widget initialization, or to change their valuse later on in the widget's lifecycle.

dijit._Widget implements the attr() method to do this. For example, this call will set a DateTextBox's value to the current date:

.. code-block:: javascript

   myDateTextBox.attr('value', new Date())

This call will tell us if a TitlePane is opened or closed:

.. code-block:: javascript

   myTitlePane.attr('open')

attr() makes use of:

  * the attributeMap
  * custom setters/getters

The attributeMap specifies a mapping of widget attributes into the DOM tree for the widget. It can map a TitlePane's title to the DOM node listing the title, for example.

The custom setters/getters can perform any needed operation for setting/resetting a value. They are used when attributeMap won't do the job.

For more details on both attributeMap and custom setters/getters, see the Writing Widgets :ref:`quickstart` guide.


========
See also
========

* Writing Widgets :ref:`quickstart`

.. _quickstart: quickstart/writingWidgets
