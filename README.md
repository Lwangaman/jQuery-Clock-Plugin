Turns a given div element into a dynamic clock that updates every second, main advantage compared to other similar plugins is that this one can also take an initial timestamp instead of client system time.

# DEMOS

## Client Timestamp Live examples

See a client side demo in action here at github: **http://lwangaman.github.io/jQuery-Clock-Plugin/**
You can see a demo that you can edit and play around with yourself at **http://jsbin.com/maqegib/edit?html,css,js,output**.
Unfortunately jsbin doesn't allow for server-side coding so it doesn't show the server timestamp example.

## Server Timestamp Live examples

If you would like to see a live demo that uses a server generated timestamp, take a look here: **https://www.johnromanodorazio.com/jQueryClock.php**

## NTP Timestamp Live example

A timestamp generated from an NTP server can also be used. See the example here: **https://www.johnromanodorazio.com/ntptest.php**

# USAGE

Use defaults:
```JavaScript
$("div#clock").clock();
```

## Show or hide calendar

By default prints the date together with the time, but can be used for time only:
```JavaScript
$("div#clock").clock({"calendar":"false"});
```

## Format Date and Time using PHP style Format Characters

There are two options that allow us to use PHP style Format Characters:
* "dateFormat" -> for formatting the date string
* "timeFormat" -> for formatting the time string

PHP Style Format Characters (such as those found [here](http://php.net/manual/en/function.date.php "PHP Format Characters")) supported by the ***dateFormat*** parameter are:

| Format Character  | Description                                | Example Returned values |
| ----------------- | ------------------------------------------ | ------------------- |
| *Day*             | ---                                        | ---                 |
| *d* | Day of the month, 2 digits with leading zeros            | *01* to *31*        |
| *D* | A textual representation of a day, three letters         | *Mon* through *Sun* |
| *j* | Day of the month without leading zeros                   | *1* to *31*         |
| *l* (lowercase 'L') | A full textual representation of the day of the week | *Sunday* through *Saturday* |
| *Month*           | ---                                        | ---                 |
| *F* | A full textual representation of a month, such as January or March | *January* through *December* |
| *m* | Numeric representation of a month, with leading zeros    | *01* through *12*   |
| *M* | A short textual representation of a month, three letters | *Jan* through *Dec* |
| *n* | Numeric representation of a month, without leading zeros | *1* through *12*    |
| *Year*            | ---                                        | ---                 |
| *Y* | A full numeric representation of a year, 4 digits        | Examples: *1999* or *2003* |
| *y* | A two digit representation of a year                     | Examples: *99* or *03* |

**Example:**
```JavaScript
$("div#clock").clock({"dateFormat":"D, F n, Y"});
```

PHP Style Format Characters (such as those found [here](http://php.net/manual/en/function.date.php "PHP Format Characters")) supported by the ***timeFormat*** parameter are:

| Format Character  | Description                                | Example Returned values |
| ----------------- | ------------------------------------------ | ----------------- |
| *Time*            | ---                                        | ---               |
| *a* | Lowercase Ante meridiem and Post meridiem                | *am* or *pm*      |
| *A* | Uppercase Ante meridiem and Post meridiem                | *AM* or *PM*      |
| *g* | 12-hour format of an hour without leading zeros          | *1* through *12*  |
| *G* | 24-hour format of an hour without leading zeros          | *0* through *23*  |
| *h* | 12-hour format of an hour with leading zeros             | *01* through *12* |
| *H* | 24-hour format of an hour with leading zeros             | *00* through *23* |
| *i* | Minutes with leading zeros                               | *00* to *59*      |
| *s* | Seconds with leading zeros                               | *00* to *59*      |

**Example:**
```JavaScript
$("div#clock").clock({"timeFormat":"h:i:s A"});
```

## Language options

Includes 6 language translations for days of the week and months of the year: English, French, Spanish, Italian, German, Russian. 
```JavaScript
$("div#clock").clock({"langSet":"de"});
```

The language translations can be easily extended. To add Portuguese language:
```JavaScript
$.clock.locale.pt = {
  "weekdays":["Domingo","Segunda-feira", "Terça-feira","Quarta-feira","Quinta-feira","Sexta-feira", "Sábado"],
  "shortWeekdays":["Dom","Seg","Ter","Quar","Quin","Sext"],
  "months":["Janeiro","Fevereiro","Março","Abril", "Maio","Junho","Julho","Agosto","Setembro","October","Novembro", "Dezembro"],
  "shortMonths":["Jan","Fev","Mar","Abr","Mai","Jun","Jul","Ago","Set","Oct","Nov","Dez"] 
};
```

You can then set a clock to use this language set:
```JavaScript
$("div#clock").clock({"langSet":"pt"});
```

## Custom client generated timestamp

You can pass in a custom javascript timestamp:
```JavaScript
var customDateObj = new Date();
var customtimestamp = customDateObj.getTime();
customtimestamp = customtimestamp+1123200000+10800000+14000; // sets the time 13 days, 3 hours and 14 seconds ahead
$("#clock").clock({"timestamp":customtimestamp});
```

See an example of this in action here: **http://jsbin.com/maqegib/edit?html,css,js,output**

## Custom server generated timestamp

This functionality can be useful to use a **server timestamp** (such as produced by a php script) instead of a client timestamp (such as produced by javascript).
Let's say you use php to set the value of a hidden input field to the server timestamp:
```PHP
<?php
//Get a server side unix timestamp
$time = time();
?>
<input id="servertime" type="hidden" val="<?php echo $time; ?>" />
```
Note however that **if a timezone is set in your PHP interpreter** (either using the *date.timezone = 'America/Los_Angeles'* directive in a php.ini or similar configuration file, or during runtime using *date_default_timezone_set('America/Los_Angeles')* or using *ini_set('date.timezone','America/Los_Angeles')*), then you would have to **compensate for that before feeding the timestamp** to your jQuery Clock. For example you could do this:
```PHP
<?php
//Get a server side unix timestamp compensating for timezone offset
$time = time() + date('Z');
?>
<input id="servertime" type="hidden" val="<?php echo $time; ?>" />
```
You can then start your clock using that timestamp. 
***In the latest version of the jQuery Clock plugin it is no longer necessary to compensate a server generated timestamp for the missing milliseconds by multiplying the value by 1000 before passing it into the plugin; this will be taken care of by the plugin itself, actually now it's important not to do so because the plugin will detect whether to compensate for local timezone offset or not depending on whether the timestamp is server generated or client generated.***
```JavaScript
/* Please do not do this anymore! */
//s̶e̶r̶v̶e̶r̶t̶i̶m̶e̶ ̶=̶ ̶p̶a̶r̶s̶e̶I̶n̶t̶(̶ ̶$̶(̶"̶i̶n̶p̶u̶t̶#̶s̶e̶r̶v̶e̶r̶t̶i̶m̶e̶"̶)̶.̶v̶a̶l̶(̶)̶ ̶)̶ ̶*̶ ̶1̶0̶0̶0̶;̶
/* Just do this: */
servertime = parseInt( $("input#servertime").val() );
$("#clock").clock({"timestamp":servertime});
```

See an example of this in action here: **https://www.johnromanodorazio.com/jQueryClock.php**

## Custom NTP Timeserver generated timestamp

It is also possible to use a timestamp from an NTP timeserver and start the clock with the ntp's timestamp, in order to have precise atomic time. An example of this can be found here: **https://www.johnromanodorazio.com/ntptest.php**. In the example the ntp timestamp is adjusted on the server to reflect the Europe/London timezone.

## Destroy handler

Includes a handler so that each clock can be stopped, just pass "destroy".
```JavaScript
$("div#clock").clock("destroy");
```

# Styling

The plugin adds a "jqclock" class to the dom element that the clock is applied to (usually a div). And the internal html structure that is created is like this:
```HTML
<div class="jqclock">
  <span class="clockdate"></span>
  <span class="clocktime"></span>
</div>
```
The clock can be styled accordingly in one's own stylesheet. Some sample styling is included to give an idea:
```CSS
  .jqclock { display:inline-block; text-align:center; border: 1px Black solid; background: LightYellow; padding: 10px; margin:20px; }
  .clockdate { color: DarkRed; margin-bottom: 10px; font-size: 18px; display: block;}
  .clocktime { border: 2px inset White; background: Black; padding: 5px; font-size: 14px; font-family: "Courier"; color: LightGreen; margin: 2px; display: block; }
```
