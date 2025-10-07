![](assets/05CopyOnWriuteArrayList原理/file-20251007143851289.png)
* CopyOnWriteArraySet 是它的马甲的意思就是：底层就是一个CopyOnWriteArrayList。类似于HashSet和HashMap的关系。
* **并发读时读的数组是旧数组而不是在新数组上读，所以可能会有弱一致性的发生**。

![](assets/05CopyOnWriuteArrayList原理/file-20251007144806353.png)
![](assets/05CopyOnWriuteArrayList原理/file-20251007145148996.png)
![](assets/05CopyOnWriuteArrayList原理/file-20251007145312963.png)