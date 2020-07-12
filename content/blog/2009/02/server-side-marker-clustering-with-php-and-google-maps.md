---
title: "Server-side Marker Clustering with PHP and Google Maps"
date: "2009-02-26"
tags: [databases,open-source,programming,web-services,webdev]
---

![with_cluster2](http://kewnode.files.wordpress.com/2009/02/with_cluster2.gif "with_cluster2")

As maps get busier, marker clustering is likely to be needed. Marker clustering is a technique by which several points of interest can be represented by a single icon when they're close to one another.

[Mika Tuupola](http://www.appelsiini.net/) wrote a [PHP library](http://github.com/tuupola/php_google_maps/tree/master) that divides the map into a given number of cells and represents all the markers present in the same cell by a single icon. This icon shows the number of markers it symbolizes.

He also wrote an [excellent post](http://www.appelsiini.net/2008/11/introduction-to-marker-clustering-with-google-maps) explaining how marker clustering works.

### Related Posts

- [Geo Proximity Search with PHP, Python and SQL](../2009/02/08/geo-proximity-search-with-php-python-and-sql/ "Permanent Link to Geo Proximity Search with PHP, Python and SQL")
