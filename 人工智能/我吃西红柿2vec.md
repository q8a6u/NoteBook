## 我吃西红柿2vec

### 目的

网络小说作家总是凭借一套体系写小说，将一个套路用了一遍又一遍，如：xx三少、xx土豆等。本2vec主要探讨”我吃西红柿“几本小说的相似性，以及是否与xx土豆用着相同的套路。

### 语料准备

《莽荒纪》、《盘龙》、《星辰变》、《斗破苍穹》、《武动乾坤》

### 具体目的与预测

**xx土豆：主角开局必有大佬（实力np，但遭受变故）庇护：**

斗破苍穹--萧炎--药老；

武动乾坤--林动--天妖貂

**xx西红柿 预测：**

莽荒纪--纪宁--白水泽；//感觉大概率是，领路人作用

盘龙--林雷--xx爷爷//名字太难记了，大概率分词失败，txt全部替换一下

星辰变--秦羽--姜澜//

### 分词及乱码处理

txt另存为**ANSI**

<img src="https://gitee.com/cpicture/picture-1/raw/master/20210623163851.png" style="zoom:50%;" />

**jieba分词**

```python
import jieba 
article = open('hlm.txt', 'r', encoding='UTF-8') 
sent = article.read()
sent_list = jieba.cut(sent) 
result = open('result.txt', 'w', encoding='UTF-8')
result.write(' '.join(sent_list)) 
article.close()
result.close()
```

**ERROR处理**

**UnicodeDecodeError** ：'gb2312' codec can't decode bytes in position 2-3：illegal multibyte sequence

解决方法：处理的字符的确是gb2312，但是其中夹杂的部分特殊字符，是gb2312编码中所没有的。

如果有些特殊字符是GB18030中有的，但是是gb2312中没有的，则用gb2312去解码，也比较会出错。 所以，此种情况，可以尝试用和当前编码（gb2312）兼容的但所包含字符更多的编码（gb18030）去解码，或许就可以了。

GB2312，GBK，GB18030，是兼容的，包含的字符个数：GB2312 < GBK < GB18030

>  转自CSDN：https://blog.csdn.net/junkichan/article/details/51913845

### 结果

#### distance.exe

**萧炎**

<img src="https://gitee.com/cpicture/picture-1/raw/master/20210623174644.png"  />

**林动**

<img src="https://gitee.com/cpicture/picture-1/raw/master/20210623174817.png"  />

**林雷**

![](https://gitee.com/cpicture/picture-1/raw/master/20210623174930.png)

**秦羽**

![](https://gitee.com/cpicture/picture-1/raw/master/20210623175005.png)

**纪宁**

![](https://gitee.com/cpicture/picture-1/raw/master/20210623175026.png)

#### 分析

**xx土豆**：萧炎、林动前三者都是助手和女主（第三个）

**xx西红柿**：林雷、秦羽第一个好兄弟，第二个女主，第三个好兄弟2号

纪宁相关性：北冥（自己的道号）、苏尤姬（追随者）、余薇（女主）

修改txt文本后：余薇（女主）相关性增强

![](https://gitee.com/cpicture/picture-1/raw/master/20210623180633.png)

#### word-analogy.exe

**萧炎---药老---林动**（**土豆类内比较**）


![](https://gitee.com/cpicture/picture-1/raw/master/20210623180812.png)

**萧炎---药老---林雷**（**土豆与西红柿一较高低**）

![](https://gitee.com/cpicture/picture-1/raw/master/20210623181048.png)

> 尽管预料错误，但是仔细一想，确实贝贝在剧情对男主的作用更大

**萧炎---药老---秦羽**（**土豆与西红柿一较高低**）

![](https://gitee.com/cpicture/picture-1/raw/master/20210623181237.png)

> 与预测一样

**萧炎---药老---纪宁**（**土豆与西红柿一较高低**）

![](https://gitee.com/cpicture/picture-1/raw/master/20210623181416.png)

> 纪宁好像确实是个人成长，脱离boss伴生buff

---







### 总结

通过比较词向量之间的相关关系，发现xx西红柿《莽荒纪》--纪宁确实区别于之前小说，但是其他的比较对象都有明显特征：**主角开局会有仙人指路**





---

### 女主比较

**xx土豆**：萧炎--萧薰儿、林动--绫清竹

xx西红柿：纪宁--余薇、林雷--迪莉娅、秦羽--姜立

---

#### xx土豆

![](https://gitee.com/cpicture/picture-1/raw/master/20210623182041.png)

> **兄弟大于女主系列**

==那如果反过来呢==

![](https://gitee.com/cpicture/picture-1/raw/master/20210623182257.png)

> 更加离谱，嗯嗯，**女主可能被绿了？？**我可能打开小说的方式不对

#### xx西红柿

![](https://gitee.com/cpicture/picture-1/raw/master/20210623182429.png)

> 女主对应刚刚好

==重点==：**反过来也一样**。nice！

![](https://gitee.com/cpicture/picture-1/raw/master/20210623182642.png)

> 女主对应刚刚好

==重点==：**反过来也一样**。nice！

#### 类间比较

![](https://gitee.com/cpicture/picture-1/raw/master/20210623183019.png)

> 好像确实是男女主对应关系，没问题

==重点==：**反过来也一样**。nice！

![](https://gitee.com/cpicture/picture-1/raw/master/20210623183326.png)

> **萧炎就离谱**
