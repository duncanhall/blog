+++
date = "2010-02-27T14:21:33.000Z"
draft = true
title = "AS3 Encoded Polyline Algorithm For Google Maps"

+++
The Google Maps API uses Polyline objects to represent a series of straight lines between a set of latitude and longitude points. Long and complicated lines can become quite cumbersome to describe (especially when needing to send Polyline data to a remote service for example). For this reason, Google provides a method for encoding a Polyline as a series of ASCII characters, with each vertex defined by its distance from the last, rather than the absolute location of each one.

The [Encoded Polyline Algorithm Format](http://code.google.com/apis/maps/documentation/polylinealgorithm.html) page documents the steps necessary to encode a set of lat/long points in the required format. For those of us working with the [Google Maps API for Flash](http://code.google.com/apis/maps/documentation/flash/), I have written an Actionscript 3 implementation of the encoder, in the form of the [PolylineEncoder](http://code.google.com/p/duncanhall-lib/source/browse/trunk/net/duncanhall/gmaps/PolylineEncoder.as) Class.

_PolylineEncoder.fromPoints()_  
Takes a `Vector` of `LatLng` objects and returns the encoded string representation:

    var points:Vector.<LatLng> = new Vector.<LatLng>();
    
    points.push(
      new LatLng(38.5, -120.2), 
      new LatLng(40.7, -120.95), 
      new LatLng(43.252, -126.453));
      
    var encoded:String = PolylineEncoder.fromPoints(points);
    trace(encoded); //Outputs _p~iF~ps|U_ulLnnqC_mqNvxq`@

_PolylineEncoder.fromPolyline()_  
Takes an existing `IPolyline` object and returns the encoded string representation:

    var poly:Polyline = new Polyline([
      new LatLng(38.5, -120.2), 
      new LatLng(40.7, -120.95), 
      new LatLng(43.252, -126.453)]);
    var encoded:String = PolylineEncoder.fromPolyline(poly);
    trace(encoded); //Outputs _p~iF~ps|U_ulLnnqC_mqNvxq`@

_PolylineEncoder.encodeLevels()_  
Takes an `Array` of `uint` level values and returns the encoded string representation:

    var encodedLevels:String = PolylineEncoder.encodeLevels([174, 100, 100, 174]);
    trace(levels); //Outputs mDcBcBmD

The use of the ‘level’ value is described by Google:

> _An encoded polyline also stores information specifying the precision when drawing the polyline. This information allows the map to ignore drawing segments at zoom levels where that precision is not necessary. Each point in an encoded polyline stores this information in a levels string which is also encoded alongside the encoded points._
>
> _Note: this encoded “level” does not correspond directly to a zoom level, though they are related. More accurately, the numLevels parameter divides up the existing zoom levels (currently 18) into groups of zoom levels._

In other words, if you are encoding a Polyline purely for storing locational data, it’s not necessary to encode a level value for each point on the line. The lat/lng of the original point is unaffected by its level value.

    var encodedPoints:String = PolylineEncoder.encodeFromPolyline(existingPolyline);
    var encodedLevels:String = PoylineEncoder.encodeLevels([100]);
    var encodedPolyline:EncodedPolylineData = new EncodedPolylineData(encodedPoints, 4, encodedLevels, 16)
    var decodedPolyline:Polyline = Polyline.fromEncoded(encodedPolyline);
    trace(existingPolyline.getLength() == decodedPolyline.getLength()); //true!

View source for [PolylineEncoder.as](http://code.google.com/p/duncanhall-lib/source/browse/trunk/net/duncanhall/gmaps/PolylineEncoder.as)  
Download [PolylineEncoder.as](http://code.google.com/p/duncanhall-lib/source/checkout)  
View [PolylineEncoder Documentatation](http://duncanhall.net/docs/net/duncanhall/gmaps/PolylineEncoder.html)