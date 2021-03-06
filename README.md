Bootstrap Calendar
===

A Full view calendar based on Twitter Bootstrap. Please try the [demo](http://bootstrap-calendar.azurewebsites.net).

![Bootstrap full calendar](http://serhioromano.s3.amazonaws.com/github/bs-calendar.png)

### Why?

Why did I start this project? Well, I believe there are no good full view calendar's out there with native Bootstrap support. In fact I could not find even one. A different UI and UX concept approach is also used.


### Features

- **Reusable** - there is no UI in this calendar. All buttons to switch view or load events are done separately. You will end up with your own uniquie calendar design.
- **Template based** - all view like **year**, **month**, **week** or **day** are based on templates. You can easily change how it looks or style it or even add new custom view.
- **LESS** - easy adjust and style your calendar with less variables file.
- **AJAX** - It uses AJAX to feed calendar with events. You provide URL and just return by this URL `JSON` list of events.
- **i18n** - language files are connected separately. You can easily translate the calendar into your own language. Holidays are also diplayed on the calendar according to your language

## How to use

### Install

You can install it with [bower](http://twitter.github.com/bower/) package manager.

	$ bower install bootstrap-calendar

Bower will automatically install all dependencies. Then by running

	$ bower list --path

You will see list of the files you need to include to your document.

### Quick setup
You will need to include the bootstrap css and calendar css. Here is the minimum setup.

	<!DOCTYPE html>
	<html>
	<head>
		<title>Minimum Setup</title>
		<link rel="stylesheet" href="css/bootstrap.min.css">
		<link rel="stylesheet" href="css/calendar.css">
	</head>
	<body>

		<div id="calendar"></div>

		<script type="text/javascript" src="js/vendor/jquery-1.9.1.js"></script>
		<script type="text/javascript" src="js/vendor/underscore-min.js"></script>
		<script type="text/javascript" src="js/calendar.js"></script>
		<script type="text/javascript">
			var calendar = $('#calendar').calendar();
		</script>
	</body>
	</html>

Bootstrap Calendar depends on [jQuery](http://jquery.com/) and [underscore.js](http://underscorejs.org/) is used as a template engine.
For the calendar you only have to include the `calendar.css` and `calendar.js` files.
If you want to localize your Calendar, it's enough to add this line before including calendar.js:

	<script type="text/javascript" src="js/language/xx-XX.js"></script>

Where xx-XX is the language code. When you initializing the calendar, you have to specify this language code:

	<script type="text/javascript">
		var calendar = $('#calendar').calendar({language: 'xx-XX'});
	</script>




### Feed with events

To feed the calendar with events you need to provide a URL where AJAX request will be sent.

	var calendar = $('#calendar').calendar({events_url:'/api/events.php'});

It will `GET` two parameters, `from` and `to`, which will tell you what period is required. You have to return it in JSON structure like this

	{
		"success": 1,
		"result": [
			{
				"id: 293,
				"title": "Event 1",
				"url": "http://someurl.com",
				"class": 'event-important',
				start: 12039485678000, // Milliseconds
				end: 1234576967000 // Milliseconds
			},
			...
		]
	}

See `events.json.php` file for more details. `start` and `end` contain dates when event starts (inclusive) and ends (exclusive) in Unix timestamp. Classes are `event-important`, `event-success`, `event-warning`, `event-info`, `event-inverse` and `event-special`. This wil change the color of your event indicators.

Note that `start` and `end` dates are in milliseconds, thus you need to divide it by 1000 to get seconds. PHP example.

    $start = date('Y-m-d h:i:s', ($_GET['start'] / 1000));

If you have an error you can return

	{
		"success": 0,
		"error": "error message here"
	}


### Usage warning.

You cannot use the calendar from a local file. 
The following error will be displayed :
Failed to load resource: Origin null is not allowed by Access-Control-Allow-Origin. 

Using Ajax with local resources (file:///), is not permited. You will need to deploy this to the web instead.
