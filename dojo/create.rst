.. _dojo/create:

dojo.create
===========

:Available: since V1.3

.. contents::
   :depth: 2

A convenient DOM creation, manipulation and placement utility shorthand. 


============
Introduction
============

dojo.create() is designed to simplify the frequently used sequence of DOM manipulation: 

* create a node, 
* set attributes on it, 
* and place it in the DOM. 

It can be used with existing nodes too, if you want to assign new attributes and place it afterwards.

To see this utility in context, read the :ref:`DOM Quick Start <quickstart/dom>` first.


=====
Usage
=====

The function takes up to four arguments:

dojo.create(tag, attrs, refNode, pos);

Here is the definition of arguments:

=========  ===========  =======================================================
Parameter  Type         Description
=========  ===========  =======================================================
tag        String|Node  Can be a string or a DOM node. If it is a string, it 
                        will be treated as a node tag name, which will be used 
                        to create a new node.
                        When creating a node the owner document of ``refNode`` 
                        is used. If ``refNode`` is ``null`` or unspecified, 
                        :ref:`dojo.doc <dojo/doc>` is used.
attrs      Object|null  A dictionary of attributes to be set on ``node``. 
                        This parameter is passed to :ref:`dojo.attr <dojo/attr>` 
                        unmodified after the node is created.
                        Can be ``null`` or undefined meaning 
                        "don't set any attributes". You should always specify 
                        it explicitly (as ``null``) if you want to specify 
                        the rest of arguments.
refNode    String|null  A string (interpreted as an id of a DOM node) or 
                        a DOM node. This parameter is passed to 
                        :ref:`dojo.place <dojo/place>` unmodified after the node is 
                        created, and attributes are set.
                        Can be omitted meaning "don't place the node".
pos        String|null  Optional argument. Can be a number or one of the 
                        following strings: "before", "after", "replace", "only", 
                        "first", or "last". If omitted, "last" is assumed. 
                        This parameter is passed to :ref:`dojo.place <dojo/place>` 
                        unmodified after the node is created, and attributes 
                        are set. Please see :ref:`dojo.place <dojo/place>` 
                        for more details and examples.
=========  ===========  =======================================================


The function returns a DOM node it created with ``tag`` or the first argument, if it was a DOM node.


========
Examples
========

Create a <div>:

.. code-block :: javascript
  :linenos:

  var n = dojo.create("div");

Create a <div> with content:

.. code-block :: javascript
  :linenos:

  var n = dojo.create("div", { innerHTML: "<p>hi</p>" });

Append a new <div> to <body> with no attributes:

.. code-block :: javascript
  :linenos:

  var n = dojo.create("div", null, dojo.body());

Place a new <div> as the first child of <body> with no attributes:

.. code-block :: javascript
  :linenos:

  var n = dojo.create("div", null, dojo.body(), "first");

Decorate and place an existing node:

.. code-block :: javascript
  :linenos:

  dojo.create(node, { style: { color: "red" } }, dojo.body());

Create an <ul>, and populate it with <li>'s. Place the list as the first child of a node with id equals "someId":

.. code-block :: javascript
  :linenos:

  var ul = dojo.create("ul", null, "someId", "first");
  var items = ["one", "two", "three", "four"];
  dojo.forEach(items, function(data){
    dojo.create("li", { innerHTML: data }, ul);
  });

Create an anchor, with an href. Place in <body>:

.. code-block :: javascript
  :linenos:

  dojo.create("a", { href: "foo.html", title: "Goto FOO!", innerHTML: "link" }, dojo.body());

============
Alternatives
============

Creating and/or placing with dojo.place()
-----------------------------------------

In some cases it is easier to create a node from an HTML fragment and place it, without applying any attributes, or specifying them as a part of the HTML fragment. If this is the case consider :ref:`dojo.place <dojo/place>`:

.. code-block :: javascript
  :linenos:

  // duplicating the next line with dojo.place()
  //dojo.create("a", { href: "foo.html", title: "Goto FOO!", innerHTML: "link" }, dojo.body());
  dojo.place("<a href='foo.html' title='Goto FOO!'>link</a>", dojo.body()

  // duplicating the next line with dojo.place()
  //var n = dojo.create("div", null, dojo.body());
  var n = dojo.place("<div></div>", dojo.body());

Setting attributes with dojo.attr()
-----------------------------------

While you can use :ref:`dojo.create <dojo/create>` to set attributes on the existing node without placing it, using :ref:`dojo.attr <dojo/attr>` is recommended:

.. code-block :: javascript
  :linenos:

  // duplicating the next line with dojo.attr()
  //var n = dojo.create(node, { innerHTML: "<p>hi</p>" });
  var n = dojo.attr(node, { innerHTML: "<p>hi</p>" });

========
See also
========

* :ref:`dojo.destroy <dojo/destroy>`
* :ref:`dojo.empty <dojo/empty>`
* :ref:`DOM Utilities <quickstart/dom>`
