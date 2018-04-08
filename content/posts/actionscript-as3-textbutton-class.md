+++
date = "2010-02-28T21:22:42Z"
draft = true
title = "Actionscript AS3 TextButton Class"
type = "default"

+++
The [TextButton](http://code.google.com/p/duncanhall-lib/source/browse/trunk/net/duncanhall/components/TextButton.as) Class provides a method for creating button components based on simple text labels. It’s something that’s often overlooked in many component sets, but since writing it I’ve found it to be invaluable in almost all of my projects.

In it’s simplest form, all you need to define for a new `TextButton` instance is the text you want it to display:

    var button:TextButton = new TextButton("Home");
    addChild(button);

This will create a button with the word “Home” rendered in the system default font. I find this can be useful for very early prototyping to get some quick navigation items on the screen. However, the various states of the button will all use the same default font to style the label, and so it wont visually appear to respond to mouse events.

Defning the styles you want applied to each state is as simple as providing a `TextFormat` object for each one.

    var upFmt:TextFormat = new TextFormat("Tahoma", 12, 0x000000, false);
    var overFmt:TextFormat = new TextFormat("Tahoma", 12, 0xFF0000, true);
    var downFmt:TextFormat = new TextFormat("Tahoma", 12, 0x00CC99, true);
    var button:TextButton = new TextButton("Click Me!", upFmt, overFmt, downFmt);
    addChild(button);

_Example 1:_

_ MISSING SWF EXAMPLE _

The `TextButton` class extends `SimpleButton` and can be treated like one for all other intents and purposes. The example below shows an event listener being added for the `MouseEvent.CLICK` event, and also shows that the styles and hit area are automatically updated whenever the value of the text changes.

    var upFmt:TextFormat = new TextFormat("Tahoma", 12, 0x000000, false);
    var overFmt:TextFormat = new TextFormat("Tahoma", 12, 0xFF0000, true);
    var downFmt:TextFormat = new TextFormat("Tahoma", 12, 0x00CC99, true);
     
    var button:TextButton = 
      new TextButton("How many times can you click this?", upFmt, overFmt, downFmt);
      
    button.addEventListener(MouseEvent.CLICK, button_clickHandler);
    addChild(button);
     
    var c:int;
    function button_clickHandler (event:MouseEvent) : void {
      c++;
      button.text = "That's " + c + " clicks";
    }

_Example 2:_

_ MISSING SWF EXAMPLE _

The last argument accepted by the constructor is `embedFonts` which you should set to `true` if any of your `TextFormat` objects use an embedded font. Note that if `embedFonts` is true, all your `TextFormat` objects must have the relevant fonts embedded.

I have kept all non-public properties and methods as `protected` so you can subclass `TextButton` and add any functionality you need.

View source for [TextButtonas.as](http://code.google.com/p/duncanhall-lib/source/browse/trunk/net/duncanhall/components/TextButton.as)  
Download [TextButton](http://code.google.com/p/duncanhall-lib/source/checkout)  
View [TextButton Documentatation](http://duncanhall.net/docs/net/duncanhall/components/TextButton.html)