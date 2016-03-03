# Introduction #
Lua extension allows:
- redefinition line style from lua script;
- definition of vector clip style (rotation, scaling, shearing);
- editing points position before rasterizing;
- coloring subtitles.

# Binding .lua to .ass file #
Lua script can be attached to ass file in header with such line:
```
Lua: <filename>
```

Log file with errors can be setup with such line:
```
LuaLog: <filename>
```

Example:
```
[Script Info]
...
PlayResX: 640
PlayResY: 480
LuaLog: animate.txt
Lua: animate.lua
...
[V4+ Styles]
```

# Initialization of script #
After loading vsfilter call "init" function without any arguments (if it exists):
```
function init()

end
```

# Custom tag realization #
All global function after loading can be used as tags:
```
function somemethod(line,args) -- \somemethod(123) or \lua(somemethod,123)

end
```

Arguments of tag function:
  * line - table with current line information:
    * "time" - time from start of line;
    * "start" - line start time;
    * "end" - line end time;
    * "length" - line duration;
    * "id" - line unique index;
    * ["animate"] - (0..1) percent of animation through \t tag (only for tags inside \t)
    * "user" - table with user data for this line

  * args - all tag arguments (as strings) from subtitle file
\somemethod(123, 22, adf) call in lua as somemethod(line, "123", "22", "adf")

Function should return table with new line style information (only needed fields):
  * "style" - table with new style information:
    * "fs"        (number) - font size (\fs)
    * "bold"      (bool)   - bold (\b)
    * "italic"    (bool)   - italic (\i)
    * "underline" (bool)   - underline (\u)
    * "strikeout" (bool)   - strikeout (\s)
    * "blur"      (number) - gaussian blur (\blur)
    * "be"        (number) - edge blur (\be)
    * "frx"       (number) - rotation x (\frx)
    * "fry"       (number) - rotation y (\fry)
    * "frz"       (number) - rotation z (\frz)
    * "fax"       (number) - shearing x (\fax)
    * "fay"       (number) - shearing y (\fay)
    * "fscx"      (number) - scale x (\fscx)
    * "fscy"      (number) - scale y (\fscy)
    * "fsc"       (number) - scale (\fsc)
    * "shadx"     (number) - shadow x (\shadx)
    * "shady"     (number) - shadow y (\shady)
    * "shad"      (number) - shadow (\shad)
    * "bordx"     (number) - outline size x (\bordx)
    * "bordy"     (number) - outline size y (\bordy)
    * "bord"      (number) - outline size (\bord)
    * "a1"        (number) - primary alpha (\1a)
    * "a2"        (number) - secondary alpha (\2a)
    * "a3"        (number) - outline alpha (\3a)
    * "a4"        (number) - shadow alpha (\4a)
    * "c1"        (number) - primary color (\1c)
    * "c2"        (number) - secondary color (\2c)
    * "c3"        (number) - outline color (\3c)
    * "c4"        (number) - shadow color (\4c)
    * "rndx"      (number) - random x (\rndx)
    * "rndy"      (number) - random y (\rndy)
    * "rndz"      (number) - random z (\rndz)
    * "rnd"       (number) - random (\rnd)
    * "fsp"       (number) - horisontal spacing (\fsp)
    * "fsvp"      (number) - vertical spacing (\fsvp)
    * "frs"       (number) - symbol rotation (\frs)

  * "pos" - table with new line position
    * "x"         (number) - x position
    * "y"         (number) - y position

  * "org" - table with new line origin
- "x"         (number) - x origin position
- "y"         (number) - y origin position

  * "vcpos" - table with new vector clip position
    * "x"         (number) - x vc position
    * "y"         (number) - y vc position

  * "user" - user table with any data, will be transmitted to all next functions of this line as argument (line.user)

  * "beforetransform" (string) - custom point transformation function name, which called before main transformation function;
  * "aftertransform"  (string) - custom point transformation function name, which called after main transformation function;
  * "customtransform" (string) - custom point transformation function name, which called instead main transformation function;
  * "clipstyle"       (string) - custom clip style definition function name;
  * "renderer"        (string) - custom colorizer function name.

# Custom transform realization #
Custom transform function can be called before, instead and after vsfilter transformation function (void CWord::Transform(CPoint org)).
Function would be called for every point in path.
```
function customtransform(line)

end
```

All coordinates (x and y) are multiplied by 8 (overscale).
Arguments of transform function:
line - table with current line information:
  * "layer" - active layer: 1 - text, 3 - outline, 4 - shadow, 5 - opaque box, 6 - clip;
    * "id"    - line unique index;
    * "minx"  - minimal x value
    * "miny"  - minimal y value
    * "maxx"  - maximal x value
    * "maxy"  - maximal y value

  * "frx"   - x rotation angle (degree)
  * "fry"   - y rotation angle (degree)
  * "frz"   - z rotation angle (degree)

  * "fscx"  - x scale (100 == normal)
  * "fscy"  - y scale (100 == normal)

  * "fax"   - x shearing
  * "fay"   - y shearing

  * "size"  - table with size information:
    * "w"   - width
    * "h"   - height

  * "pos"   - table with point position
    * "x"   - x coordinate
    * "y"   - y coordinate
    * "z"   - z coordinate

  * "org"   - table with line origin
    * "x"   - x coordinate
    * "y"   - y coordinate

  * "user"  - table with user info (please, get and modify table from line.user, if it exists)

Function should return table with new point position:
  * "x" (number) - new x coordinate
  * "y" (number) - new y coordinate

# Clip styling realization #
Adjusting inacccesible from ass-subtitles vector clip style options.
```
function clipstyling(line)

end
```

Arguments of transform function:
line - table with current line information:
- "id"    - line unique index;
- "user"  - table with user info.

Function should return table with new line style information (only needed fields):
  * "style" - table with new style information:
    * "frx"       (number) - rotation x (\frx)
    * "fry"       (number) - rotation y (\fry)
    * "frz"       (number) - rotation z (\frz)
    * "fax"       (number) - shearing x (\fax)
    * "fay"       (number) - shearing y (\fay)
    * "fscx"      (number) - scale x (\fscx)
    * "fscy"      (number) - scale y (\fscy)
    * "fsc"       (number) - scale (\fsc)
    * "rndx"      (number) - random x (\rndx)
    * "rndy"      (number) - random y (\rndy)
    * "rndz"      (number) - random z (\rndz)
    * "rnd"       (number) - random (\rnd)

  * "pos" - table with new line position
    * "x"         (number) - x position
    * "y"         (number) - y position

  * "org" - table with new line origin
    * "x"         (number) - x origin position
    * "y"         (number) - y origin position

  * "user" - user table with any data, will be transmitted to all next functions of this line as argument (line.user)

  * "beforetransform" (string) - custom point transformation function name, which called before main transformation function (for vector clip);
  * "aftertransform"  (string) - custom point transformation function name, which called after main transformation function (for vector clip);
  * "customtransform" (string) - custom point transformation function name, which called instead main transformation function (for vector clip);

# Colorizing realization #
Function for specifying individual pixels color.
```
function colorizer(line,rend)

end
```

Arguments of colorizer function:
  * "line" - line options
    * "id"     - line unique index;
    * "layer"  - active layer: 1 - text, 3 - outline, 4 - shadow, 5 - opaque box;
    * "width"  - image width;
    * "height" - image height;
    * "gran"   - position of border (in \k tag) between primary and secondary colors;
    * "c1"     - primary color;
    * "c2"     - secondary color;
    * "a1"     - primary alpha;
    * "a2"     - secondary alpha;
    * "user"   - table with user info.

  * "rend" - renderer object
    * "get"    - function for getting color from surface (local alpha, color = rend:get(x,y))
    * "mix"    - function for mixing specified color with color from surface (rend:mix(x, y, color, alpha))