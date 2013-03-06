Simpletap.js
============

Enables fast tap events across all elements. Requires jQuery 1.8.

See the [Creating Fast Buttons for Mobile Web Applications][buttons] article for 
the rationale of why this needs to exist.

[buttons]: https://developers.google.com/mobile/articles/fast_buttons

Usage
-----

Simply call it like so:

``` javascript
$.simpletap();
```

And *all* elements across the page will automatically get a 'tap' event.

``` javascript
$('a').on('tap', function() {
  console.log('tap!');
});
```

Tap events will also be triggered when you click.

Advanced usage
--------------

You can specify custom options. Note that these are *all optional*.

``` javascript
$.simpletap({
  for: '*',            // Restrict tappable objects to this selector
  threshold: 10,       // Maximum finger distance for taps in pixels
  timeout: 400,        // Amount of time to supress clicks
  event: 'tap',        // Event name to be triggered
  activeClass: 'tap',  // CSS class to add to tapped things
  emulateTaps: true,  // Emulate taps when clicks happen
  stopClicks: false    // Stop all click events from happening
});
```

Caveats & recommendations
-------------------------

### Restricting elements

Only the top-most element will be triggered. This means if you have an
element like so:

``` html
<button class='btn primary'>
  <i class='trash'></i> Delete
</button>
```

...and you click on the trash icon, it will be the `<i>` that will receive
the `tap` event, not `button`.

To get around this, restrict the tappable element selector to those you're
concerned about:

``` javascript
$.simpletap({ 'for': 'button' });
```

### Devices that support touch and click

Some devices support both. For instance, there are Lenovo PCs with touch screens 
and pointing devices.

In this case, it behaves like so:

 * Touch taps produce `tap` event.
 * Clicks produce both `tap` and `click` by default.
 * If the `emulateTaps` option is disabled, clicks will only produce clicks.

### Stopping links

When attaching tap events to links and buttons, stopping isn't really 
straightforward.

``` javascript
$('a').on('tap', function(e) {
  e.preventDefault();   // Will NOT work
});
```

You'll need to listed to the `click` event as well. This will, however, trigger 
twice (since clicks will produce taps as well), so you'll have to turn off the 
`emulateTaps` option.

``` javascript
$.simpletap({ emulateTaps: false });

$('a').on('click tap', function(e) {
  e.preventDefault();   // Will work!
});
```

You can tell Simpletap to stop all clicks by default.

``` javascript
$.simpletap({
  'for': 'a, button',
  'stopClicks': true
});
```

Styling tapped objects
----------------------

The usual `:active` classes will not work right away. Just style the `.tap`
class. Simpletap adds the `tap` class to tapped elements by default.

``` javascript
a.tap {
  background: #333;
}
```

Limitations
-----------

Only handles taps. Other touch events like pinching and swiping are not handled.
Other libraries exist that may getSaround Simpletap's limitations:

 * [Touchable](https://github.com/dotmaster/Touchable-jQuery-Plugin):
   Handles other touch events.

 * [Tappable](https://github.com/cheeaun/tappable):
   Has more options and events, and implements a *noScroll* mode.

Acknowledgements
----------------

© 2013, Nadarei, Inc. Released under the [MIT 
License](http://www.opensource.org/licenses/mit-license.php).

**Simpletap** is authored and maintained by [Rico Sta. Cruz][rsc] and Michael
Galero with help from its [contributors][c]. It is sponsored by our startup, 
       [Nadarei, Inc][nd].

[rsc]: http://ricostacruz.com
[c]:   http://github.com/nadarei/simpletap.js/contributors
[nd]:  http://nadarei.co
