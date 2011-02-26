.. _dijit/layout/BorderContainer:

dijit.layout.BorderContainer
============================

:Authors: Becky Gibson, Bill Keese, Nikolai Onken, Marcus Reimann
:Developers: ?-
:Available: since V.2

.. contents::
    :depth: 2

This widget is a container partitioned into up to five regions: left (or leading), right (or trailing), top, and bottom with a mandatory center to fill in any remaining space. Each edge region may have an optional splitter user interface for manual resizing. 


=====
Usage
=====

Regions
-------

Each child element must have an attribute "region" which indicates where it should be positioned (most names are self explainatory):

  * top
  * bottom
  * right
  * left
  * center
  * leading: used have flexible layout in left-to-right/right-to-left environments. In ltr, it will be equivalent to left, in rtl equivalent to right
  * trailing: opposite of 'leading': right in ltr, left in rtl

There can be multiple widgets for each region, in which case their order (i.e. closeness to the edge of the BorderContainer) is controlled by their relative layoutPriority settings.

There must always be one region marked 'center'.

Setting sizes
-------------
Sizes are specified for the edge regions in pixels or percentage using CSS -- height for top and bottom and width for the sides. You might specify a top region of height:100px and a left region of width:50%. The center must not have any dimensions specified in CSS as it resizes automatically to fill the remaining space.  Do not set the width of the top/bottom panes or the height of the left/right panes as that would be meaningless.

Besides setting the size of the BorderContainer itself, you generally need to set the width of the leading and trailing (left and the right) panes.
You shouldn't need to set the height of the top/bottom panes as that can be determined automatically.


``note:`` In order to set the overall size of a BorderContainer to the full size of the viewport, the `<body>` element needs an explicit size set as well as a size on the BorderContainer itself:

.. code-block :: html
  :linenos:

    <style type="text/css">
    body, html { width:100%; height:100%; margin:0; padding:0; overflow:hidden; } 
    #borderContainer { width:100%; height:100% } 
    </style>

Otherwise, the computed style of the BorderContainer will report 0 rather than the browser-calculated size of the viewport.

Layout modes
------------

BorderContainer operates in a choice of two layout modes: the design attribute may be set to "headline" (by default) or "sidebar". With the "headline" layout, the top and bottom sections extend the entire width of the box and the remaining regions are placed in the middle. With the "sidebar" layout, the side panels take priority, extending the full height of the box.

However, the layoutPriority setting for child panes overrides the design attribute on the BorderContainer.   In other words, if the top and bottom sections have a lower layoutPriority then the left and right panes then the top and bottom panes will extend the entire width of the box.
 
========
Examples
========

Declarative example
-------------------

.. code-example::
  :type: inline
  :height: 400
  :width: 660

  Lets specify a simple BorderContainer with a left and center region

  .. javascript::

    <script type="text/javascript">
      dojo.require("dijit.layout.ContentPane");
      dojo.require("dijit.layout.BorderContainer");
    </script>

  The markup has to look as follows
  
  .. html::
    :label: A dijit button
    
    <div dojoType="dijit.layout.BorderContainer" design="sidebar" gutters="true" liveSplitters="true" id="borderContainer">
      <div dojoType="dijit.layout.ContentPane" splitter="true" region="leading" style="width: 100px;">Hi</div>
      <div dojoType="dijit.layout.ContentPane" splitter="true" region="center">Hi, I'm center</div>
    </div>
  
  .. css::
    :label: A simple set of css rules

    <style type="text/css">
      html, body {
        width: 100%;
        height: 100%;
        margin: 0;
        overflow:hidden;
      }

      #borderContainer {
        width: 100%;
        height: 100%;
      }
    </style>


Using layoutPriority
--------------------

This example uses layoutPriority to include two left panes in one BorderContainer:

.. code-example::
  :type: inline
  :height: 400
  :width: 660
  :version: 1.6

  .. javascript::

    <script type="text/javascript">
      dojo.require("dijit.layout.ContentPane");
      dojo.require("dijit.layout.BorderContainer");
    </script>
  
  .. html::
    
    <div dojoType="dijit.layout.BorderContainer" design="sidebar" gutters="true" liveSplitters="true" id="layoutPriorityBorderContainer">
      <div dojoType="dijit.layout.ContentPane" splitter="true" region="leading" layoutPriority="1" style="width: 100px;">Left #1</div>
      <div dojoType="dijit.layout.ContentPane" splitter="true" region="leading" layoutPriority="2" style="width: 100px;">Left #2</div>
      <div dojoType="dijit.layout.ContentPane" splitter="true" region="center">Hi, I'm center</div>
    </div>
  
  .. css::
 
    <style type="text/css">
      html, body {
        width: 100%;
        height: 100%;
        margin: 0;
        overflow:hidden;
      }

      #layoutPriorityBorderContainer {
        width: 100%;
        height: 100%;
      }
    </style>


Nested Layout Widgets
---------------------

Lets take a look at a more advanced example of using BorderContainer and other layout widgets.
This example uses two BorderContainers to allow to, left and right content areas. 
Note the tabStrip attribute on the TabContainer.

.. code-example::
  :djConfig: parseOnLoad: true
  :type: inline
  :height: 400
  :width: 660

  .. javascript::
    :label: The dojo requires

    <script type="text/javascript">
      dojo.require("dijit.layout.ContentPane");
      dojo.require("dijit.layout.BorderContainer");
      dojo.require("dijit.layout.TabContainer");
      dojo.require("dijit.layout.AccordionContainer");
    </script>

  The markup has to look as follows
  
  .. html::
    :label: The markup

    <div dojoType="dijit.layout.BorderContainer" gutters="true" id="borderContainerTwo" liveSplitters="false">
      <div dojoType="dijit.layout.ContentPane" region="top" splitter="false">
        Hi, usually here you would have important information, maybe your company logo, or functions you need to access all the time..  
      </div>	
      <div dojoType="dijit.layout.AccordionContainer" minSize="20" style="width: 300px;" id="leftAccordion" region="leading" splitter="true">
          <div dojoType="dijit.layout.AccordionPane" title="One fancy Pane">
          </div>
          <div dojoType="dijit.layout.AccordionPane" title="Another one">
          </div>
          <div dojoType="dijit.layout.AccordionPane" title="Even more fancy" selected="true">
          </div>
          <div dojoType="dijit.layout.AccordionPane" title="Last, but not least">
          </div>
      </div><!-- end AccordionContainer -->
      <div dojoType="dijit.layout.TabContainer" region="center" tabStrip="true">
          <div dojoType="dijit.layout.ContentPane" title="My first tab" selected="true">
            Lorem ipsum and all around...
          </div>
          <div dojoType="dijit.layout.ContentPane" title="My second tab">
            Lorem ipsum and all around - second...
          </div>
          <div dojoType="dijit.layout.ContentPane" title="My last tab" closable="true">
            Lorem ipsum and all around - last...
          </div>
      </div><!-- end TabContainer -->
    </div><!-- end BorderContainer -->

  .. css::
    :label: A few simple css rules

    <style type="text/css">
      html, body {
        width: 100%;
        height: 100%;
        margin: 0;
        overflow:hidden;
      }

      #borderContainerTwo {
        width: 100%;
        height: 100%;
      }
    </style>


BorderContainer Inside A Dijit Template
---------------------------------------

You can use a BorderContainer inside your own dijit template with a bit of care to call startup() on your dijit after it has been added to the DOM, so that its contained BorderContainer can lay itself out.

.. code-example::
  :djConfig: parseOnLoad: true
  :height: 400
  :width: 660
  :version: 1.5

  .. javascript::
    :label: The dojo requires

    <script type="text/javascript">
        dojo.require("dijit.layout.BorderContainer");
        dojo.require("dijit.layout.ContentPane");
        dojo.require("dijit.form.Button");

        dojo.addOnLoad(function() {
            dojo.declare("MyDijit",
                [dijit._Widget, dijit._Templated], {
                    widgetsInTemplate: true,
                    // Note: would be a call to dojo.cache() in a 'proper' dijit
                    templateString: '<div style="width: 100%; height: 100%;">' +
                        '<div dojoType="dijit.layout.BorderContainer" design="headline" ' +
                        '  style="width: 100%; height: 100%;" dojoAttachPoint="outerBC">' +
                        '<div dojoType="dijit.layout.ContentPane" region="center">MyDijit - Center content goes here.</div>' +
                        '<div dojoType="dijit.layout.ContentPane" region="bottom">MyDijit - Bottom : ' +
                        ' <div dojoType="dijit.form.Button">A Button</div>' +
                        '</div>' +
                        '</div></div>'
            });
            // it's now safe to allow creation of our dijit instance
            dijit.byId('createButton').attr('disabled', false);
        });
    </script>

  The markup has to look as follows
  
  .. html::
    :label: The markup

    <div dojoType="dijit.layout.BorderContainer" gutters="true" id="borderContainerThree" >
      <div dojoType="dijit.layout.ContentPane" region="top">
        <div dojoType="dijit.form.Button" id="createButton" disabled="true">Create Inner Dijit
          <script type="dojo/connect" event="onClick">
            // Create a new instance
            var newdijit = new MyDijit( {}, dojo.create('DIV'));
            newdijit.placeAt(dojo.byId('mydijitDestination'));
            newdijit.startup();
          </script>
        </div>
      </div>
      <div dojoType="dijit.layout.ContentPane" region="left" splitter="false">
        OUTER LEFT<br/>
        This is my content.<br/>
        There is much like it,<br/>
        but this is mine.<br/>
        My content is my best friend.<br/>
        It is my life.<br/>
        I must master it,<br/>
        as I must master my life.
      </div>
      <div dojoType="dijit.layout.ContentPane" region="center" splitter="false">
        <div id="mydijitDestination" style="width: 100%; height: 100%"></div>
      </div>
    </div>

  .. css::
    :label: A few simple css rules

    <style type="text/css">
      html, body {
        width: 100%;
        height: 100%;
        margin: 0;
      }

      #borderContainerThree {
        width: 100%;
        height: 100%;
        overflow:hidden;
        border: none;
      }
    </style>

=============
Accessibility
=============

Keyboard
--------

===========================================    =================================================
Action                                         Key
===========================================    =================================================
Navigate to splitters for resizable regions    tab - all resizable splitters are in the tab order
Change the size of a vertical region           left / right arrows to decrease and increase 
Change the size of a horizontal region         down / up arrows to decrease and increase
===========================================    =================================================

Note: The children of BorderContainer must be created in the source code in their natural tab order. Header regions should be first and footer regions last. In Left to right locales, left regions before center and right ones.
