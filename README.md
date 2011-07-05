## jQuery Appear

Mimics a custom "appear" event, which fires when an element scrolls into view or otherwise becomes visible to the user.

``` js
$('#foo').appear(function() {
  $(this).text('Hello world');
});
```

This plugin can be used to prevent unnecessary requests for content that's hidden or outside the viewable area.


## Examples: Image Gallery

Load images when the user has scrolled to them.

Markup

``` html
<!-- Content that takes up the viewable area -->
<div>Lorem ipsum...</div>

<!-- Content that appears below the fold -->
<div id="gallery">
  <div><a href="/images/image1.jpg">Image 1</a></div>
  <div><a href="/images/image2.jpg">Image 2</a></div>
  <div><a href="/images/image3.jpg">Image 3</a></div>
  <div><a href="/images/image4.jpg">Image 4</a></div>
  <div><a href="/images/image5.jpg">Image 5</a></div>
  <div><a href="/images/image6.jpg">Image 6</a></div>
</div>
```

Script

``` js
$(document).ready(function() {
  $('#gallery div').appear(function() {
    var a = $(this).find('a');
    $(this).html('<img src="' + a.attr('href') + '" title="' + a.text() + '" />');
  });
});
```

### Basic: Append Content with Ajax

Append HTML content to the page as the user scrolls to the end.

Markup

``` html
<!-- Content that takes up the viewable area -->
<div>Lorem ipsum...</div>

<!-- Content that appears below the fold -->
<div id="articles">
  <div><a href="/articles/article1.html">Article 1</a></div>
  <div><a href="/articles/article2.html">Article 2</a></div>
  <div><a href="/articles/article3.html">Article 3</a></div>
  <div><a href="/articles/article4.html">Article 4</a></div>
  <div><a href="/articles/article5.html">Article 5</a></div>
  <div><a href="/articles/article6.html">Article 6</a></div>
</div>
```

Script

``` js
$(document).ready(function() {
  $('#articles div').appear(function() {
    var a = $(this).find('a');
    $(this).load(a.attr('href'));
  });
});
```

### Advanced: Image Search

An image search similar to Microsoft Bing.  Initially the user sees four images.  As he scrolls down to the next four images, the next four results are loaded into the dom, and so on as he keeps scrolling.

``` html
<style type="text/css">
/* the summary always visible */
#summary {
  position: fixed;
  height: 2em;
  width: 100%;
}
#results {
  padding-top: 2em;
}
</style>

<script type="text/javascript" src="jquery-1.3.2.min.js"></script>
<script type="text/javascript" src="jquery.appear-1.1.1.min.js"></script>
<script type="text/javascript">
var offset = 0;
var current = 0;
var query = 'Las Vegas'; //should come from the server
var total = 500; //should come from the server
var perPage = 4; //this should be updated as the user resizes the window, 
                 //but let's assume there's always 4 per page for simplicity

function updateSummary() {

  //update the current offset
  current = parseInt($(this).attr('offset'));

  //update the summary text
  $('#summary').text((current+1) + '-' + (current+perPage) + ' of ' + total + ' results for' + query);
}

function loadMore() {
  
    //move the images in #more to #current
    $(this).appendTo('#current');
    
    //update the summary whenever this area appears
    $(this).appear(updateSummary, { 
      one: false
    };
    
    //adjust the offset
    offset = parseInt($(this).attr('offset'));
    
    //load the next results into #more
    $('#more').load('/search?q=' + query + '&offset=' + (offset+perPage), function() {
    
      //load more results when the user scrolls to this section
      $(this).find('.section').appear(loadMore);
    });
    
}

$(function() {
  
  //update the summary whenever this area appears
  $('#current .section').appear(updateSummary, { 
    one: false
  };
  
  //load more results when the user scrolls to this section
  $('#more .section').appear(loadMore);
  
});
</script>

<div id="summary"></div>

<div id="results">

  <!-- displayed at the start -->
  <div id="current">
    <div class="section" offset="0">
      <a href="full1.jpg"><img src="thumb1.jpg"/></a>
      <a href="full2.jpg"><img src="thumb2.jpg"/></a>
      <a href="full3.jpg"><img src="thumb3.jpg"/></a>
      <a href="full4.jpg"><img src="thumb4.jpg"/></a>
    </div>
  </div>

  <!-- loaded dynamically -->
  <div id="more">
    <div class="section" offset="4">
      <a href="full5.jpg"><img src="thumb5.jpg"/></a>
      <a href="full6.jpg"><img src="thumb6.jpg"/></a>
      <a href="full7.jpg"><img src="thumb7.jpg"/></a>
      <a href="full8.jpg"><img src="thumb8.jpg"/></a>
    </div>
  </div>

</div>
```






## API

### `appear(fn)`

Call a function `fn` when an element first appears.

``` js
$('#foo').appear(function() {
  $(this).text('Hello world');
});
```

### `appear(fn, options)`

Call a function `fn` when an element appears, with the following options.

  * *`one`* whether to fire the event one time only, or every time it appears _(default: `true`)_
  * *`data`* arbitrary data to be passed as arguments to the function attached to the event _(default: `undefined`)_

``` js
$('#foo').appear(function(event, a, b, c) {
  $(this).text(a + ' ' + b + ' ' + c);
}, { 
  one: false,
  data: [1, 'bar', true]
});
```

### `appear()`

Fire an element's `appear` event.

``` js
$('#foo').appear();
```



## Changelog

### 1.2.1

  * Minor fixes

### 1.2.0

  * Added `disappear` binding

### 1.1.1

  * Bug fixes / optimizations from previous version
  * You can now fire an element's appear event by calling `element.appear()`
  * Optional settings as a second parameter - `element.appear(fn, options)`
    * `one`: whether to fire the appear event every time the element becomes visible, or just the first time _(default: true)_
    * `data`: arbitrary data to be passed as arguments to the function when the event is fired

### 1.1

  * Listens for dom and style changes in addition to binding to the window scroll
  * Fires appear only when element is (a) in the viewable window and (b) `element.is(':visible')` == true.

### 1.0

  * Very basic implementation.  Only binds to the window's scroll event.



## Copyright

jQuery.appear
http://code.google.com/p/jquery-appear/

Copyright (c) 2009 Michael Hixson

Licensed under the [MIT license](http://www.opensource.org/licenses/mit-license.php)
