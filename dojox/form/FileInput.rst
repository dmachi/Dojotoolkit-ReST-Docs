.. _dojox/form/FileInput:

dojox.form.FileInput
====================

:Status: Draft
:Version: 1.2
:Project owner: Peter Higgins
:Available: since V1.3

.. contents::
   :depth: 2

The FileInput class provides a foundation for a series of FileInput widgets: FileInput, FileInputAuto and FileInputBlind. 

Unless you have a clinical aversion to Flash, it is recommended you use the newer :ref:`FileUploader <dojox/form/FileUploader>` and provided in the DojoX Form project.

============
Introduction
============

There are three widgets available here. You must issue a require() call for the specific type you desire:

.. code-block :: javascript
  :linenos:

  dojo.require("dojox.form.FileInputAuto");
  // or just
  dojo.require("dojox.form.FileInput");

There is also a required CSS file. All three widgets share a single sheet of rules:

.. code-block :: html
  :linenos:

    <link rel="stylesheet" href="$dojo/dojox/form/resources/FileInput.css" />

The three types are described as:

* Base - A plain input type="file" widget, intended to match Dijit theme styles and fit into regular forms.
* Auto - An extension on base FileInput which will submit the file after a period of time after selection, giving the user a moment to cancel if necessary. 
* Blind - An extension on FileInputAuto which removes the input functionality, and provides only a button to trigger the file selection dialog.

========
See also
========

* :ref:`FileUploader <dojox/form/FileUploader>`
* :ref:`IFrame IO <dojox/io/frame>`
