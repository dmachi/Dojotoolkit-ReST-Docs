.. _dojo/extend:

dojo.extend
-----------

Dojo extend works much like :ref:`dojo.mixin <dojo/mixin>`, though works directly on an object's prototype. Following the same pattern as mixin, dojo.extend mixes members from the right-most object into the first object, modifying the object directly.

We can use extend to extend functionality into existing classes. Consider the following:

.. code-block :: javascript
  :linenos:

  dojo.require("dijit.TitlePane");
  dojo.extend(dijit.TitlePane, {
      randomAttribute:"value"
  }); 

The way the :ref:`dojo.parser <dojo/parser>` works, a custom attribute on the node will be recognized, as in the interest of performance, only declared members are mixed as part of the parsing process. Before the above dojo.extend() call, this sample would not recognize the follow markup:

.. code-block :: html
  :linenos:
  
     <div dojoType="dijit.TitlePane" randomAttribute="newValue"></div>

After the extend, any new instances of a TitlePane will have the 'randomAttribute' member mixed into the instance. dojo.extend affects all future instances of a Class (or rather, any object with a .prototype). 

Extending _Widget
-----------------

A potentially confusing result of the above actually provides us a lot of flexibility. All Dijit widgets inherit from `dijit._Widget` in one way or another. Some widgets, like the :ref:`BorderContainer <dijit/layout/BorderContainer>` can contain arbitrary widgets, though require a 'region' parameter on the contained widget, though rather than manually adding a "region" parameter to each declaration across Dijit, BorderContainer simply extends _Widget with the member, and anyone using any widget within a BorderContainer can specitiy a region:

.. code-block :: javascript
  :linenos:

  dojo.extend(dijit._Widget, {
      region:"center"
  });

The side-effect of this is a documentation nightmare. Now ``every`` Dijit appears to have a region variable, when in fact it is just there for the benefit of BorderContainer. 
