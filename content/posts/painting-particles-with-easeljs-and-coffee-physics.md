+++
archive = true
date = "2012-03-15T19:15:37.000Z"
draft = true
title = "Painting particles with EaselJS and Coffee Physics"

+++

As a platform for trying out [EaselJS](http://easeljs.com) I wanted to create a simple particle demo using the [Coffee Physics](https://github.com/soulwire/Coffee-Physics) engine. Through a bit of experimentation and (mainly) curiosity, I ended up with something half particle generator, half generative art tool.

[![](http://blog.duncanhall.net/wp-content/uploads/2012/03/sample1.png "Sample output image")](http://blog.duncanhall.net/wp-content/uploads/2012/03/sample1.png)

[View the demo](http://duncanhall.net/canvas) | [View sample images](http://duncanhall.net/canvas/samples) | [View the source ](https://github.com/duncanhall/EaselJS-Coffee-Physics-Demo/)

Having not used EaselJS before it was reassuringly easy to get to grips with, and there’s good documentation available.  Anyone who’s done any work with the Flash display list should have no problems diving straight in. It takes very little code to setup a Stage and a rendering loop, and keeps your application nicely browser and platform agnostic. The demo runs (from best to worst) in Chrome, IE9, Firefox and  Safari for iOS (although very slowly), without the need to write specific code for any of them, which is a great reason for using it.

Although the Coffee Physics demo source contains various renderers, the engine itself is, as you’d expect, completely independent of any rendering process. This makes it very easy to integrate with Easel, or any other rendering method you choose. I didn’t end up using any of the the supplied behaviours in my demo (collision, attraction etc.), but I did play around with these to begin with. The behaviour architecture makes it very easy to leave out any heavy-lifting you don’t need, and easily create and add your own features.

The idea to ‘paint’ the particles came simply from browsing the [EaselJS API docs](http://easeljs.com/docs) and is achieved easily by setting the autoClear property of the Stage to false. This prevents automatic clearing of the canvas before each render update and would be useful for more serious generative art pieces.

Saving the image seemed like an obvious progression, and it was nice to be able to output and save image data without needing a server transaction or, dare I say it, Flash. It is however, not the most polished solution: dump a base64 encoded image string into a browser window and hope it knows what to do with it. Internet Explorer doesn’t know what to do with it.

Both EaselJS and Coffee Physics are in alpha/beta stage right now, so it will be interesting to see how they progress. This was also my first time using [CoffeeScript](http://coffeescript.org/), which seems to be fairly contentious at the moment, but I found it intuitive and enjoyable to work with. The compiler is reliable, and being able to ‘watch’ any files, recompiling them whenever they are saved, makes the write/run cycle no different from writing straight JavaScript. You could even argue the compile step adds extra sanity checking to your code before run time. Either way, I’ll definitely be using it again


