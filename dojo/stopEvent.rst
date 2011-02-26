.. _dojo/stopEvent:

dojo.stopEvent
==============

`FIXME`: inline API information

The same as calling ``event.preventDefault()`` and ``event.stopPropagation()`` on some passed event:

.. code-block :: javascript

   dojo.connect(node, "onclick", function(e){
       dojo.stopEvent(e); // click does nothing
   });

See Also:
=========

* :ref:`The event QuickStart <quickstart/events>`
