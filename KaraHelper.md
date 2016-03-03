# What is it? #

KaraHelper is special Lua package to use with Aegisub Automation. It provides new functions which allow to work with rendered subtitles.

# Installation #

Copy contents of archive to Aegisub automation folder. KaraHelper requires at least one working csri-server placed at Aegisub csri directory (by default Aegisub already have one). To use KaraHelper functions you should place _include("karahelper.lua")_ somewhere at the top of your script.

# Functions #

> _**text\_table = karahelper.text\_extents(style, text)**_

Arguments:
  * _style_ is standard Aegisub style table;
  * _text_ is text which will be rasterized.
Returns table with next fields:
  * _width_;
  * _height_;
  * _ascent_;
  * _descent_;
  * _extlead_;
  * _offsetx_.
First five fields are similar to _aegisub.text\_extents_ function returned values, but you should note that values returned by _karahelper.text\_extents_ may differ from values returned by _aegisub.text\_extents_. The last field of returned table (_offsetx_) is the offset of rendered text to the right so it may fit in returned rasterized width (it deals with _`\`blur_ and _`\`be_ tags). That's why the real position must be calculated as _x - text\_table.offsetx_.

> _**draw\_text\_str = karahelper.text\_outline(style, text)**_

Arguments are similar to previous function. Function returns string with drawing commands (_`\`p_) which will form rendered text. Doesn't deal with any tags. Also according that only integer values are allowed for drawing commands the result may be a bit awkward. So it's recommended to increase font size before rendering and scale down the resulting image.

> _**rendered\_text = karahelper.render(style, text)**_

Arguments are similar to previous function. Function returns table with next fields:
  * _width_;
  * _height_;
  * _offsetx_;
  * _color`[`x`][`y`]`_;
  * _alpha`[`x`][`y`]`_.
The result is actualy an array of dots which form the rendered text. _width_, _height_, _offsetx_ values are same to _karahelper.text\_extents_ returned values. Only _`\`blur_ or _`\`be_ tags are allowed in text. It's strongly recommended to free memory allocated for table after finishing working with it by writing _rendered\_text = nil_.

> _**rendered\_img = karahelper.loadpng(filename)**_

Works similar to previous function but now it takes an image as the parameter. Retuned table is similar to _karahelper.render_ function.

# Examples #

Check archive with KaraHelper at Downloads page. You will find a decent example inside of it.