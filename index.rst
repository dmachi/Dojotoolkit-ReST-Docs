.. _manual/index:

Dojo Toolkit Reference Guide
============================

.. contents::
   :depth: 2


=====
Dojo
=====

* :ref:`dojo.js <dojo/index>`

  The base functionality of the Dojo Toolkit, provided by just including ``dojo.js``. This includes tons of features like CSS-based queries, event handling, animations, Ajax, class-based programming, and a package system that makes getting access taso the rest of Dojo a snap.testasdfasdfasdfasf
tasdf
* :ref:`Dojo Core <dojo/index>`

  Additional stable (but optional) components for advanced animations, I/O, data, Drag and Drop and much more.

=====
Dijit
=====

Dijit is Dojo's themeable, accessible, easy-to-customize UI Library. There are many widgets to choose from, so be sure to check out the :ref:`quickstart <quickstart/index>` guide which covers the basics. Dijit requires ``dojo.js`` and other Core modules. 

* :ref:`Dijit Reference <dijit/index>`

=====
DojoX
=====

DojoX stands for Dojo eXtensions and contains many useful sub-projects and varying states of maturity -- from very stable and robust, to alpha and experimental. All DojoX projects contain README files that outline their maturity and authorship, so be sure to check those along with the documentation pages to get the full picture of where a module is headed.

* :ref:`DojoX Reference <dojox/index>`


=====
DojoC
=====

DojoC is an external svn repository used by DojoCampus for a variety of widgets, tutorials, sandbox, and other demos. You are welcome to explore and contribute, though absolutely nothing is guaranteed to work. DojoC is meant as a community workshop, and code comes and goes frequently, often times 'promoted' to :ref:`DojoX projects <dojox>`.a

* :ref:`More about DojoC <dojoc/index>`


================
Dojo Styleguides
================

* :ref:`Styleguides <styleguides/index>`


===========================
Utilities and Miscellaneous
===========================

:ref:`Dojo Build System <build/index>`
---------------------------------------

The Dojo build system is used to create efficient optimized packages of JavaScript and CSS, customized for a given application or website. You can take advantage of this powerful tool to help structure your development and speed up your applications.

:ref:`The Dojo API Doc System <util/doctools>`
-----------------------------------------------

Dojo uses a custom inline comment syntax which produces well structured xml, and powers the official `API Docs <http://api.dojocampus.org/>`_ . 

:ref:`ShrinkSafe <shrinksafe/index>`
-------------------------------------

A standalone utility for compressing JavaScript, used by the Dojo Build System as an optional compression step, though can be used on individual files manually.


:ref:`Checkstyle <util/checkstyle>`
-------------------------------------

A standalone utility for checking JavaScript files for violations of the Dojo style guide. Also includes an online tool for automatically fixing the majority of style guide violations.
