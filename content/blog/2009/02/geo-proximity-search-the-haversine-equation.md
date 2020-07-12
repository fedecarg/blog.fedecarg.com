---
title: "Geo Proximity Search: The Haversine Equation"
date: "2009-02-08"
tags: [databases,open-source,programming,python,webdev]
---

I'm working on a project that requires Geo proximity search. Basically, what I'm doing is plotting a radius around a point on a map, which is defined by the distance between two points on the map given their latitudes and longitudes. To achieve this I'm using the [Haversine](http://en.wikipedia.org/wiki/Haversine_formula) formula (spherical trigonometry). This equation is important in navigation, it gives great-circle distances between two points on a sphere from their longitudes and latitudes. You can see it in action here: [Radius From UK Postcode](http://www.freemaptools.com/radius-from-uk-postcode.htm).

This has already been covered in some blogs, however, I found most of the information to be inaccurate and, in some cases, incorrect. The Haversine equation is very straight forward, so there's no need to complicate things.

I've implemented the solution in SQL, Python and PHP. Use the one that suits you best.

**SQL implementation**

```
set @latitude=53.754842;
set @longitude=-2.708077;
set @radius=20;

set @lng_min = @longitude - @radius/abs(cos(radians(@latitude))*69);
set @lng_max = @longitude + @radius/abs(cos(radians(@latitude))*69);
set @lat_min = @latitude - (@radius/69);
set @lat_max = @latitude + (@radius/69);

SELECT * FROM postcode
WHERE (longitude BETWEEN @lng_min AND @lng_max)
AND (latitude BETWEEN @lat_min and @lat_max);
```

**Python implementation**

```
from __future__ import division
import math

longitude = float(-2.708077)
latitude = float(53.754842)
radius = 20

lng_min = longitude - radius / abs(math.cos(math.radians(latitude)) * 69)
lng_max = longitude + radius / abs(math.cos(math.radians(latitude)) * 69)
lat_min = latitude - (radius / 69)
lat_max = latitude + (radius / 69)

print 'lng (min/max): %f %f' % (lng_min, lng_max)
print 'lat (min/max): %f %f' % (lat_min, lat_max)
```

**PHP implementation**

```
$longitude = (float) -2.708077;
$latitude = (float) 53.754842;
$radius = 20; // in miles

$lng_min = $longitude - $radius / abs(cos(deg2rad($latitude)) * 69);
$lng_max = $longitude + $radius / abs(cos(deg2rad($latitude)) * 69);
$lat_min = $latitude - ($radius / 69);
$lat_max = $latitude + ($radius / 69);

echo 'lng (min/max): ' . $lng_min . '/' . $lng_max . PHP_EOL;
echo 'lat (min/max): ' . $lat_min . '/' . $lat_max;
```

It outputs the same result:

```
lng (min/max): -3.1983251898421/-2.2178288101579
lat (min/max): 53.464986927536/54.044697072464
```

That's it. Happy Geolocating!
