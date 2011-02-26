.. _dojox/editor/plugins/PasteFromWord:

dojox.editor.plugins.PasteFromWord
==================================

:Project owner: Jared Jurkiewicz
:Authors: Jared Jurkiewicz
:Available: since V1.5

.. contents::
    :depth: 2

Have you ever pasted in content from Microsoft Word or similar programs into the dijit.Eidtor and found that it tended to paste in really unpleasant HTML as well as a lot of bogus and bad tags?  Is so, then this plugin is intended for you

This plugin provides a new toolbar button that opens a dialog where you can paste in content from word processors like Word.  Filters are then applied to the content to strip out a lot of the extraneous HTML, classes and other nonsense that make the pasted content poor to work with.  The cleaned up content can then be pasted into the dijit.Editor much more safely

========
Features
========

This plugin provides the following

* A button and icon representing paste from Word.
* An input dialog for pasting content from Word Processors for processing before being injected into the dijit Editor.
* It makes use of dojox.html.format to also try to clean up and normalize the HTML as much as possible.

=====
Usage
=====

Basic Usage
-----------
Usage of this plugin is quite simple and painless.  The first thing you need to do is require into the page you're using the editor.  This is done in the same spot all your dojo.require calls are made, usually a head script tag.  For example:

.. code-block :: javascript
 
    dojo.require("dijit.Editor");
    dojo.require("dojox.editor.plugins.PasteFromWord");

You then need to import its CSS.  This is done by just adding a link tag to the header.  Something like:

.. code-block :: html

  <style>
    @import "dojox/editor/plugins/resources/css/PasteFromWord.css";
  </style>

Once it has been required in, all you have to do is include it in the list of extraPlugins you want to load into the editor.  For example:

.. code-block :: html

  <div dojoType="dijit.Editor" id="editor" extraPlugins="['pastefromword']"></div>

And that's it.  The editor instance you can reference by 'dijit.byId("editor")' is now enabled with the PasteFromWord plugin!

Configuring PasteFromWord
-------------------------

The PasteFromWordplugin supports two options, the width and height to use for the input area in the dialog.  This allows users to customize how large an area is used for content input.

+-----------------------------------+---------------------------------------------------------------------+------------------------+
| **option**                        | **Description**                                                     | **Required**           |
+-----------------------------------+---------------------------------------------------------------------+------------------------+
| width                             |String defining how wide to make the input.   The default is 400px.  | No                     |
+-----------------------------------+---------------------------------------------------------------------+------------------------+
| height                            |String defining how tall to make the input.   The default is 300px.  | No                     |
+-----------------------------------+---------------------------------------------------------------------+------------------------+

How do I configure the options?  Glad you asked.  You do it where you declare the plugin.  See the following example, the input area is configured to be 200px wide and 200px tall

.. code-block :: html

  <div dojoType="dijit.Editor" 
       id="editor" extraPlugins="[{name: 'pastefromword', width: "200px", height: "200px"}]">
  </div>


========
Examples
========

Basic Usage
-----------

.. code-example::
  :djConfig: parseOnLoad: true
  :version: 1.4

  .. javascript::

    <script>
      dojo.require("dijit.Editor");
      dojo.require("dojox.editor.plugins.PasteFromWord");
    </script>

  .. css::

    <style>
      @import "{{baseUrl}}dojox/editor/plugins/resources/css/PasteFromWord.css";
    </style>
    
  .. html::


  .. html::

    <b>Clear the editor, click paste from word, then paste in content you want!</b>
    <br>
    <div dojoType="dijit.Editor" height="100px"id="input" extraPlugins="['pastefromword']">
    <div>
    <br>
    blah blah & blah!
    <br>
    </div>
    <br>
    <table>
    <tbody>
    <tr>
    <td style="border-style:solid; border-width: 2px; border-color: gray;">One cell</td>
    <td style="border-style:solid; border-width: 2px; border-color: gray;">
    Two cell
    </td>
    </tr>
    </tbody>
    </table>
    <ul> 
    <li>item one</li>
    <li>
    item two
    </li>
    </ul>
    </div>


========
See Also
========

* :ref:`dijit.Editor <dijit/Editor>`
* :ref:`dijit._editor.plugins <dijit/_editor/plugins>`
* :ref:`dojox.editor.plugins <dojox/editor/plugins>`
