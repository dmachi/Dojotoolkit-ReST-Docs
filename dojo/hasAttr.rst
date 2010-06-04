.. _dojo/hasAttr:

dojo.hasAttr
============

:Available: since V1.2

.. contents::
   :depth: 2

Checks a node for the presence of an attribute.


============
Introduction
============

``dojo.hasAttr()`` is a companion function for :ref:`dojo.attr <dojo/attr>`. It checks if an attribute is present on a DOM node, and returns the truthy value if it is there, and falsy value otherwise.

Since 1.4 it will return true for standard properties that can't have a corresponding attribute, e.g., ``innerHTML`` or ``class``.


=====
Usage
=====

.. code-block :: javascript
 :linenos:

 result = dojo.hasAttr(node, attr);

node
  id or reference of the DOM node to get/set style for

attr
  the attribute property name.

result
  truthy, if the attribute is present, falsy otherwise


========
Examples
========

Testing for attributes
----------------------

The following example will check for several attributes.

.. cv-compound::

  .. cv:: javascript

    <script type="text/javascript">
      function checkAttributes(){
        showAttribute("id");
        showAttribute("type");
        showAttribute("name");
        showAttribute("innerHTML");
        showAttribute("foo");
        showAttribute("baz");
      }
      function showAttribute(name){
        var result = dojo.hasAttr("model", name);
        // I don't use dojo.create() here because it was not available in 1.2
        var wrapper = dojo.doc.createElement("div");
        dojo.place(wrapper, "out");
        wrapper.innerHTML = "<input type='checkbox' disabled='disabled' " +
          (result ? "checked='checked'" : "") + "> has " + name;
      }
    </script>

  .. cv:: html

    <p><input id="model" name="model" baz="foo"> &mdash; our model node</p>
    <p><button onclick="checkAttributes();">Check attributes</button></p>
    <p id="out"></p>

========
See also
========

DOM operations:

* :ref:`dojo.attr <dojo/attr>`
* :ref:`dojo.getNodeProp <dojo/getNodeProp>`
* :ref:`dojo.removeAttr <dojo/attr>`
* :ref:`dojo.style <dojo/style>`

NodeList:

* :ref:`dojo.NodeList <dojo/NodeList>`
* :ref:`dojo.NodeList.attr <dojo/NodeList/attr>`
* :ref:`dojo.NodeList.removeAttr <dojo/NodeList/removeAttr>`

External links:

* `DOM Attributes and The Dojo Toolkit 1.2 <http://www.sitepen.com/blog/2008/10/23/dom-attributes-and-the-dojo-toolkit-12/>`_
