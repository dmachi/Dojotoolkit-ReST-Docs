.. _quickstart/arrays:

Arrays and Dojo
===============

:Authors: Nikolai Onken, Marcus Reimann, Josh Trutwin

.. contents::
    :depth: 2

Dojo comes with a bunch of useful methods to deal with arrays, a few more than you get from your browser by default.


============
dojo.indexOf
============

.. code-block :: javascript

  dojo.indexOf(array, valueToFind, fromIndex, findLast)


dojo.indexOf lets you easily determine the index of an element in an array. It locates the first index of the provided value in the passed array. If the value is not found, -1 is returned.

.. cv-compound::

  .. cv :: javascript

    <script type="text/javascript">
    // this Button is just to make the demo look nicer:
    dojo.require("dijit.form.Button"); 

    var arrIndxOf = ["foo", "hoo", "zoo"];

    function testIndxOf() {
        var position = dojo.indexOf(arrIndxOf, "zoo");
        dojo.place(
            "<p>The index of the word 'zoo' within the array is " + position + "</p>", 
            "result1", 
            "after"
        );
    }
    </script>

  .. cv :: html

    <div>The content of our test array is ["foo", "hoo", "zoo"].</div>
    <button id="refButton1" dojoType="dijit.form.Button" onClick="testIndxOf()" type="button">Show the index of the word 'zoo' within the array.</button>
    <div id="result1"></div>


================
dojo.lastIndexOf
================

.. code-block :: javascript

  dojo.lastIndexOf(array, valueToFind, fromIndex)


dojo.lastIndexOf lets you easily determine the last index of an element in an array. It locates the last index of the provided value in the passed array. If the value is not found, -1 is returned.

**Note**: Calling `indexOf` with the `findLast` parameter set to true is the same as calling `lastIndexOf`.

.. cv-compound::

  .. cv :: javascript

    <script type="text/javascript">
    // this Button is just to make the demo look nicer:
    dojo.require("dijit.form.Button"); 

    var arrLastIndxOf = ["foo", "hoo", "zoo", "shoe", "zoo", "nuu"];

    function testLastIndxOf() {
        var position = dojo.lastIndexOf(arrLastIndxOf , "zoo");
        dojo.place(
            "<p>The last index of the word 'zoo' within the array is " + position + "</p>", 
            "result2", 
            "after"
        );
    }
    </script>

  .. cv :: html

    <div>The content of our test array is ["foo", "hoo", "zoo", "shoe", "zoo", "nuu"].</div>
    <button id="refButton2" dojoType="dijit.form.Button" onClick="testLastIndxOf()" type="button">Show the last index of the word 'zoo' within the array.</button>
    <div id="result2"></div>


============
dojo.forEach
============

.. code-block :: javascript

  dojo.forEach(array, callback, fromIndex)

This is a heavylifter you will use a lot when writing your apps using Dojo. dojo.forEach lets you iterate over arrays, node lists and provides you with ways to filter your results. Lets take a look at a very basic example.
Note the "i" variable which returns the current position of an iteration

.. cv-compound::

  .. cv :: javascript

    <script type="text/javascript">
    dojo.require("dijit.form.Button"); // this is just to make the demo look nicer

    var arrFruit = ["apples", "kiwis", "pineapples"];
    function populateData(){
      dojo.forEach(arrFruit, function(item, i){
        var li = dojo.doc.createElement("li");
        li.innerHTML = i+1+". "+item;
        dojo.byId("forEach-items").appendChild(li);
      });
    }
    </script>

  .. cv :: html

    <button dojoType="dijit.form.Button" onClick="populateData()" type="button">Populate data</button>
    <ul id="forEach-items">

    </ul>

Now lets use dojo.forEach with a list of dom nodes we retrieve using dojo.query. Note that dojo.query returns the list of dom nodes as an array. This way you can easily iterate over each dom node using dojo.forEach

.. cv-compound::

  .. cv :: javascript

    <script type="text/javascript">
    dojo.require("dijit.form.Button"); // this is just to make the demo look nicer

    var arr = ["apples", "kiwis", "pineapples"];
    function populateQueryData(){
      dojo.query("li").forEach(function(item, i){
        var li = dojo.doc.createElement("li");
        li.innerHTML = i+1+". "+item.innerHTML;
        dojo.byId("forEachQuery-items").appendChild(li);
      });
    }
    </script>

  .. cv :: html

    <button dojoType="dijit.form.Button" onClick="populateQueryData()" type="button">Populate data</button>
    <ul id="forEachQuery-items">

    </ul>

To break the forEach-Loop you should use dojo.some

.. cv-compound::

  .. cv :: javascript

    <script type="text/javascript">
	dojo.require("dijit.form.Button");

	function arrayLoopTest() {
		var myArray = [0,1,2,3,4,5,6,7,8,9];
		var count;
		
		//lets iterate ALL entrys of myArray
		count = 0;
		dojo.forEach(myArray, function(entry){
			count++;
		});
		
		alert("iterated "+count+" entrys (dojo.forEach)"); //will show "iterated 10 entrys"
		
		//lets only iterate the first 4 entrys of myArray
		count = 0;
		dojo.some(myArray, function(entry){

			if(count >= 4)
			{
				return false;
			}
			
			count++;
		});
		
		alert("iterated "+count+" entrys (dojo.some)"); //will show "iterated 4 entrys"
	}
    </script>

  .. cv :: html

    <button dojoType="dijit.form.Button" onClick="arrayLoopTest()" type="button">Start Testloops</button>


===========
dojo.filter
===========

.. code-block :: javascript

  dojo.filter(array, callback, thisObject)

There are many cases when you have an array and want to filter it by a certain condition, say you have an array of people with a last name. You would like to filter those having a certain last name. Lets take a look at anexample

.. cv-compound::

  .. cv :: javascript

    <script type="text/javascript">
    dojo.require("dijit.form.Button"); // this is just to make the demo look nicer

    var arr = [{surname: "Washington", name: "Paul"}, 
               {surname: "Gordon", name: "Amie"}, 
               {surname: "Meyer", name: "Sofie"}, 
               {surname: "Jaysons", name: "Josh"}, 
               {surname: "Washington", name: "George"}, 
               {surname: "Doormat", name: "Amber"}, 
               {surname: "Smith", name: "Susan"}, 
               {surname: "Hill", name: "Strawberry"}, 
               {surname: "Washington", name: "Dan"}, 
               {surname: "Dojo", name: "Master"}];

    function filterArray(){
      var filteredArr = dojo.filter(arr, function(item){
        return item.surname == "Washington";
      });

      dojo.forEach(filteredArr, function(item, i){
        var li = dojo.doc.createElement("li");
        li.innerHTML = i+1+". "+item.surname+", "+item.name;
        dojo.byId("filtered-items").appendChild(li);
      });

      dojo.forEach(arr, function(item, i){
        var li = dojo.doc.createElement("li");
        li.innerHTML = i+1+". "+item.surname+", "+item.name;
        dojo.byId("unFiltered-items").appendChild(li);
      });
    }
    </script>

  .. cv :: html

    <button dojoType="dijit.form.Button" onClick="filterArray()" type="button">Filter array</button>
    <div style="width: 300px; float: left;">
    Filtered items<br />(only people with "Washington" as surname)
    <ul id="filtered-items">

    </ul>
    </div>
    <div style="width: 300px; float: left;">
    Unfiltered items<br /> (all people are represented in the list)
    <ul id="unFiltered-items">

    </ul>
    </div>

========
dojo.map
========

.. code-block :: javascript

  dojo.map(array, callback, thisObject)

Another great function provided by Dojo is dojo.map. dojo.map lets you run a function on all elements of an array and returns a new array with the changed values. A very good example is the "Give all my employees a 10% salary rise":

.. cv-compound::

  .. cv :: javascript

    <script type="text/javascript">
    dojo.require("dijit.form.Button"); // this is just to make the demo look nicer

    var arrSalary = [200, 300, 1500, 5, 4500];

    function raiseSalary(){
      var raisedSalaries = dojo.map(arrSalary, function(item){
        return item+(item/100)*10;
      });

      dojo.forEach(raisedSalaries, function(item, i){
        var li = dojo.doc.createElement("li");
        li.innerHTML = i+1+". New salary: "+item;
        dojo.byId("filteredSalary-items").appendChild(li);
      });

      dojo.forEach(arrSalary, function(item, i){
        var li = dojo.doc.createElement("li");
        li.innerHTML = i+1+". Old salary: "+item;
        dojo.byId("unFilteredSalary-items").appendChild(li);
      });
    }
    </script>

  .. cv :: html

    <button dojoType="dijit.form.Button" onClick="raiseSalary()" type="button">Raise the salary</button>
    <div style="width: 300px; float: left;">
    Peoples salaries after raise:
    <ul id="filteredSalary-items">

    </ul>
    </div>
    <div style="width: 300px; float: left;">
    Peoples salaries before raise:
    <ul id="unFilteredSalary-items">

    </ul>
    </div>

For complete documentation and more examples please check the :ref:`dojo.map documentation <dojo/map>`


=========
dojo.some
=========

.. code-block :: javascript

  dojo.some(array, callback, thisObject);

Imagine you are a manager of a famous bank. A client of you comes and visits your office asking for another million dollars as a credit.
Now your bank policies only allows you to give each client one credit over 1 million, not two, not three - though you may have several smaller credits. Even 3 credits a 500.000 - weird bank.. anyways. dojo.some is the perfect function to tell you whether an array has some of the asked values:

.. cv-compound::

  .. cv :: javascript

    <script type="text/javascript">
    // this Button is just to make the demo look nicer:
    dojo.require("dijit.form.Button"); 

    var arrIndxSome = [200000, 500000, 350000, 1000000, 75, 3];

    function testIndxSome() {
        if (dojo.some(arrIndxSome, function(item){ return item>=1000000})) {
            result = 'yes, there are';
        } else {
            result = 'no, there aren no such items';
        }
        dojo.place(
            "<p>The answer is: " + result + "</p>", 
            "result6", 
            "after"
        );
    }
    </script>

  .. cv :: html

    <div>The content of our test array is [200000, 500000, 350000, 1000000, 75, 3].</div>
    <button id="refButton6" dojoType="dijit.form.Button" onClick="testIndxSome()" type="button">Are there some items >=1000000 within the array?</button>
    <div id="result6"></div>


==========
dojo.every
==========

.. code-block :: javascript

  dojo.every(array, callback, thisObject);

Lets get back to our bank manager. A client wants another credit, but you only allow a credit if every income transfer is at least 3000,-
An example:

.. cv-compound::

  .. cv :: javascript

    <script type="text/javascript">
    // this Button is just to make the demo look nicer:
    dojo.require("dijit.form.Button"); 

    var arrIndxEvery = [{'month': 'january', 'income': 2000}, {'month': 'february', 'income': 3200}, {'month': 'march', 'income': 2100}];

    function testIndxSome() {
        if (dojo.every(arrIndxEvery , function(item){ return item.income>=3000})) {
            result = 'yes, he is allowed';
        } else {
            result = 'no, unfortunately not';
        }
        dojo.place(
            "<p>The answer is: " + result + "</p>", 
            "result7", 
            "after"
        );
    }
    </script>

  .. cv :: html

    <div>The content of our test array is [{'month': 'january', 'income': 2000}, {'month': 'february', 'income': 3200}, {'month': 'march', 'income': 2100}].</div>
    <button id="refButton7" dojoType="dijit.form.Button" onClick="testIndxSome()" type="button">Is the client allowed to get the credit?</button>
    <div id="result7"></div>
