.. _dijit/layout/AccordionContainer:

dijit.layout.AccordionContainer
===============================

:Authors: Becky Gibson, Nikolai Onken
:Developers: ?-
:Available: since V?
:jsDoc: http://api.dojotoolkit.org/jsdoc/HEAD/dijit.layout.AccordionContainer

.. contents::
    :depth: 2

Like :ref:`StackContainer <dijit/layout/StackContainer>` and :ref:`TabContainer <dijit/layout/TabContainer>`, an **AccordionContainer** holds a set of panes whose titles are all visible, but only one pane's content is visible at a time. Clicking on a pane title slides the currently-displayed one away, similar to a garage door. Users can explicitly select the pane that is to be made visible when the widget is loaded. If it is not specified, then the first pane is taken by default.


========
Examples
========

In the example below, second pane is selected when the widget is loaded.

Programmatic example
--------------------
 
.. cv-compound::

  .. cv:: javascript

    <script type="text/javascript">
	dojo.require("dijit.layout.AccordionContainer");
	dojo.require("dijit.layout.ContentPane");

	dojo.addOnLoad(function(){
	  var aContainer = new dijit.layout.AccordionContainer({style:"height: 300px"}, "markup");

	  aContainer.addChild(new dijit.layout.ContentPane({
				title:"This is a content pane", 
				content:"Hi!"
	  }));
	  aContainer.addChild(new dijit.layout.ContentPane({
				title:"This is as well", 
				content:"Hi how are you?"
          }));
	  aContainer.addChild(new dijit.layout.ContentPane({
				title:"This too", 
				content:"Hello im fine.. thnx"
	  }));
	  aContainer.startup();
      });
    </script>

  .. cv:: html

     <div id="markup" style="width:300px; height: 300px"></div>
  

Declarative example
-------------------

.. cv-compound::

  .. cv:: javascript

    <script type="text/javascript">
    dojo.require("dijit.layout.AccordionContainer");
    </script>

  .. cv:: html

    <div style="width: 300px; height: 300px">
      <div dojoType="dijit.layout.AccordionContainer" style="height: 300px;">
        <div dojoType="dijit.layout.ContentPane" title="Heeh, this is a content pane">
        Hi!
        </div>
        <div dojoType="dijit.layout.ContentPane" title="This is as well" selected="true">
        Hi how are you?
        </div>
        <div dojoType="dijit.layout.ContentPane" title="This too">
        Hi how are you? .....Great, thx
        </div>
      </div>
    </div>


=============
Accessibility
=============

Keyboard
--------

==========================================    =================================================
Action                                        Key
==========================================    =================================================
Navigate to next title                        Right or down arrow
Navigate to previous title                    Left or up arrow
Navigate into page                            Tab
Navigate to next page                         Ctrl + page down, ctrl + tab (except IE7)
Navigate to previous page                     Ctrl + page up
==========================================    =================================================
