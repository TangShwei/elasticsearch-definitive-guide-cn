## 通过地理坐标点过滤

有四种地理坐标点相关的过滤方式可以用来选中或者排除文档：

- `geo_bounding_box`::

    找出落在指定矩形框中的坐标点

- `geo_distance`::

    找出与指定位置在给定距离内的点

- `geo_distance_range`::

    找出与指定点距离在给定最小距离和最大距离之间的点

- `geo_polygon`::

    找出落在多边形中的点。_这个过滤器使用代价很大_。当你觉得自己需要使用它，最好先看看 [geo-shapes](/geo-shapes) 

所有这些过滤器的工作方式都相似：
把 _索引中所有文档_（而不仅仅是查询中匹配到的部分文档，见 [fielddata-intro](/fielddata-intro)）的经纬度信息都载入内存，然后每个过滤器执行一个轻量级的计算去判断当前点是否落在指定区域。

> 提示

> 地理坐标过滤器使用代价昂贵 —— 所以最好在文档集合尽可能少的场景使用。
> 你可以先使用那些简单快捷的过滤器，比如 `term` 或者 `range`，来过滤掉尽可能多的文档，最后才交给地理坐标过滤器处理。

> 布尔型过滤器（`bool filter`）会自动帮你做这件事。
> 它会优先让那些基于“bitset”的简单过滤器(见 [filter-caching](/filter-caching))来过滤掉尽可能多的文档，然后依次才是地理坐标过滤器或者脚本类的过滤器。

