.. _dojox/grid/DataGrid:

dojox.grid.DataGrid
===================

:Status: Draft
:Version: 1.0
:Project owners: Scott J. Miles, Steve Orvell, Bryan Forbes
:Available: since V1.2
:jsDoc: http://api.dojotoolkit.org/jsdoc/HEAD/dojox.grid.DataGrid

.. contents::
   :depth: 2

A visual grid/table much like a spreadsheet.

============
Introduction
============

Grids are familiar in the client/server development world. Basically a grid is a kind of mini spreadsheet, commonly used to display details on master-detail forms. From HTML terms, a grid is a "super-table" with its own scrollable viewport.


.. code-example::
  :toolbar: themes, versions, dir
  :version: local
  :width: 480
  :height: 300

  .. javascript::

    <script type="text/javascript">
        dojo.require("dojox.grid.DataGrid");
        dojo.require("dojox.data.CsvStore");
    
        dojo.addOnLoad(function(){
          // our test data store for this example:
          var store4 = new dojox.data.CsvStore({ url: '{{ dataUrl }}dojox/grid/tests/support/movies.csv' });

          // set the layout structure:
          var layout4 = [
              { field: 'Title', name: 'Title of Movie', width: '200px' },
              { field: 'Year', name: 'Year', width: '50px' },
              { field: 'Producer', name: 'Producer', width: 'auto' }
          ];

          // create a new grid:
          var grid4 = new dojox.grid.DataGrid({
              query: { Title: '*' },
              store: store4,
              clientSort: true,
              rowSelector: '20px',
              structure: layout4
          }, document.createElement('div'));

          // append the new grid to the div "gridContainer4":
          dojo.byId("gridContainer4").appendChild(grid4.domNode);

          // Call startup, in order to render the grid:
          grid4.startup();
        });
    </script>

  .. html::

    <div id="gridContainer4" style="width: 100%; height: 100%;"></div>

  .. css::

    <style type="text/css">
        @import "{{ baseUrl }}dojox/grid/resources/Grid.css";
        @import "{{ baseUrl }}dojox/grid/resources/{{ theme }}Grid.css";

        .dojoxGrid table {
            margin: 0;
        }

        html, body {
            width: 100%;
            height: 100%;
            margin: 0;
        }
    </style>

A structure is an array of views and a view is an array of cells.

This Widget inherits from dojo.grid._Grid and hence all methods and properties pertaining to that Widget also apply here.


=====
Usage
=====

At a high level, a DataGrid can be defined either declaratively in HTML markup or programatically in JavaScript.  In markup, the following high level structure is defined:

.. code-block :: html
  :linenos:

  <table dojoType="dojox.grid.DataGrid" >
    <thead>
      <tr>
        <th field="fieldName" width="200px">Column Name</th>
        <th field="fieldName" width="200px">Column Name</th>
      </tr>
    </thead>
  </table>

The ``<table>`` tag defines that a DataGrid is being created.  The nested ``<th>`` tags define the columns on the table.

*note:* the ``<thead>`` element is *required* in order for the DataGrid to read this markup as the layout. 

In the ``<th>`` tag in a declarative definition of a DataGrid, the following attributes are permitted

field
  The name of the field in the store data.  ``New in 1.4`` If you set the value of the field to "_item", then your formatter will be called with the entire item from the store - instead of just one field value
``New in 1.4`` fields
  An array of field names, when used, all values of all matching fields are returned to the grid
width
  The width of the column
cellType
  The type of cell in the column.  Allowable cell types include

  * ``dojox.grid.cells.Bool``
  * ``dojox.grid.cells.Select``

options
  Used when cellType is ``dojox.grid.cells.Select`` to name the allowable options
editable
  A boolean value that declares whether or not the cell is editable
``New in 1.4`` draggable
  A boolean value that you can set to false if you want a cell not to be draggable but others to be draggable
formatter
  A JavaScript function that is called which returns the value to be shown in the cell.  The value from the data store is passed as a parameter to the function.  The returned value that is inserted into the page can be any legal HTML.  In dojo 1.3 and earlier, it should *not* be a dijit Widget as that is not parsed.  ``New in 1.4`` You can return a dijit Widget and it will be placed in that location in the cell.  ``New in 1.4`` You can also return a dojo.Deferred and can then pass the deferred's callback function a string to insert at a later point in time.
get
  A JavaScript function that is called which returns the value to be shown in the cell.  The function is passed two parameters.  The first is the row index in the DataGrid.  The second is the DataStore record/item.  Given these two parameters, the function is expected to know what to return.  It should *not* be a dijit Widget as that is not parsed.  Care should be taken that the ``item`` parameter is not null.  Tests have shown that the function may be called more often than it should be and this is highlighted with an ``item = null``.
hidden
  This boolean property can be used to hide a column in the table.  If its value is ``true`` the column is hidden.  If ``false`` the column is displayed.
sortInfo
  A numerical value indicating what column should be sorted in the grid.  e.g. "1" would mean "first column, ascending order.  "-2" would mean "second column, descending order".  Note that this replaces the alternative approach of providing queryOptions to the store's fetch() invocation.  Defined on dojox.grid._Grid.

The value of the text between a ``<th>`` and ``</th>`` is used as the header label for the column.

The structure of the table can also be set programatically.  The ``<table>`` attribute called ``structure`` can name an object that defines the cell structure.

Event handling
--------------
Event handlers can be associated with the DataGrid.  Many of these events are expected to be handled by the DataGrid itself.  Grabbing these events without passing the event on to the grid can cause unexpected results.  As such, it is wise to add an event handler as opposed to replace the event handler.   Consider using :ref:`dojo.connect() <dojo/connect>`.

The following handlers are defined:

onStyleRow(inRow)
   TBD
onMouseOver(e)
   Fired when the mouse is over the grid.  The event contains references to the grid, cell and rowIndex.
onMouseOut(e)
   Fired when the mouse is leaves the grid.  The event contains references to the grid, cell and rowIndex.
onRowClick(e)
   Fired when a row is clicked.  The event contains references to the grid, cell and rowIndex.
onRowDblClick(e)
   Fired when a row is double clicked.  The event contains references to the grid, cell and rowIndex.
onRowContextMenu(e)
   Fired when a row is selected and then right clicked.

And many more ...

For the above, an event contains the normal DOM Events plus

cell
  TBD
cellIndex
  TBD
cellNode
  TBD
grid
  The DataGrid that caused the event
rowIndex
  The row index in the grid
rowNode
  TBD
sourceView
  TBD



DataGrid options
----------------
In addition to the options for the columns, there are also options available for the DataGrid itself.

jsId
  The name of a JavaScript variable that will be created that will hold the grid object.  This can then be referenced in scripts.
store
  The name of JavaScript variable that holds the store object used to get data for the grid.
rowSelector
  Specifying this table option adds a selection area on the left of the table to make row selection easier.  The value of this option is a width to be used for the selector.
selectionMode
  This option defines how row selection is handled.  Available options are:

  * none - No row selection.
  * single - Only single row selection.
  * multiple - Multiple explicit row selection.  A single click selects a row a second single click deselects the row.
  * extended - Multiple row selection including ranges (default).

columnReordering
  This boolean property allows columns to be dynamically reordered.  When enabled, a column header can be dragged and dropped at a new location causing the column to be moved.
headerMenu
  A menu can be associated with a header.  This attribute names a ``dijit.Menu`` which is displayed when the header is clicked.
autoHeight
  If true, automatically expand grid's height to fit data. If numeric, defines the maximum rows of data displayed (if the grid contains less than **autoHeight** rows, it will be shrunk).
autoWidth
  Automatically set width depending on columns width
singleClickEdit
  A boolean value that defines whether a single or double click is needed to enter cell editing mode.
loadingMessage
  The message to show while the content of the grid is loading.
errorMessage
  The message to show if an error has occurred loading the data.
``New in 1.3`` selectable
  Set to true if you want to enable text selection on your grid.
``New in 1.4`` formatterScope
  Set to an object that you would like to execute your formatter functions within the scope of.
``New in 1.4`` updateDelay
  A value, in milliseconds (default 1) to delay updates when receiving notifications from a datastore.  Set to 0 to update your grid immediately.  A larger value will result in a more performant grid when there are lots of datastore notifications happening, but there will be significant lag time in the update on-screen.  The default value of 1 will basically re-render changes once the browser is idle.
``New in 1.4`` initialWidth
  A CSS string value to use for autoWidth grids as their initial width.  If not set, it defaults to the sum width of all columns.  If set, it overrides any values passed to the grid via css or the html style parameter on the source node.
``New in 1.3.2`` escapeHTMLInData
  This will escape HTML brackets from the data to prevent HTML from user-inputted data being rendered with may contain JavaScript and result in XSS attacks. This is true by default, and it is recommended that it remain true. Setting this to false will allow data to be displayed in the grid without filtering, and should be only used if it is known that the data won't contain malicious scripts. If HTML is needed in grid cells, it is recommended that you use the formatter function to generate the HTML (the output of formatter functions is not filtered, even with escapeHTMLInData set to true). Setting this to false can be done:

.. code-block :: javascript
  :linenos:

  <table dojoType="dojox.grid.DataGrid" escapeHTMLInData="false" ...>

Editing cells
-------------
A cell can be defined as editable by setting its ``editable`` flag to be ``true``.  In the markup, this is achieved by adding the attribute ``editable="true"`` to the ``<th>`` definition.

If a cell is editable and no ``cellType`` is supplied, then double clicking on the cell will provide an in-place text editor to change its value.

If the type of the cell is a boolean, then its value is displayed as either the string ``true`` or ``false``.  If a check box is desired, setting the ``cellType`` to be ``dojox.grid.cells.Bool`` and marking it as editable will make a checkbox appear.

If the cell type is defined to be ``dojox.grid.cells.Select`` then a combo-box/pulldown is available showing allowable options.

.. Question: How to make a checkbox appear when we don't want the cell to be editable?

Data for the grid
-----------------
Data for the grid comes from a data store.  The data can be specified declaratively using the ``store="name"`` attribute where ``name`` is the name of a global JavaScript object that represents a DataStore.  This could previously have been created as follows:

.. code-block :: html
  :linenos:

  <span dojoType="dojo.data.ItemFileWriteStore" 
     jsId="myStore" url="/myData.json">
  </span>

Programatically, a store can be assigned to a DataGrid with the ``setStore(myStore)`` method call.

It should be noted that as of grid 1.3.1, the grid searched your datastore and converts all < to &lt; to avoid a cross-site scripting attack. Site developers who can guarantee that their data is safe can add a formatter function to convert all &lt; back to < if they need the datastore information parsed by the browser. 


Locking columns from horizontal scrolling
-----------------------------------------
A set of columns can be *locked* to prevent them from scrolling horizontally while allows other columns to continue to scroll.  To achieve this, the ``<colgroup>`` tags can be inserted before the ``<thead>`` tag.  For example, if a DataGrid has four columns, the following will lock the first column but allow the remaining columns the ability to scroll horizontally:

.. code-block :: html
  :linenos:

  <colgroup span="1" noscroll="true"></colgroup>
  <colgroup span="3"></colgroup>

Auto-width columns
------------------
Columns with width="auto" are not fully supported, and do not work in all cases.  In addition, they are poorly performant.

The main reason for this is the "dynamic" nature of the grid itself.  The grid needs to start laying itself out *before* it has any data - so it does not have a way to "know" how wide to draw the columns - because we don't have the data.  Depending on the browser, we are able to make a "best guess" - but it doesn't work in all situations.

It is strongly suggested that users move away from using width="auto" columns.  We are even considering deprecating their use in upcoming releases of the grid.

The only way that we are able to support width="auto" is to:
  1. require that all data be present (so we can figure out the "widest" value for the column)
  2. render all data at once (so that we are sure we have rendered the "widest" value)
  3. render the grid twice (once to lay out the values and calculate the widest one - another time to actually set all the widths to the width of the widest value)

Each of these greatly hurts the grid - and in reality is not feasible.  #1 would mean that you are unable to use stores such as JsonRestStore or QueryReadStore with a grid.  #2 will really impact your performance...because it throws away all the benefits of incremental rendering and virtual scrolling...you'll never be able to have million-row grids like you can right now.  #3 is bad - especially in combination with #2 - since, in effect, it will take twice as long to display your grid...and you will get "flickering" - that is, you will see it render once with different cell widths, and then it will redraw again.

Again - don't use width="auto".  It's very much not recommended, and will not be supported in the future.


Multi-rowed *rows*
------------------
We are used to a row in a table being a single line of data.  DataGrid provides the ability for a single logical row to contain multiple lines of data.  This can be achieved by adding additional ``<tr>`` tags into the DataGrid declaration.

For example:

.. code-block :: javascript
  :linenos:

  <table dojoType="dojox.grid.DataGrid" store="myTestStore" style="width: 800px; height: 300px;">
    <thead>
      <tr>
        <th field="A" width="200px">Col1</th>
        <th field="B" width="200px">Col2</th>
        <th field="C" width="200px">Col3</th>
      </tr>
      <tr>
        <th field="D" colspan="3">Col4</th>
      </tr>
    </thead>
  </table>

Results in a grid with columns A, B and C and a fourth *column* called D which exists on the same row of data.

Required CSS
------------
Some style sheets supplied with the Dojo distribution are required:

.. code-block :: html
  :linenos:

  <style type="text/css">
    @import "/dojox/grid/resources/Grid.css";
    @import "/dojox/grid/resources/{{ theme }}Grid.css";

    .dojoxGrid table {
      margin: 0;
    }
  </style>


DataGrid object functions
-------------------------

getItem(idx)
  Returns the store ``item`` at the given row index.
getItemIndex(item)
  Returns the row index for the given store ``item``.
setStore
  TBD
setQuery
  TBD
setItems
  TBD
filter
  TBD
sort
  TBD
canSort
  canSort is called by the grid to determine if each column should be sortable.  It takes a single integer argument representing the column index, which is positive for ascending order and negative for descending order, and should return true if that column should be sortable in that direction, and false if not.  For example, to only allow the second column to be sortable, in either direction: "function canSort(col) { return Math.abs(col) === 2; }"
getSortProps
  TBD
removeSelectedRows
  TBD


Unknown at this time
--------------------
Here are some undocumented (here) components:

* elasticView - An attribute on the table
* rowsPerPage - An attribute on the table
* query - An attribute on the table
* clientSort - An attribute on the table




Getting a value from a row knowing the row index
------------------------------------------------
Assume that you know the row index and the name of the column whos value you wish to retrieve, you can obtain that value using the following snippet:

.. code-block :: javascript
  :linenos:

  var value = grid.store.getValue(grid.getItem(rowIndex), name);


===================================================
IMPORTANT INFORMATION about Formatting and Security
===================================================

Preventing cross-site scripting (XSS) attacks
---------------------------------------------

To avoid cross-site scripting (XSS) attacks, the grid will escape any HTML data that comes from an external source (datastore).  This escaping also applies to any values that are returned from a custom get function on a cell.  If you would like to format your data using HTML, you should create a custom formatter function for the cell and apply your formatting there instead.

Site developers who can guarantee that their data is safe can add a formatter function to convert all &lt; back to < if they need the datastore information parsed by the browser.

Finally, you can use the escapeHTMLInData option - however, this is `VERY HIGHLY DISCOURAGED` as it opens your application up to XSS attacks.

========
Examples
========

The following examples are for the new Grid 1.2.

A simple Grid
-------------

This example shows how to create a simple Grid declaratively.

.. cv-compound::
  :djConfig: parseOnLoad: true
  :version: local

  .. cv:: javascript

    <script type="text/javascript">
        dojo.require("dojox.grid.DataGrid");
        dojo.require("dojox.data.CsvStore");
    </script>

  .. cv:: html

    <span dojoType="dojox.data.CsvStore" 
        jsId="store1" url="{{ dataUrl }}dojox/grid/tests/support/movies.csv">
    </span>

    <table dojoType="dojox.grid.DataGrid"
        store="store1"
        query="{ Title: '*' }"
        clientSort="true"
        style="width: 100%; height: 100%;"
        rowSelector="20px">
        <thead>
            <tr>
                <th width="300px" field="Title">Title of Movie</th>
                <th width="50px">Year</th>
            </tr>
            <tr>
                <th colspan="2">Producer</th>
            </tr>
        </thead>
    </table>

  .. cv:: css

    <style type="text/css">
        @import "{{ baseUrl }}dojox/grid/resources/Grid.css";
        @import "{{ baseUrl }}dojox/grid/resources/{{ theme }}Grid.css";

        .dojoxGrid table {
            margin: 0;
        }

        html, body {
            width: 100%;
            height: 100%;
            margin: 0;
        }
    </style>


Programmatically creating a DataGrid
------------------------------------

This example shows how to create a simple Grid programmatically.

.. cv-compound::
  :version: local

  .. cv:: javascript

    <script type="text/javascript">
        dojo.require("dojox.grid.DataGrid");
        dojo.require("dojox.data.CsvStore");
    
        dojo.addOnLoad(function(){
          // our test data store for this example:
          var store4 = new dojox.data.CsvStore({ url: '{{ dataUrl }}dojox/grid/tests/support/movies.csv' });

          // set the layout structure:
          var layout4 = [
              { field: 'Title', name: 'Title of Movie', width: '200px' },
              { field: 'Year', name: 'Year', width: '50px' },
              { field: 'Producer', name: 'Producer', width: 'auto' }
          ];

          // create a new grid:
          var grid4 = new dojox.grid.DataGrid({
              query: { Title: '*' },
              store: store4,
              clientSort: true,
              rowSelector: '20px',
              structure: layout4
          }, document.createElement('div'));

          // append the new grid to the div "gridContainer4":
          dojo.byId("gridContainer4").appendChild(grid4.domNode);

          // Call startup, in order to render the grid:
          grid4.startup();
        });
    </script>

  .. cv:: html

    <div id="gridContainer4" style="width: 100%; height: 100%;"></div>

  .. cv:: css

    <style type="text/css">
        @import "{{ baseUrl }}dojox/grid/resources/Grid.css";
        @import "{{ baseUrl }}dojox/grid/resources/{{ theme }}Grid.css";

        .dojoxGrid table {
            margin: 0;
        }

        html, body {
            width: 100%;
            height: 100%;
            margin: 0;
        }
    </style>

Note the grid.startup() command after constructing the DataGrid.  Earlier development
versions of DataGrid didn't require this but as of 1.2.0b1, you must call
startup() as you would with other dijits, or the grid will not render.

Working with selections
-----------------------

To get the current selected rows of the grid, you can use the method yourGrid.selection.getSelected(). You will get an array of the selected items. The following code shows an example:

.. cv-compound::
  :djConfig: parseOnLoad: true
  :version: local
  :height: 480

  .. cv:: javascript

    <script type="text/javascript">
        dojo.require("dojox.grid.DataGrid");
        dojo.require("dojox.data.CsvStore");
        dojo.require("dijit.form.Button");
    </script>

  .. cv:: html

    <span dojoType="dojox.data.CsvStore" 
        jsId="store2" url="{{ dataUrl }}dojox/grid/tests/support/movies.csv">
    </span>

    <p class="info">
        Select a single row or multiple rows in the Grid (click on the Selector on the left side of each row). 
        After that, a click on the Button "get all Selected Items" will show you each attribute/value of the
        selected rows.
    </p>

    <table dojoType="dojox.grid.DataGrid"
        jsId="grid2"
        store="store2"
        query="{ Title: '*' }"
        clientSort="true"
        style="width: 100%; height: 300px;"
        rowSelector="20px">
        <thead>
            <tr>
                <th width="300px" field="Title">Title of Movie</th>
                <th width="50px">Year</th>
            </tr>
            <tr>
                <th colspan="2">Producer</th>
            </tr> 
        </thead>
    </table>

    <p class="container">
    <span dojoType="dijit.form.Button">
        get all Selected Items
        <script type="dojo/method" event="onClick" args="evt">
            // Get all selected items from the Grid:
            var items = grid2.selection.getSelected();
            if(items.length){
                // Iterate through the list of selected items.
                // The current item is available in the variable 
                // "selectedItem" within the following function:
                dojo.forEach(items, function(selectedItem) {
                    if(selectedItem !== null) {
                        // Iterate through the list of attributes of each item.
                        // The current attribute is available in the variable
                        // "attribute" within the following function:
                        dojo.forEach(grid2.store.getAttributes(selectedItem), function(attribute) {
                            // Get the value of the current attribute:
                            var value = grid2.store.getValues(selectedItem, attribute);
                            // Now, you can do something with this attribute/value pair.
                            // Our short example shows the attribute together
                            // with the value in an alert box, but we are sure, that
                            // you'll find a more ambitious usage in your own code:
                            alert('attribute: ' + attribute + ', value: ' + value);
                        }); // end forEach
                    } // end if
                }); // end forEach
            } // end if
        </script>
    </span>
    </p>

  .. cv:: css

    <style type="text/css">
        @import "{{ baseUrl }}dojox/grid/resources/Grid.css";
        @import "{{ baseUrl }}dojox/grid/resources/{{ theme }}Grid.css";

        .dojoxGrid table {
            margin: 0;
        }

        html, body {
            width: 100%;
            margin: 0;
        }

        .container {
            text-align: center;
        }

        .info {
            margin: 10px;
        }
    </style>


Grid 1.2 supports a new parameter "selectionMode" which allows you to control the behaviour of the selection functionality:

'none'
  deactivates the selection functionality
'single'
  let the user select only one item at the same time
'multiple'
  let the user selects more than one item at the same time
'extended' (default) 
  *not sure, what's the difference between "multiple" and "extended"*


Editing data
------------

Grid allows you to edit your data easily and send the changed values back to your server

First, you have to set a editor for each cell, you would like to edit:

.. cv-compound::
  :djConfig: parseOnLoad: true
  :version: local
  :height: 480

  .. cv:: javascript

    <script type="text/javascript">
        dojo.require("dojox.grid.DataGrid");
        dojo.require("dojo.data.ItemFileWriteStore");
    </script>

  .. cv:: html

    <span dojoType="dojo.data.ItemFileWriteStore" 
        jsId="store3" url="{{ dataUrl }}dijit/tests/_data/countries.json">
    </span>

    <p class="info">
        This example shows, how to make the column "Type" editable.
        In order to select a new value, you have to double click on the current value in the second column.
    </p>

    <table dojoType="dojox.grid.DataGrid"
        jsId="grid3"
        store="store3"
        query="{ name: '*' }"
        rowsPerPage="20"
        clientSort="true"
        style="width: 100%; height: 300px;"
        rowSelector="20px">
        <thead>
            <tr>
                <th width="200px" 
                    field="name">Country/Continent Name</th>
                <th width="auto" 
                    field="type" 
                    cellType="dojox.grid.cells.Select" 
                    options="country,city,continent" 
                    editable="true">Type</th>
            </tr>
        </thead>
    </table>

  .. cv:: css

    <style type="text/css">
	@import "{{ baseUrl }}dojox/grid/resources/{{ theme }}Grid.css";

        html, body {
            width: 100%;
            margin: 0;
        }

        .info {
            margin: 10px;
        }
    </style>

Adding and Deleting data
------------------------

If you want to add (remove) data programatically, you just have to add (remove) it from the underlying data store.
Since DataGrid is "DataStoreAware", changes made to the store will be reflected automatically in the DataGrid.
 
.. cv-compound::
  :djConfig: parseOnLoad: true
  :version: local
  :height: 480

  .. cv:: javascript

    <script type="text/javascript">
        dojo.require("dojox.grid.DataGrid");
        dojo.require("dojo.data.ItemFileWriteStore");
        dojo.require("dijit.form.Button");
    </script>

  .. cv:: html

    <span dojoType="dojo.data.ItemFileWriteStore" 
        jsId="store3" url="{{ dataUrl }}dijit/tests/_data/countries.json">
    </span>

    <p class="info">
        This example shows, how to add/remove rows
    </p>

    <table dojoType="dojox.grid.DataGrid"
        jsId="grid5"
        store="store3"
        query="{ name: '*' }"
        rowsPerPage="20"
        clientSort="true"
        style="width: 100%; height: 300px;"
        rowSelector="20px">
        <thead>
            <tr>
                <th width="200px" 
                    field="name">Country/Continent Name</th>
                <th width="auto" 
                    field="type" 
                    cellType="dojox.grid.cells.Select" 
                    options="country,city,continent" 
                    editable="true">Type</th>
            </tr>
        </thead>
    </table>

    <p class="container">
      <span dojoType="dijit.form.Button">
          Add Row
          <script type="dojo/method" event="onClick" args="evt">
              // set the properties for the new item:
              var myNewItem = {type: "country", name: "Fill this country name"}; 
              // Insert the new item into the store:
              // (we use store3 from the example above in this example)
              store3.newItem(myNewItem);
          </script>
      </span>
    
      <span dojoType="dijit.form.Button">
          Remove Selected Rows
          <script type="dojo/method" event="onClick" args="evt">
              // Get all selected items from the Grid:
              var items = grid5.selection.getSelected();
              if(items.length){
                  // Iterate through the list of selected items.
                  // The current item is available in the variable 
                  // "selectedItem" within the following function:
                  dojo.forEach(items, function(selectedItem) {
                      if(selectedItem !== null) {
                          // Delete the item from the data store:
                          store3.deleteItem(selectedItem);
                      } // end if
                  }); // end forEach
              } // end if
          </script>
      </span>
    </p>

  .. cv:: css

    <style type="text/css">
	@import "{{ baseUrl }}dojox/grid/resources/{{ theme }}Grid.css";

        html, body {
            width: 100%;
            margin: 0;
        }

        .container {
            text-align: center;
            margin: 10px;
        }

        .info {
            margin: 10px;
        }
    </style>

Filtering data
--------------

The Grid offers a filter() method, to filter data from the current query (client-side filtering).

.. cv-compound::
  :djConfig: parseOnLoad: true
  :version: local
  :height: 480

  .. cv:: javascript

    <script type="text/javascript">
        dojo.require("dojox.grid.DataGrid");
        dojo.require("dojox.data.CsvStore");
        dojo.require("dijit.form.Button");
    </script>

  .. cv:: html

    <span dojoType="dojox.data.CsvStore" 
        // We use the store from the examples above.
        // Please uncomment this line, if you need your own store:
        // jsId="store2" url="{{ dataUrl }}dojox/grid/tests/support/movies.csv">
    </span>

    <p class="info">
        Click on the button "filter movies" to filter the current data (only movies with title "T*" will be visible).<br />
        Click on the button "show all movies" to remove the filter.
    </p>
 
    <table dojoType="dojox.grid.DataGrid"
        jsId="grid3"
        store="store2"
        query="{ Title: '*' }"
        clientSort="true"
        style="width: 100%; height: 300px;"
        rowSelector="20px">
        <thead>
            <tr>
                <th width="300px" field="Title">Title of Movie</th>
                <th width="50px">Year</th>
            </tr>
            <tr>
                <th colspan="2">Producer</th>
            </tr> 
        </thead>
    </table>

    <p class="container">
    <span dojoType="dijit.form.Button">
        filter movies
        <script type="dojo/method" event="onClick" args="evt">
            // Filter the movies from the data store:
            grid3.filter({Title: "T*"});
        </script>
    </span>

    <span dojoType="dijit.form.Button">
        show all movies
        <script type="dojo/method" event="onClick" args="evt">
            // reset the filter:
            grid3.filter({Title: "*"});
        </script>
    </span>
    </p>

  .. cv:: css

    <style type="text/css">
	@import "{{ baseUrl }}dojox/grid/resources/{{ theme }}Grid.css";

        html, body {
            width: 100%;
            margin: 0;
        }

        .container {
            text-align: center;
            margin: 10px;
        }

        .info {
            margin: 10px;
        }
    </style>

Grid styling: Rows
------------------

The DataGrid provides extension points which allows you to apply custom css classes or styles to a row, depending on different parameters.
To use it, you just have to override default behavior by yours.

.. cv-compound::
  :djConfig: parseOnLoad: true
  :version: local

  .. cv:: javascript

    <script type="text/javascript">
        dojo.require("dojox.grid.DataGrid");
        dojo.require("dojo.data.ItemFileWriteStore");
    </script>

  .. cv:: html

    <span dojoType="dojo.data.ItemFileWriteStore" 
        jsId="store3" url="{{ dataUrl }}dijit/tests/_data/countries.json">
    </span>

    <table dojoType="dojox.grid.DataGrid"
        jsId="grid6"
        store="store3"
        query="{ name: '*' }"
        rowsPerPage="20"
        clientSort="true"
        style="width: 100%; height: 100%;"
        rowSelector="20px">
        <script type="dojo/method" event="onStyleRow" args="row">
	     //The row object has 4 parameters, and you can set two others to provide your own styling
	     //These parameters are :
	     //	-- index : the row index
	     //	-- selected: wether the row is selected
	     //	-- over : wether the mouse is over this row
	     //	-- odd : wether this row index is odd.
	     var item = grid6.getItem(row.index);
	     if(item){
		var type = store3.getValue(item,"type",null);
		if(type == "continent"){
		    row.customStyles += "color:red;";
	        }
	     }
	     grid6.focus.styleRow(row);
	     grid6.edit.styleRow(row);
	</script>
        <thead>
            <tr>
                <th width="200px" 
                    field="name">Country/Continent Name</th>
                <th width="auto" 
                    field="type" 
                    cellType="dojox.grid.cells.Select" 
                    options="country,city,continent" 
                    editable="true">Type</th>
            </tr>
        </thead>
    </table>

  .. cv:: css

    <style type="text/css">
        @import "{{ baseUrl }}dojox/grid/resources/{{ theme }}Grid.css";

        .dojoxGrid table {
            margin: 0;
        }

        html, body {
            width: 100%;
            height: 100%;
            margin: 0;
        }
    </style>


====
Tips
====

Creating a grid in a node with display: none
--------------------------------------------

It is not possible to create a grid as a child of a node which is set to be not displayed (display: none).
If you need to do this though for some reason you can set the grids visibility to "hidden" and its position offscreen 

Hiding the Headers of a Grid
----------------------------

You can hide the columns of a Grid by using normal css:

.. code-block :: html
  :linenos:

  .dojoxGrid-header { display:none; }


Refreshing the content of a grid
--------------------------------

There are times when you may wish to update the content of the grid. For example, a button on the screen may cause an xhrGet to retrieve a new set of information that you want to display in the table. The following code snippet can be used to update the grid:

.. code-block :: javascript
  :linenos:

  var newStore = new dojo.data.ItemFileReadStore({data: {... some data ...});
  var grid = dijit.byId("gridId");
  grid.setStore(newStore);

===============================
Accessibility in 1.3 and Beyond
===============================

Keyboard
--------

==============================================    ===============================================
Action                                            Key
==============================================    ===============================================
Navigate into the grid			          The column header section and the data section are two separate tab stops in the grid. Press tab to put focus into the column header. With focus on a column header, press tab to set focus into the data portion of the grid. Focus will go to the data cell which last had focus in the grid or to the first data cell if focus had not been previously set into the grid in this session. 
Navigate between column headers	                  With focus on a column header, use the left and right arrow keys to move between column headers.
Navigate between data cells		          With focus on a data cell, use the left, right, up, down, pageup and pagedown arrow keys to move between data cells. The grid may load additional content as it is scrolled which may result in a delay.  Focus should appear on the appropriate cell once the data has completed loading.
Sort a column					  With focus on a column header press the enter key to sort the column. Focus remains in the column header after the sort.
Edit a cell				          If the cell is editable, pressing enter with focus on the cell will put it into edit mode.
Cancel edit mode				  When a cell is being edited, pressing escape will cancel edit mode. 
End edit mode					  When a cell is being edited, pressing enter will accept the change and end edit mode.
Focus editable cells				  With focus on an editable cell, pressing tab will move focus to the next editable cell in editing mode.  Pressing shift-tab will move focus to the previous editable cell in editing mode.  Note there are still some issues when traversing row boundaries.
Invoke an onrowclick event	                  If the grid row has an onrowclick event, it can be invoked by pressing enter with focus on a cell in the row.
Select a row				          With focus on a cell in a row, press the space bar.
Select contiguous rows			          Select a row, hold down the shift key and arrow up or down to a new row, press the space bar to select the rows between the original row and the new row.
Select discontinuous rows		          Select a row,  hold down the control key and use the arrow keys to navigate to a new row,  continue holding the control key and press the space bar to add the new row to the selection.
Change column size (1.4)                          Set focus to a column header, hold shift+control and press the left or right arrow key so change the column size.
==============================================    ===============================================

Known Issues
------------

The basic DataGrid is accessible however, some advanced features are not.  

Keyboard
~~~~~~~~

* There is no keyboard mechanism to change column size in 1.3. This was added in 1.4.  
* Keyboard navigation does NOT skip hidden columns in 1.3. This was fixed in 1.4. Hidden colummns are now skipped when arrowing through the column headers and data.
* There is no keyboard support for drag and drop. If you rely on drag and drop to reorder columns, you must provide an alternative keyboard mechanisism (dialog box, context menu, etc.) to perform the same function. 
* Tree Grids are not supported for Accessibility.
* Developers who add additional features via scripting, such as hidden rows, are responsible for the accessibility of the added feature(s).
* Invoking links within cells via the keyboard is not supported.  

Screen Reader
~~~~~~~~~~~~~
The DojoX DataGrid is a complicated widget created via Scripting.  It has been enabled with `WAI-ARIA <http://www.w3.org/WAI/intro/aria>`_  properties, but unfortunately the current browsers (Firefox 3.5+ and IE 8) and screen readers (JAWS 11) do not fully support all of those properties.  Thus, information about the grid readonly, row selection and column sort status are not spoken by the screen reader.  There is still additional work on the part of the screen reader for information about row and column headers to be correctly spoken as the user traverses the data cells. Better support is expected in future versions of the browsers and screen readers and the Dojox DataGrid will be updated, as necessary, to take advantage of the additional ARIA support.  


========
See also
========

* :ref:`dojox.grid.EnhancedGrid <dojox/grid/EnhancedGrid>`

  An enhanced version of the base grid, which extends it in numerous useful ways

* :ref:`dojox.grid.TreeGrid <dojox/grid/TreeGrid>`

  This grid offers support for collapsable rows and model-based (:ref:`dijit.tree.ForestStoreModel <dijit/tree/ForestStoreModel>`) structure

* :ref:`Grid Plugin API <dojox/grid/pluginAPI>`

* `Introducing the 1.2 DataGrid <http://www.sitepen.com/blog/2008/07/14/dojo-12-grid/>`_
* `New Features in Dojo Grid 1.2 <http://www.sitepen.com/blog/2008/10/22/new-features-in-dojo-grid-12/>`_
* `Dojo Grids: Diving Deeper <http://www.sitepen.com/blog/2007/11/13/dojo-grids-diving-deeper/>`_
* `Simple Dojo Grids <http://www.sitepen.com/blog/2007/11/06/simple-dojo-grids/>`_
* `Dojo Grid Widget Updated. Data Integration and Editing Improvements. <http://ajaxian.com/archives/dojo-grid-widget-updated-data-integration-and-editing-improvements>`_
