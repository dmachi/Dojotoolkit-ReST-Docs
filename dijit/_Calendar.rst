.. _dijit/_Calendar:

dijit._Calendar
===============

:Status: Contributed, Draft
:Version: 1.3
:Authors: Adam Peller
:Developers: Adam Peller

.. contents::
    :depth: 2

The _Calendar widget displays a localized month-view calendar and allows the user to navigate months and years and select a date.  It is typically used as part of the :ref:`DateTextBox <dijit/form/DateTextBox>` which includes a text box and uses the _Calendar as a drop-down. Localizations for hundreds of languages and cultures are included as part of the Dojo Toolkit.  The locale will be chosen based on the djConfig.locale setting of your page, using navigator.language by default.  To meet the accessibility requirements of the Dijit project, which requires keyboard accessibility on input controls, the _Calendar must be used in conjunction with :ref:`DateTextBox <dijit/form/DateTextBox>`. For this reason, the _Calendar widget is not supported standalone and is prefixed with an underscore ("_").  However, a standalone calendar can easily be added to a page as seen in the demo below.

Navigating between months is possible with the arrow icons, and the next and previous year may be clicked to change to that year.  Holding the mouse down on these controls will repeat the action.  In version 1.4+, the month name is a drop-down control for selecting a different month.

Non-Gregorian calendar use is possible using the datePackage attribute and experimental date classes in :ref:`dojox.date <dojox/date>`.  


========
Examples
========

A plain Calendar widget with the formatted date below

.. cv-compound::
  :type: inline
  :height: 350

  .. cv:: javascript

    <script type="text/javascript">
      dojo.require("dijit.dijit"); // loads the optimized dijit layer
      dojo.require("dijit._Calendar");
    </script>

  .. cv:: html

    <div dojoType="dijit._Calendar" onChange="dojo.byId('formatted').innerHTML=dojo.date.locale.format(arguments[0], {formatLength: 'full', selector:'date'})"></div>
    <p id="formatted"></p>
    
  .. cv:: css

    <style type="text/css">
      .{{ theme }} table.dijitCalendarContainer {
        margin: 25px auto;
      }
      #formatted {
        text-align: center;
      }
    </style>

With an initial selection and weekends disabled

.. cv-compound::
  :height: 320
  :version: 1.4

  .. cv:: javascript

    <script type="text/javascript">
      dojo.require("dijit.dijit"); // loads the optimized dijit layer
      dojo.require("dijit._Calendar");
    </script>

  .. cv:: html

    <div id="mycal" dojoType="dijit._Calendar" value="2009-08-07" isDisabledDate="dojo.date.locale.isWeekend"></div>
    
  .. cv:: css

    <style type="text/css">
      .{{ theme }} .dijitCalendarDisabledDate {
        background-color:#333 !important;
        text-decoration:none !important;
      }

      .{{ theme }} table.dijitCalendarContainer {
        margin: 25px auto;
      }
    </style>

Javascript declaration, with a restriction of +/- one week from the current date

.. cv-compound::
  :height: 320

  .. cv:: javascript

    <script type="text/javascript">
      dojo.require("dijit.dijit"); // loads the optimized dijit layer
      dojo.require("dijit._Calendar");

	dojo.addOnLoad(function(){
		new dijit._Calendar({
			value: new Date(2009, 7, 7),
			isDisabledDate: function(d){
				var d = new Date(d); d.setHours(0,0,0,0);
				var today = new Date(); today.setHours(0,0,0,0);
				return Math.abs(dojo.date.difference(d, today, "week")) > 0;
			}
		}, "mycal");
	});
    </script>

  .. cv:: html

    <div id="mycal"></div>
    
  .. cv:: css

    <style type="text/css">
      .{{ theme }} table.dijitCalendarContainer {
        margin: 25px auto;
        width: 200px;
      }
    </style>

With a local custom template to change the layout (does not work against CDN)

.. cv-compound::
  :height: 600
  :version: 9.99

  .. cv:: javascript

    <script type="text/javascript">
      dojo.require("dijit.dijit"); // loads the optimized dijit layer
      dojo.require("dijit._Calendar");

	dojo.addOnLoad(function(){
		//Need to declare BigCalendar here in an addOnLoad block so that it works
		//with xdomain loading, where the dojo.require for dijit._Calendar 
		//may load asynchronously. This also means we cannot have HTML
		//markup in the body tag for BigCalendar, but instead inject it in this
		//onload handler after BigCalendar is defined.
		dojo.declare("BigCalendar", dijit._Calendar, {
				templatePath: "http://docs.dojocampus.org/dijit/_Calendar?action=AttachFile&do=get&target=bigCalendar.html",
				templateString: null,  /* need this for builds */
				getClassForDate: function(date){
					if(!(date.getDate() % 10)){ return "blue"; } // apply special style to all days divisible by 10
				}
		});

		var bigCalendar = dojo.byId("calendar5");
		bigCalendar.setAttribute("dojoType", "BigCalendar");
		dojo.parser.parse(bigCalendar.parentNode);
	});
    </script>

  .. cv:: css

	<style>
		#calendar5 .dijitCalendarDateTemplate { height: 50px; width: 50px; border: 1px solid #ccc; vertical-align: top }
		#calendar5 .dijitCalendarDateLabel, #calendar5 .dijitCalendarDateTemplate { text-align: inherit }
		#calendar5 .dijitCalendarDayLabel { font-weight: bold }
		#calendar5 .dijitCalendarSelectedYear { font-size: 1.5em }
		#calendar5 .dijitCalendarMonthLabel { font-family: serif; letter-spacing: 0.2em; font-size: 2em }
		.blue { color: blue }
                .{{ theme }} table.dijitCalendarContainer {
                  margin: 25px auto;
                }
	</style>

  .. cv:: html

	<input id="calendar5" dayWidth="abbr" value="2008-03-13">
        

[experimental: 1.4+] Non-Gregorian calendars

.. cv-compound::
  :version: 1.4
  :height: 340

  .. cv:: javascript

    <script type="text/javascript">
      dojo.require("dijit.dijit"); // loads the optimized dijit layer
      dojo.require("dijit._Calendar");

      dojo.require("dojox.date.hebrew");
      dojo.require("dojox.date.hebrew.Date");
      dojo.require("dojox.date.hebrew.locale");

      dojo.require("dojox.date.islamic");
      dojo.require("dojox.date.islamic.Date");
      dojo.require("dojox.date.islamic.locale");

      var publishing = false;

      function publishDate(d){
        if(!publishing){
          publishing = true;
          dojo.publish("date", [{date: d.toGregorian ? d.toGregorian() : d, id: this.id}]);
          publishing = false;
        }
      }

      dojo.subscribe("date", function(data){
        dijit.registry.filter(function(widget){ return widget.id != data.id; }).forEach(function(widget){ widget.attr('value', data.date); });
      });

      function formatDate(d) {
        var datePackage = (this.id == "gregorian") ? dojo.date : dojox.date[this.id];
	dojo.byId(this.id+"Formatted").innerHTML = datePackage.locale.format(arguments[0], {
          formatLength: 'long',
          selector: 'date'
        });
      }
    </script>

  .. cv:: html

    <table class="container">
      <tr>
        <td>
          <div id="hebrew" dojoType="dijit._Calendar" datePackage="dojox.date.hebrew" onValueSelected="publishDate" onChange="formatDate"></div>
          <div id="hebrewFormatted"></div>
        </td>
        <td>
          <div id="islamic" dojoType="dijit._Calendar" datePackage="dojox.date.islamic" onValueSelected="publishDate" onChange="formatDate"></div>
          <div id="islamicFormatted"></div>
        </td>
        <td>
          <div id="gregorian" dojoType="dijit._Calendar" onValueSelected="publishDate" onChange="formatDate"></div>
          <div id="gregorianFormatted"></div>
        </td>
      </tr>
    </table>

  .. cv:: css

    <style type="text/css">
      .{{ theme }} table.dijitCalendarContainer {
        margin: 25px auto;
      }
    </style>

========
See Also
========

  * :ref:`dojox.widget.Calendar <dojox/widget/Calendar>` - An enhanced but still experimental version of Calendar which has additional capabilities like year-only views and animation effects.
