# 课程第五周

> Proxy、Range和CSSOM操作

# 前言

本周学习Proxy与双向绑定，模仿reactive实现原理；使用Range和CSSOM API实现DOM精确操作，主要实现拖拽的功能实现

# 正文


## Proxy(代理)与Reflect(反射)

使用Proxy实现双向绑定，模仿reactive实现原理，需要了解到下面两个属性

### Proxy(代理)

Proxy是ES6新增的代理对象，通过代理对象可以兰家JavaScript引擎内部目标的底层对象的操作，这些底层操作被拦截之后会触发特定操作的陷阱函数，通常是监听对象的set和get方法来设置和获取对象属性

### Reflect(反射)

Reflect也是ES6新增的用于对象操作的集合封装，用来取代原来Object上的一些方法，特提供了一个Reflect对象API，该对象中的让发默认特性与底层的操作相应；而每一个代理陷阱会对应一个命名和参数都相同的Reflect方法（其实就是每个代理陷阱都会对应一个Reflect API接口来实现JavaScript底层操作）

而利用Proxy代理对象的实现中，也是推荐结合Reflect操作对象属性，因为 Reflect 中的API的计算具有原子性，实现更安全可靠

案例如下：

```js
let obj = {
    a: '张三',
    b: '李四'
}

let proxy = new Proxy(obj, {
    set (terget, prop, value, recriver) {
        // 原子性操作
        return Reflect.set(terget, prop, value, recriver)
    },
    get (target, prop) {
        return Reflect.get(target, prop)
    }
})
```

关于通过Proxy和Reflect代理操作对象属性，可以查看学习 Immerjs 库，利用Proxy和Object.defineProperty()，基于不可变数据的理念实现，实际项目中用的非常多的一个不可变数据的库


## Range 与 CSSOM

### Range

查看 Range MDN 描述可以，Range表示一种fragment（HTML片段），它包含了节点或文本节点的一部分。可以通过document.createRange（）或 selection对象的getRangeAt（）方法获得，具体属性可以查看MDN详细描述，常用的使用场景如下：

-   文本选择

如文本的光标前后的字符、插入或替换文本等操作，最常见的就是富文本编辑器的操作

-   Dom Range的区域选择

创建Range，包含Dom、复制Dom等，例如：

```js
function clone() {
        const dom = document.getElementById("dom1")

        // 创建Range
        const range = document.createRange()

        // 包含元素
        range.selectNodeContents(dom)

        // 克隆内容
        const newDom = range.cloneContents()
        dom.appendChild(newDom)
    }

```

-   CSSOM

web中除了DOM之外还有一个对象模型：CSS对象模型（即CSSOM）。CSSOM其实是一组允许JavaScript操作CSS的API，它和DOM非常像，但是允许用户动态读取和修改CSS样式，是作用于CSS而不是HTML。

CSSOM的操作都是和CSS相关，可能实际项目中有用到但并没有意识到是CSSOM的操作，如常见的修改Dom的样式等

```js
const cover = document.getElementById('cover');
cover.style.opacity; // 假设是 0.5

cover.style.opacity += 0.3;
```

## 总结

Proxy和Reflect是ES6新增的属性，针对新的特性弥补了原来Object.defineProperty()对对象和数组特性的不支持。

CSSOM是通过JS操作DOM CSS属性的利器，可以很方便灵活的获取和设置DOM的样式，实际项目中也会经常使用。

# 参考文章

-	[更高效、更安全地操作 CSSOM ：CSS Typed OM)](https://www.zhihu.com/search?type=content&q=CSSOM)
-   [JS 之DOM range对象](http://www.lvesu.com/blog/main/cms-421.html)