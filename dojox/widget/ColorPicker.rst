.. _dojox/widget/ColorPicker:

dojox.widget.ColorPicker
========================

:Status: Draft
:Version: 1.0
:Project owner: Nathan Toone
:Available: since V1.1

.. contents::
   :depth: 2

A Photoshop-like HSV ColorPicker widget. 

============
Introduction
============

This ColorPicker adds many options to the standard :ref:`ColorPallete <dijit/ColorPalette>` available in Dijit. 

=====
Usage
=====

To use the ColorPicker you need to first load the CSS:

.. code-block :: html
  :linenos:
  
    <link rel="stylesheet" href="dojotoolkit/dojox/widget/ColorPicker/ColorPicker.css" />

And Require the module:

.. code-block :: javascript
  :linenos:

  dojo.require("dojox.widget.ColorPicker");


========
Examples
========

Programmatic example
--------------------

.. code-example::

  .. css::

    <link rel="stylesheet" href="{{ dataUrl }}/dojo/dojox/widget/ColorPicker/ColorPicker.css" />

  .. javascript::

    <script type="text/javascript">
        dojo.require("dojox.widget.ColorPicker");
        dojo.addOnLoad(function() {
            var c = new dojox.widget.ColorPicker({},"picker1");
        });
    </script>

  .. html::

    <div id="picker1"></div>


Declarative example
-------------------

.. code-example::

  .. css::

    <link rel="stylesheet" href="{{ dataUrl }}/dojo/dojox/widget/ColorPicker/ColorPicker.css" />

  .. javascript::

    <script type="text/javascript">
        dojo.require("dojox.widget.ColorPicker");
    </script>

  .. html::

    <div dojoType="dojox.widget.ColorPicker" id="picker2"></div>


A ColorPicker within a DropDownButton
-------------------------------------

.. code-example::

  .. css::

    <link rel="stylesheet" href="{{ dataUrl }}/dojo/dojox/widget/ColorPicker/ColorPicker.css" />

  .. javascript::

    <script type="text/javascript">
        dojo.require("dojox.widget.ColorPicker");
        dojo.require("dijit.form.DropDownButton");
    </script>

  .. html::

    <div dojoType="dijit.form.DropDownButton">
        <span>
            ColorPicker
        </span>
        <div dojoType="dojox.widget.ColorPicker" id="picker3"></div>
    </div>


=====
Notes
=====

``NOTE:`` as of Dojo 1.2, The ColorPicker is experimental, and doesn't implement the Dijit .attr() method for getting and setting values. 
