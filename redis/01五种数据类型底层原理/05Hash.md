![](assets/05Hash/file-20251103002906057.png)
## 1.redis6的数据结构——hashtable+ziplist压缩列表

###### 总体机制
![](assets/05Hash/file-20251103003447123.png)
![](assets/05Hash/file-20251103003534944.png)
![](assets/05Hash/file-20251103003543907.png)
![](assets/05Hash/file-20251103003730010.png)
######  redis中Hashtable结构
![](assets/05Hash/file-20251103004408958.png)
![](assets/05Hash/file-20251103004428648.png)

###### ziplist结构
![](assets/05Hash/file-20251103005217404.png)
![](assets/05Hash/file-20251103005443310.png)
![](assets/05Hash/file-20251103005648383.png)
![](assets/05Hash/file-20251103010136530.png)
![](assets/05Hash/file-20251103010640467.png)
![](assets/05Hash/file-20251103010831880.png)
![](assets/05Hash/file-20251103010910200.png)
![](assets/05Hash/file-20251103010601966.png)
![](assets/05Hash/file-20251103010610149.png)

这样设计的好处：  
![](assets/05Hash/file-20251103011202773.png)
![](assets/05Hash/file-20251103011451907.png)
![](assets/05Hash/file-20251103011458925.png)
![](assets/05Hash/file-20251103011845907.png)

###### ziplist的总结
![](assets/05Hash/file-20251103012012417.png)


## 2.redis7的数据结构——hashtable+listpack紧凑列表
![](assets/05Hash/file-20251103201450671.png)
![](assets/05Hash/file-20251103201713649.png)
![](assets/05Hash/file-20251103201728121.png)
* 默认值与redis6相同，在符合相同情况下使用列表；不符合人任一条件的底层结构就为hashtable

![](assets/05Hash/file-20251103201605203.png)

###### 有了ziplist为什么要用listpack替代ziplist
![](assets/05Hash/file-20251103202251014.png)
![](assets/05Hash/file-20251103202638721.png)
![](assets/05Hash/file-20251103202745245.png)
![](assets/05Hash/file-20251103202804410.png)
![](assets/05Hash/file-20251103202836956.png)


###### 紧凑列表listpack的结构
![](assets/05Hash/file-20251103202028560.png)
![](assets/05Hash/file-20251103203106472.png)
![](assets/05Hash/file-20251103203125068.png)


###### 二者结构对比
![](assets/05Hash/file-20251103203331357.png)
![](assets/05Hash/file-20251103203402644.png)