1. древовидная структура данных
2. для расщепления переполненных вершин используются различные алгоритмы:
	1. с квадратичной сложностью
	2. с линейной сложностью
	3. с экспоненциальной сложностью

The key idea of the data structure is to group nearby objects and represent them with their [minimum bounding rectangle](https://en.wikipedia.org/wiki/Minimum_bounding_rectangle "Minimum bounding rectangle") in the next higher level of the tree; the "R" in R-tree is for rectangle. Since all objects lie within this bounding rectangle, a query that does not intersect the bounding rectangle also cannot intersect any of the contained objects. At the leaf level, each rectangle describes a single object; at higher levels the aggregation includes an increasing number of objects. This can also be seen as an increasingly coarse approximation of the data set.


![Pasted image 20231204223819](../../../../_Attachments/Pasted%20image%2020231204223819.png)

# References:

1. [https://en.wikipedia.org/wiki/Spatial_database](https://en.wikipedia.org/wiki/Spatial_database)
2. [https://docs.mongodb.com/manual/geospatial-queries/](https://docs.mongodb.com/manual/geospatial-queries/)
3. [https://en.wikipedia.org/wiki/R-tree](https://en.wikipedia.org/wiki/R-tree)
4. [System Design: Design a Geo-Spatial index for real-time location search](https://kousiknath.medium.com/system-design-design-a-geo-spatial-index-for-real-time-location-search-10968fe62b9c)
5. [ИНДЕКСИРОВАНИЕ ПРОСТРАНСТВЕННЫХ ДАННЫХ](http://www.duskyrobin.com/tpu/2006-04-00037.pdf)