#GAP - Google Analytics automatic Push [![Build Status](https://travis-ci.org/rockymadden/gap.png?branch=master)](https://travis-ci.org/rockymadden/gap)
Dead simple wrapper around the Google Analytics API which provides automatic tracking of user behaviors via events. Want to see the event tracking in action? Open up your JavaScript console and head to the [project site](http://rockymadden.com/gap/).

##Installation
There is no need to include any of the default Google Analytics tracking code, as GAP takes care of all this for you. Simply [download the gap.min.js](https://raw.github.com/rockymadden/gap/master/gap.min.js) file, place on your server, and update the noted fields below. That's it!

```javascript
var _gap = _gap || [];

// Standard Google Analytics pushes, just with _gap.push. 
// You can push anything, just like normal, via _gap.push instead of _gaq.push.
_gap.push(["_setAccount", "UA-XXXXXX-X"]); // CHANGE.
_gap.push(["_trackPageview"]);

// With both bounce trackers active, any user who scrolls down 50%
// OR stays 10 seconds is not counted as a bounce. You can use just one too.
_gap.push(["_gapTrackBounceViaTime", 10]); // Change seconds, if needed.
_gap.push(["_gapTrackBounceViaScroll", 50]); // Change percentage, if needed.

// Every 20 seconds, push a read event so that time on site is more accurate.
// Only allow 30 of these read events per page.
_gap.push(["_gapTrackReads", 20, 30]); // Change cadence and max read events per page, if needed.

// Anytime a user clicks a link, internal or external, push the event.
_gap.push(["_gapTrackLinkClicks"]);

(function() {
	var gap = document.createElement("script");
	gap.async = true;
	gap.type = "text/javascript";
	gap.src = "/js/gap.min.js"; // CHANGE.

	var s = document.getElementsByTagName("script")[0];
	s.parentNode.insertBefore(gap, s);
})();
```

## Trackers

__gapTrackBounceViaScroll__

Logs a single ```gapBounceViaScroll``` event after scrolling n percentage down the page. This event will only fire once per session.

If you do not consider users who fully scroll (to your specified requirement) down your page a bounce, this tracker can help you. The default implementation of the Google Analytics tracking code has some undesirable traits. The following scenarios help illustrate: 

* User A arrives on your website. He finds a wealth of information he is looking for on the first landing page. He fully scrolls/reads the entire page and then departs. By default, this is considered a bounce.
* More information on this subject, [straight from Google](http://analytics.blogspot.com/2012/07/tracking-adjusted-bounce-rate-in-google.html).


```javascript
_gap.push([
	"_gapTrackBounceViaScroll",
	50 // Percentage of the page scrolled before this user is not considered a bounce.
]);
```

-----

__gapTrackBounceViaTime__

Logs a single ```gapBounceViaTime``` event after n seconds. This event will only fire once per session.

If you do not consider users who spend long durations on their first and only page visit on your website a bounce, this tracker can help you. The default implementation of the Google Analytics tracking code has some undesirable traits. The following scenarios help illustrate: 

* User A arrives on your website. She finds a wealth of information she is looking for on the first landing page. She spends 15 minutes reading and then departs. By default, this is considered a bounce.
* More information on this subject, [straight from Google](http://analytics.blogspot.com/2012/07/tracking-adjusted-bounce-rate-in-google.html).


```javascript
_gap.push([
	"_gapTrackBounceViaTime",
	10 // Number of seconds before this user is not considered a bounce.
]);
```

-----

__gapTrackReads__

Logs ```gapRead``` events every n seconds. Each new event label is updated as time progresses. For instance, with a 10 second cadence the first event label would be 10, the second would be 20, and so on.

If you are puzzled as to why so many of your users have a time on site of 0 seconds, this tracker can help you. If you want to accurately test changes to your website and how they truly affect user engagement, this tracker can help you. The default implementation of the Google Analytics tracking code has some undesirable traits. The following scenarios help illustrate: 

* User A arrives on your website. He finds a wealth of information he is looking for on the first landing page. He spends 5 minutes reading and then departs. By default, the time on site for this user is 0 seconds.
* User B arrives on your website. He doesn't find what he is looking for on the first page, so 10 seconds after arriving he lands on the second, and final, page of his visit. He finds a wealth of information, and he spends 30 minutes reading and then departs. By default, the time on site for this user is 10 seconds.
* You have redesigned your website hoping to improve user engagement (e.g. increase time on site and lower bounce rate). Because you used the default implementation of the Google Analytics code, your bounce rate is over stated and your time on site is under stated. After the redesign, you see positive numbers on both these fronts. Bounce rate is down and time on site is up. Both these data points are not only incorrect, but the opposite could be true. Your time on site could, in actuality, be lower than before. Your bounce rate could, in actuality, be higher than before.
* More information on this subject, [straight from Google](http://analytics.blogspot.com/2012/07/tracking-adjusted-bounce-rate-in-google.html).

Polling cadence can be changed as well as the number of events total this tracker can utilize per page. There is a limit of [500 total events tracked per session](https://developers.google.com/analytics/devguides/collection/gajs/limits-quotas), so keep this in mind.

```javascript
_gap.push([
	"_gapTrackReads",
	20, // Polling cadence, in seconds.
	30 // Maximum number of these events tracked per page.
]);
```

-----

__gapTrackLinkClicks__

Logs ```a``` and ```button``` mousedown events (i.e. user clicks) via event delegation. Event action logged is ```gapLinkClick```. Event label logged is ```#{linkName} (#{linkURL})```.

```javascript
_gap.push(["_gapTrackLinkClicks"]);
```

## Versioning
[Semantic Versioning v2.0](http://semver.org/)

## License
[Apache License v2.0](http://www.apache.org/licenses/LICENSE-2.0)