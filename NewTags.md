# List of new override tags #

## Font scale ##
> _\fsc`<`scale`>`_

Similar to aggregate \fscx`<`scale`>`\fscy`<`scale`>`. Animatable by \t.

**Example**
> _\fsc200_ — make the text double size.

## Leading ##
> _\fsvp`<`leading`>`_

Changes text leading. Animatable by \t.

**Example**
> ![http://s57.radikal.ru/i157/0905/b7/7119a48b167e.png](http://s57.radikal.ru/i157/0905/b7/7119a48b167e.png)

## Baseline obliquity ##
> \frs`<`angle`>`

Character's baseline obliquity. Rotation anchor depends from style definition and \an tag. Animatable by \t.

**Example**
> ![http://img704.imageshack.us/img704/325/87069744.png](http://img704.imageshack.us/img704/325/87069744.png) ![http://img692.imageshack.us/img692/2739/13259861.png](http://img692.imageshack.us/img692/2739/13259861.png)

## Z coordinate ##
> _\z`<`arg`>`_

Sets z coordinate. It may be signified as a distance from screen to text. It's noticeable in case of using \frx and \fry tags. Animatable by \t.

## Distortion ##
> _\distort(u1,v1,u2,v2,u3,v3)_

Distorts text by moving corner pins to specifed relative coordinates. Animatable by \t.

![http://s54.radikal.ru/i144/0905/74/4fa7679d3448.png](http://s54.radikal.ru/i144/0905/74/4fa7679d3448.png)

**Example**
> ![http://s60.radikal.ru/i167/0905/ac/ce9b31019906.png](http://s60.radikal.ru/i167/0905/ac/ce9b31019906.png)

## Boundaries deforming ##
> _\rnd`<`arg`>`_

> _\rndx`<`arg`>`_

> _\rndy`<`arg`>`_

> _\rndz`<`arg`>`_

Moves border points on random number of pixels from (-arg,arg) interval in selected direction. Animatable by \t.

**Example**

> ![http://s51.radikal.ru/i131/0905/b5/51d50dbfe20b.png](http://s51.radikal.ru/i131/0905/b5/51d50dbfe20b.png)

## Gradients ##
> _\$vc(left-top-color,right-top-color,left-bottom-color,right-bottom-color)_

> _\$va(left-top-transparency,right-top-transparency,left-bottom-transparency,right-bottom-transparency)_

Creates gradients by using anchor colors or opacity levels. May be slow. Animatable by \t.

![http://s56.radikal.ru/i152/0905/8e/6e0b6374e7f9.png](http://s56.radikal.ru/i152/0905/8e/6e0b6374e7f9.png)

**Example**
> ![http://s56.radikal.ru/i152/0905/e7/fff25b6247fd.png](http://s56.radikal.ru/i152/0905/e7/fff25b6247fd.png)

## Images instead of color fills ##
> _\$img(path\_to\_png\_file`[`,xoffset,yoffset`]`)_

Replaces color fill with repeated image pattern. Parameters are slashed (/) path to image and optional fill's base offset. Path may be relative to current subtitle file location. First VSFilterMod loads attached images and if fails trys to find them localy. VSFilterMod supports only 24 or 32 bit truecolor pngs with or without transparency channel. Offset is animatable by \t. Note that \be and \blur tag will not blur image but only mask which is used to place fill.

**Example**

> ![http://img641.imageshack.us/img641/6733/10424465.png](http://img641.imageshack.us/img641/6733/10424465.png)

> ![http://img534.imageshack.us/img534/6879/64529891.png](http://img534.imageshack.us/img534/6879/64529891.png) and _{\3vc(&HFF00FF&,&HFFFF00&,&H00FFFF&,&HFFFFFF&)\1img(Z:/subs/as.png,0,0)\pos(300,250)\bord10\p1}m -150 0 b -150 -80 -80 -150 0 -150 80 -150 150 -80 150 -1 150 80 80 150 4 150 -80 150 -150 80 -150 0{\p0}_

## Polar move ##
> _\mover(x1,y1,x2,y2,angle1,angle2,radius1,radius2`[`,t1,t2`]`)_

It works like \move but now it's possible to use rounded, oval or spiral trajectories.

**Example**
> _\mover(10,10,60,60,0,0,0,0)_ — it's equivalent to \move(10,10,60,60).

> _\mover(0,0,0,0,-90,0,150,150)_ — moves relatively to upper left screen corner along an arc of a circle (-90,0) with radius 150 points.

## Spline-move ##
> _\moves3(x1,x2,x2,y2,x3,y3`[`,t1,t2`]`)_

> _\moves4(x1,x2,x2,y2,x3,y3,x4,y4`[`,t1,t2`]`)_

It moves subtitle by spline curve trajectory. Functions with three or four base points are available, they produce cubic or bicubic Bezier curve trajectory.

## Shaking ##
> _\jitter(left,right,up,down,period`[,`seed`]`)_

It performs subtitle position shaking. First four parameters adjust maximum offset in every direction, fifth parameter sets shaking period in milliseconds, sixth parameter sets the initial seed for random number generator so the form shaking will not change upon calls. Animatable by \t.

## Moveable vector clip ##
> _\movevc(x1,y1)_

> _\movevc(x1,y1,x2,y2`[`,t1,t2`]`)_

It moves vector clips (\clip, \iclip) independently to subtitles (unaffected by \move or \pos). Parameters are same to \move. Pixel precision.