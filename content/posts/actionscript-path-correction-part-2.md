+++
archive = true
date = "2010-03-06T13:57:10.000Z"
title = "Actionscript Path Correction Part 2"

+++
After my initial success in [removing dead ends and backtracking from a directed path](http://blog.duncanhall.net/posts/actionscript-path-correction---no-going-back-/), I realised I was only half way to solving the problem. In the [original post](http://blog.duncanhall.net/posts/actionscript-path-correction---no-going-back-/), points A, B and C were all single point digressions away from the main path.

![](/uploads/2018/04/08/PathCorrectA.png)

With the addition of the new points at D, the previous solution would only have removed the segments marked in green, still leaving a large part of D that we don’t want.

To remedy this, I have made 2 additions to the original solution. First, I created a method that allows me to recursively check the corrected path, with the corrected points being fed back into the correction algorithm a specified number of times. Initially I thought this would be all I needed, but I was still getting problem points left along the path. The key was to check the distance between the 2 points left remaining after an adjoining point had been removed. If it is below a certain distance, the next point along the path is also culled from the new route. The example below shows correction of a path with some complex faults:

<div class="efe-flash" id="efe-swf-7">  
Recursive path correction example swf  
</div>I’m quite pleased with this solution, so far its proved to eliminate every one of the features I’ve sought to remove. I can’t go into my own uses for it yet, but it’s applications could vary from optimising a route through a maze to auto correcting a shape drawn from user input.

I have created the `DirectedPath` class to encapsulate the 2 methods I created.

_DirectedPath.correctPath()_  
Removes dead ends and vertices that double back on themselves from a path of consecutive points. The `threshold` value specifies the minimum allowable angle between any 3 consecutive points.

    var correctedPath:Array = DirectedPath.correctPath(arrayOfPoint, 12);

_DirectedPath.correctPathRecursive()_  
If the path is found to find faults, the path is recursively corrected until no more faults are found, or the `maxLevels` parameter is reached.

    var correctedPath:Array = DirectedPath.correctPathRecursive(arrayOfPoint, 12, 10, 4);

View source for [DirectedPath.as](http://code.google.com/p/duncanhall-lib/source/browse/trunk/net/duncanhall/geom/DirectedPath.as)  
Download [DirectedPath.as](http://code.google.com/p/duncanhall-lib/source/checkout)  
View [DirectedPath Documentatation](http://duncanhall.net/docs/net/duncanhall/geom/DirectedPath.html)