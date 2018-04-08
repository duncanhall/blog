+++
archive = true
date = "2010-03-13T16:10:00.000Z"
title = "Google Maps Flash API & Selectable Copyright Text"

+++

When using the Google Maps Flash API you are required to display the relevant copyright notice for the tiles currently being shown. This is handled internally by the API and is generally not a problem. However, the current version of the SWC provided (1.18) chooses to handle the copyright text as selectable, meaning that the text cursor is displayed when the mouse is anywhere near the bottom 30px of the map. See below for an example

_ MISSING EXAMPLE SWF _

While I’m not sure if this is necessarily ‘as designed’, it doesn’t seem unreasonable to want to make the text unselectable. I’ve registered it as a [bug](http://code.google.com/p/gmaps-api-issues/issues/detail?id=2234), but for now I’ve also created a workaround in the hope this will be changed in the next release.

After investigating the display list of a `Map` object, it seems one of the sub-children is an object of type `com.google.maps.controls.CopyrightView`. This class is not publicly exposed in the API so I’m unsure of the implications of altering it in this way. The terms of use state that:

> *[you must not] delete, obscure, or in any manner alter any warning, notice (including but not limited to any copyright or other proprietary rights notice), or link that appears in the Products or the Content*

We’re certainly not deleting or obscuring it, and we’re not actually altering the notice itself, just the interactive properties it has when displayed. In any case, the solution is presented below:

**Update 15/02/2010:** It seems the `CopyrightView` object is not always contained at position `0` in the parents display list. To rectify this, I have ammended the solution below to explicitly find the `CopyrightView` via its qualified class name.

    //_map is an instance of a com.google.maps.interfaces.IMap object
    _map.addEventListener(MapEvent.MAP_READY, map_readyHandler);

    /**
     * Wait for the map to initialize and add an ENTER_FRAME event listener 
     */
    function map_readyHandler (event:MapEvent) : void
    {
        addEventListener(Event.ENTER_FRAME, setCopyrightToUnselectable);
    }

    /**
     * Locate the copyright textfield and change its selectable property
     */
    function setCopyrightToUnselectable (event:Event) : void
    {
        removeEventListener(Event.ENTER_FRAME, setCopyrightToUnselectable);

        var container:DisplayObjectContainer = _map.getChildAt(5) as DisplayObjectContainer;
        var numChildren:int = container.numChildren;
        var child:*;

        for (var i:int = 0; i < numChildren; i++)
        {
            child = container.getChildAt(i);
            if (getQualifiedClassName(child) == "com.google.maps.controls::CopyrightView")
            {
                var textfield:TextField = child.getChildAt(0) as TextField;
                textfield.selectable = false;	
                break;
            }
        }
    }

</td></tr></table></div>Even when the `MapEvent.MAP_READY` event is fired, it seems the `CopyrightView` object has not yet been added to its parent (or has not been initialized). To get around this, a 1 frame delay is added, at which point the problematic `TextField` can be targeted and we can set the `selectable` property to false. This solution gives us exactly what we’re after, without altering the text or effecting the ‘terms of use’ link.

I’m hoping this isn’t an intended feature and that it will change in the next SWC, and in the meantime that the above solution isn’t violating any terms of use.

