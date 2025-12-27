![](assets/04String/file-20251102233351037.png)
![](assets/04String/file-20251102233622741.png)
* **String的编码格式**：如果存入得是整数，编码类型就是int；如果是长度小于44字节的字符串就是embstr；大于44字节的字符串的编码格式就为raw

### 1. SDS——简单动态字符串
![](assets/04String/file-20251102234123846.png)


SDS的内部结构如下图所示   
![](assets/04String/file-20251102234356348.png)
![](assets/04String/file-20251102234428882.png)
![](assets/04String/file-20251102234226888.png)
![](assets/04String/file-20251102234703178.png)

![](assets/04String/file-20251102234839714.png)
![](assets/04String/file-20251102235005946.png)
![](assets/04String/file-20251102235256754.png)


### 2.三大物理编码格式
![](assets/04String/file-20251103002350015.png)
![](assets/04String/file-20251102235927894.png)
![](assets/04String/file-20251103000155111.png)


![](assets/04String/file-20251103001010567.png)
![](assets/04String/file-20251103001029020.png)
![](assets/04String/file-20251103001131618.png)


![](assets/04String/file-20251103001429183.png)
![](assets/04String/file-20251103001449564.png)


![](assets/04String/file-20251103001611755.png)



### 3.总结
![](assets/04String/file-20251103002103454.png)
![](assets/04String/file-20251103002402361.png)

![](assets/03RedisObject/file-20251103002502544.png)
![](assets/03RedisObject/file-20251103002510030.png)