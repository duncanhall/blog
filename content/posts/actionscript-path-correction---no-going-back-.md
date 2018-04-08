+++
archive = true
date = "2010-03-03T20:20:22.000Z"
draft = true
title = "Actionscript Path Correction - No Going Back!"

+++

**Update**: See the [improved solution](http://blog.duncanhall.net/2010/03/actionscript-path-correction-part-2/)

For a project I’m currently working on, I needed a way to remove extremities from a path of consecutive points. The extremities represent points or segments of the path that we want to remove from the route (usually “going back on yourself”), while maintaining the intended course as much possible.

![Path Correction](/content/images/2015/02/PathCorrect1.png)

Given the path in black above, the points A, B and C represent the types of feature we want to remove. Continuous curves would veer the path too far from the original, and in most cases would still leave an unacceptable ‘quirk’ in the path. My searches took me from [Line Generalization](http://www.lostinactionscript.com/blog/index.php/2007/07/11/douglas-peuker-line-generalization/) to [Line Simplification](http://www.geom.unimelb.edu.au/gisweb/LGmodule/LGSimplification.htm) and [Line Smoothing](http://www.geom.unimelb.edu.au/gisweb/LGmodule/LGSmoothing.htm), with smoothing getting closest to what I was after.

All of them however, alter too many of the points along the path, and generally leave a fairly recognisable footprint of the feature we are trying to remove. To get the result we’re after, we need to explicitly identify the points that are causing a problem. With this in mind, the solution actually turns out to be somewhat simpler than those required above.

<div class="efe-flash" id="efe-swf-5"></div>

Each point along the path is tested against the point immediately before and after it. The angle given by the point in question is put through a high pass filter, with those that fail not making it into the new path. The `threshold` value specifies the lowest allowable angle derived from any 3 adjacent points. The first and last points of the original line always remain unaltered. *(In the example above, the threshold was set to 12)*

<del datetime="2010-03-07T12:10:49+00:00">I won’t brother wrapping this up in a class, as it’s only one method, so here it is, to do with what you please. </del>  

I’ve since [updated](http://blog.duncanhall.net/2010/03/actionscript-path-correction-part-2/) this original version and it’s now available in the [DirectedPath](http://code.google.com/p/duncanhall-lib/source/browse/trunk/net/duncanhall/geom/DirectedPath.as) class. I’ll leave the code below as a reference, as the update does not alter the basic premise of the solution.

```lang-javascript
function correctPath (points:Array, threshold:Number) : Array
{
	var numPoints:int = points.length;
	var corrected:Array = [];
	corrected.push(points[0]);
 
	var p0:Point;
	var p1:Point;
	var p2:Point;
	var da:Number;
	var db:Number
	var dc:Number;
	var c:Number;
	var n:int = numPoints - 1;
 
	for (var i:int = 1; i < n; i++)
	{
		p0 = points[i - 1];
		p1 = points[i];
		p2 = points[i + 1];
		da = Point.distance(p0, p1);
		db = Point.distance(p1, p2);
		dc = Point.distance(p0, p2);
		c = Math.acos((da * da + db * db - dc * dc) / (2 * da * db));
 
		if ((c * 180 / Math.PI) >= threshold) corrected.push(p1);
	}
 
	corrected.push(points[numPoints - 1]);
	return corrected;
}
```
