.. _dojox/NodeList/delegate:

dojox.NodeList.delegate
=======================

.. contents::
    :depth: 2

:author: Bill 
:since: 1.6.0

========
Overview
========

A module providing an event delegation API to :ref:`dojo.NodeList <dojo/NodeList>`

Delegate() monitors nodes in this NodeList for [bubbled] events on nodes that match a given selector.

This allows an app to setup a single event handler on a high level node, rather than many
event handlers on subnodes. For example, to monitor clicks on any <a> in your navigation section:

.. code-block :: javascript
  :linenos:

  dojo.query("#navbar").delegate("a", "onclick", function(evt){ ... } )

Since setting up many event handlers is expensive, using delegate() can increase performance.

Also, since the handler (in this example) is on navbar, it will report clicks on any <a> nodes added to navbar in the future
(in addition to the <a> nodes in navbar at the time of the delegate() call).

Note that delegate() will not work for events that don't bubble, like focus.
onmouseenter/onmouseleave also don't currently work.


=======
Example
=======

.. code-example::
  :type: inline
  :version: 1.6

  .. html::
    :label: a navigation bar

    <div id="navbar">
        <a href="#" id="home">home</a> &nbsp;
        <a href="#" id="mail">mail</a> &nbsp;
        <a href="#" id="calendar">calendar</a> &nbsp;
    </div>

  .. javascript::
    :label: dojo.delegate in action

    <script type="text/javascript">
    dojo.require("dojox.NodeList.delegate");
    dojo.addOnLoad(function(){
      dojo.query("#navbar").delegate("a", "onclick", function(evt){
          alert("clicked " + dojo.attr(this, "id"));
          dojo.stopEvent(evt);
      });
    });
    </script>

========
See Also
========

* :ref:`dojo.NodeList <dojo/NodeList>`
* :ref:`dojox.NodeList <dojox/NodeList>`
