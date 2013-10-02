Google Analytics
==============================

Tracking an AJAX Request as a Pageview

In your create.js.erb:
```javascript
_gaq.push(['_trackPageview', '/users']);
```

Then in Google Analytics, do the following:

Reporting > Conversions > Goals > Goal URL > New Goal > Destination Goal

Use Regular Expression tracking: '/users'
