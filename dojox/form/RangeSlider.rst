.. _dojox/form/RangeSlider:

dojox.form.RangeSlider
======================

:Status: Draft
:Version: 1.0
:Authors: ?--
:Project owner: ?--
:Available: since V?

.. contents::
   :depth: 2

The RangeSlider is a descendant of :ref:`dijit.form.Slider <dijit/form/Slider>` that allows a selection of a range of values.

============
Introduction
============

The RangeSlider differs from the :ref:`dijit.form.Slider <dijit/form/Slider>` by providing two handles that allow you to select a range of values across the scale.  There is the **dojox.form.HorizontalRangeSlider** and the **dojox.form.VerticalRangeSlider** which provide a horizontal and vertical version respectively.


=====
Usage
=====

The RangeSlider is used in the same fashion as most dijit Form Widgets.

.. code-block :: javascript
 :linenos:

 <script type="text/javascript">
   dojo.require("dojox.form.RangeSlider");
   var rangeSlider = new dojox.form.HorizontalRangeSlider({
   },"myRangeSlider");
 </script>

Here are some of the constructor parameters:

===================  ====================  =============================================================================
Parameter            Type                  Description
===================  ====================  =============================================================================
value                array                 Initial values of the slider (``[0,100]``)
showButtons          boolean               Whether to show or not buttons at each end of the slider (``true``)
minimum              integer               Minimum value of the slider (``0``)
maximum              integer               Maximum value of the slider (``100``)
intermediateChanges  boolean               If fractional parts between steps are reported (``false``)
discreteValues       integer               Number of "steps" in the slider. For example if ``discreteValues`` is ``3``, you'll have 3 steps: ``minimum``, ``maximum`` and a value in the middle
===================  ====================  =============================================================================

========
Examples
========

Programmatic horizontal example
-------------------------------
.. cv-compound::

  .. cv:: javascript

    <script type="text/javascript">
      dojo.require("dojox.form.RangeSlider");

      dojo.addOnLoad(function(){

        var rangeSlider = new dojox.form.HorizontalRangeSlider({
          name: "rangeSlider",
          value: [2,6],
          minimum: -10,
          maximum: 10,
          intermediateChanges: true,
          style: "width:300px;",
          onChange: function(value){
            dojo.byId("sliderValue").value = value;
          }
        }, "rangeSlider");
      });
    </script>

  .. cv:: html

    <div id="rangeSlider"></div>
    <p><input type="text" id="sliderValue" /></p>

  .. cv:: css

    <style type="text/css">
      @import url({{baseUrl}}dojox/form/resources/RangeSlider.css);
    </style>

Programmatic vertical example with rulers
-----------------------------------------
.. cv-compound::

  .. cv:: javascript

    <script type="text/javascript">
      dojo.require("dojox.form.RangeSlider");
      dojo.require("dijit.form.VerticalRule");

      dojo.addOnLoad(function(){
        var vertical = dojo.byId("vertical");
        var rulesNode = document.createElement("div");
        vertical.appendChild(rulesNode);
        var sliderRules = new dijit.form.VerticalRule({
            count:11,
            style:"width:5px;"
        }, rulesNode);
        var slider = new dojox.form.VerticalRangeSlider({
          name: "vertical",
          value: [2,6],
          minimum: -10,
          maximum: 10,
          intermediateChanges: true,
          style: "height:300px;"
        }, vertical);
      });
    </script>

  .. cv:: html

    <div id="vertical"></div>

  .. cv:: css

    <style type="text/css">
      @import url({{baseUrl}}dojox/form/resources/RangeSlider.css);
    </style>

Declarative horizontal example
------------------------------

.. cv-compound::

  .. cv:: javascript

    <script type="text/javascript">
      dojo.require("dojox.form.RangeSlider");
    </script>

  .. cv:: html

    <div id="rangeSlider" dojoType="dojox.form.HorizontalRangeSlider"
        value="2,6" minimum="-10" maximum="10" intermediateChanges="true"
        showButtons="false" style="width:300px;">
        <script type="dojo/method" event="onChange" args="value">
            dojo.byId("sliderValue").value = value;
        </script>
    </div>
    <p><input type="text" id="sliderValue" /></p>

  .. cv:: css

    <style type="text/css">
      @import url({{baseUrl}}dojox/form/resources/RangeSlider.css);
    </style>

**NOTE** In delarative mode, the value of the attribute ``value`` is specified as a comma delimited string and not as an array (e.g. ``value="2,6"`` and not ``value="[2,6]"``.

=============
Accessibility
=============

TODO: provide accessibility information

========
See also
========

* See :ref:`dijit.form.Slider <dijit/form/Slider>` for more information.
