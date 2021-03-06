# 课程第四周

> 字符串相关：字符串模式匹配算法

# 前言

本周结算字符串模式匹配算法相关，主要介绍BF算法、字典树、KMP、Wildcard等算法，学习字符串匹配的各种方法

# 正文

## Brute-Force算法

Brute-Force算法，又称为暴力结算法，思路很简单：从目标字符串出事位置开始，依次与Pattern的各个位置的字符比较，如果相同，继续比较下一个未知的字符直至完全匹配；如果不同，则跳过目标字符串，继续比较下一个位置。重复此过程，知道找到匹配字符串。

这里可以注意到Brute Force 算法是每次移动一个单位，一个一个单位移动显然太慢，设目标串String的长度为m，Pattern的长度为n，不难得出BF算法的时间复杂度最坏为O(mn)，效率很低。

## 字典树

字典树，即为Trie树，又成单词查找树或键树，是一种树形结构，是哈希树的变种。主要用于统计和排序大量打字符串（但不仅限于字符串），所以经常被搜索引擎系统用于文本词频统计，主要优点是可以最大限度的减少所谓的字符串比较。

Trie的核心思想是空间换时间，利用字符串的公共齐纳追来较低查询时间的开销来达到查找提高效率的目的

### 字典树的基本性质

字典树的每个节点都是一个字符，并按照一定顺序进行排序，主体结构如下：

![](https://upload-images.jianshu.io/upload_images/426671-b6f3caf9253f3a4d.png?imageMogr2/auto-orient/strip|imageView2/2/format/webp)

从字典树的结构不难看出字典树的基本特性

-   根节点不包含字符，出根节点之外的每个节点都包含一个字符
-   从根节点到某一个节点，路径上经过的字符连接起来，为该节点对应的字符串
-   每个节点的所有子节点包含的字符都是不相同的

### 字典树的优点

字典树的优点是可以最大限度的减少无畏的字符串比较，和哈希表比较：

1  最坏情况下复杂度hash表好
2   没有重复
3   自带排序功能，中序遍历Trie可以得到排序后的字符串

### 字典树的缺点

1 由于Trie的核心思想是以空间换时间，所以Tire每一个字符都可能包含最长自渡船大小数目的空字符
2、如果数据存储在完毕存储器等较慢位置，Trie会较hash速度慢
3、长的浮点数会让链变得很长，这时可以用 Bitwise Trie改进

### 应用场景

-   字符串检索等
-   文本预测、自动完成、拼写检查等
-   词频统计等
-   字符串排序等
-   字符串最长公共前缀等
-   字符串搜索的前缀匹配等
-   作为其他数据结构和算法的辅助结构等

## KMP

KMP算法是一种改进的字符串匹配算法，是 D.E.Knuth、J,H,Morris 和 V.R.Pratt 三位神人共同提出的，称之为 Knuth-Morria-Pratt 算法，简称 KMP 算法。该算法的关键是利用匹配失败后的信息，精良减少模式串与主串的匹配次数以达到款速匹配的目的，在BF的技术上通过next函数找出下一个目标与Pattern比较的位置，利用Pattern字符重复的特性来排除不必要的比较，从而可以每次移动n位来排除冗余。

KMP算法相对于 Brute-Force（暴力）算法有比较大的改进，主要是消除了主串指针的回溯，从而使算法效率有了某种程度的提高。

KMP算法的核心，是一个被称为部分匹配表（Partial  Match Table）的数组，下面我们重点理解下KMP算法中的PMT中的值代表什么意思。

对于模式字符串“abababca”，他的PMT如下所示

![](https://pic1.zhimg.com/80/v2-e905ece7e7d8be90afc62fe9595a9b0f_720w.jpg?source=1940ef5c)

就模式字符串“abababca”来说，PMT的值的个数，就是待匹配的模式字符串的长度，这里就会有8个值

字符串的前缀和后缀：

如果字符串A和B，存在A=BS，其中S表示任意的非空字符串，那就称B为A的前缀。例如：“Harry”的前缀包括{'H', 'Ha', 'Har', 'Harr'}，我们把所有前缀组成的集合，称为字符串的前缀集合。同样可以定义后缀A=SB，其中S是任意的非空字符串，则称B为A的后缀，如“Potter”的后缀包括{'otter', ' tter', 'ter', 'er', 'r'}，然后把虽有后对组成的集合，称为字符串的后缀集合。需要注意的是，字符串本身并不是自己的后缀。

有了上面的定义，就可以说明PMT中值得意义了。<b>PMT中的值就是字符串的"前缀"和"后缀"的最长的共有元素的长度。</b>，

例如，对于”aba”，它的前缀集合为{”a”, ”ab”}，后缀 集合为{”ba”, ”a”}。两个集合的交集为{”a”}，那么长度最长的元素就是字符串”a”了，长 度为1，所以对于”aba”而言，它在PMT表中对应的值就是1。

再比如，对于字符串”ababa”，它的前缀集合为{”a”, ”ab”, ”aba”, ”abab”}，它的后缀集合为{”baba”, ”aba”, ”ba”, ”a”}， 两个集合的交集为{”a”, ”aba”}，其中最长的元素为”aba”，长度为3。

通过这种方式就可以计算得到PMT表，而KMP算法的想法是，设法利用PMT的已知信息，不要把“搜索位置”移回到已经比较过的位置，继续向后移动，从而提高查找效率，具体案例请参考阮一峰老师案例讲述： [字符串匹配的KMP算法](http://www.ruanyifeng.com/blog/2013/05/Knuth%E2%80%93Morris%E2%80%93Pratt_algorithm.html)

## Wildcard

Wildcard表示字符串的统配实现，其中

'?' 表示匹配单一字符
'*' 表示可匹配任意多字符（包含0个）

起匹配原理如下：
要匹配的字符串设为 s，模式匹配用的字符串为p，那么如果是普通字符，连个字符串索引向前推进一位即可，如果 p 中的字符是？也是向前推进一位。所以现在的关键就在于如何处理 '*'，因为 * 可以匹配0、1、2...个字符，所以遇到 * 时，s应该静可能的向前推进，注意到 p 中的 * 后面可能跟着其他普通字符，故 s 向前推进多少位直接与 p 中 * 后面的字符相关。同时此时两个字符串中的索引即成为回溯点，如果后面的字符串匹配不成功，则 s 中的索引向前推进，向前推进的字符串即表示和 p 中 * 匹配的字符个数

## 总结

字符串的匹配算法表较多，处理上面描述的BF算法、KMP算法、Wildcard算法，还有其他如Boyer-Moore算法、Sunday算法、状态机、正则等，通过这些算法的原理，可以更好地理解字符串模式匹配的相关流程

# 参考文章

-	[字符串匹配算法总结 (分析及Java实现)](https://blog.csdn.net/chndata/article/details/43792363)
-   [如何更好地理解和掌握 KMP 算法?](https://www.zhihu.com/question/21923021)
-   [KMP算法—终于全部弄懂了](https://blog.csdn.net/dark_cy/article/details/88698736)
-   [3]  [字符串匹配的KMP算法](http://www.ruanyifeng.com/blog/2013/05/Knuth%E2%80%93Morris%E2%80%93Pratt_algorithm.html)
-   [LeetCode字典树(Trie)总结](http://www.jianshu.com/p/bbfe4874f66f)
-   [字典树(Trie树)的实现及应用]( http://www.cnblogs.com/binyue/p/3771040.html#undefined)
-   [6天通吃树结构—— 第五天 Trie树]( http://www.cnblogs.com/huangxincheng/archive/2012/11/25/2788268.html)