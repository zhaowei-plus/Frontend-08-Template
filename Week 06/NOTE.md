# 课程第六周

> 重学JavaScript，巩固基础知识

# 前言

本周JavaScript的基本知识第一部分，主要有关于JavaScript语言的来源于思想，设计方式和基础类型等

# 正文

## JavaScript原子（Atom）

-   语法部分
    Literal、Variable、Keywords、Whitespace、LineTerminator，是组成Js的最小的元素，虽然不会产生实际上的语言作用，但是可以让我们的整个语言的样式更好看一些，维护更加方便。

-   运行时
    Types、Execution  Context

## JavaScript的基本类型

### Number

IEEE 754 Double Float，表示双精度浮点数，其存储方式可以查看，基本思想就是把一个数字拆解成指数和有效位数，指数决定范围，有效位数决定精度，还有一个符号位，节后如下：

1符号位（1 负值 0 正值）（Sign） + 11指数位（分为正值和负值，所以最大位 2^10）（Exponent） + 52个精度位（具有相应的隐藏位）（Fraction）

-   基本语法

需要注意小数点的位置，注意2进制、8进制、16进制等不同形式的表示方式，特别注意：0.toString()和 0 .toString()

### String

字符串类型，即文本类型

-   Charater（字符）
计算机中使用 Code Point（码点 ）来表示一个字符，如97表示a，不同的字符集是通过

数字，如97->a
ASCII码，值规定了127个字符，包括大写字母、小写字母、特殊符号以及制表符等特殊符号

-   Encoding（字符集）

字符串的字符集也有很多，是在不同时间和不同地区演化而来，满足不同的场景，但都是兼容ASCII码

ASCII - 单字节
Unicode - 双字节

全世界的字符都进行统一，两个字节表示0000 ~ FFFF

UCS

GB（国标）
-   GB2312
-   GBK(GB13000)
-   GB18030

ISO-8859 - 与国标类似，东欧国家的编码集
BIG5-台湾的编码集（称为大五码）
特定地区的编码集

UTF
-   UTF8 
-   UTF16

中文一个字节不够存储，需要规定保存格式，储存的空间不太一样

## Boolean

Boolean表示真值，有true false两个值，并且这两个值都是关键字

## Null & Undefined

都表示空值，null表示有值，但是为空，unde是表示为赋值

null - 关键字，是关键字，不可被赋值
undefined - 值，并不是关键字，可以被赋值，一般用 void 0表示

## Object

Object对象类型，在JavaScript运行时，原生对象的描述方式非常简单，我们只需要关心原型和属性两个部分

-   原型
-   属性

JavaScript属性是一个key-value对，key可以是Symbol，也可以是字符串，属性主要分为两种

数据属性（Data Property），一般同于描述状态，当数据属性中如果存储函数，也可以用来描述行为
访问器属性 （Accessor Property），一般用户描述行为

-   API相关

 Object.defineProperty
Objetc.create/setPrototypeOf/getPrototypeOf
new / class / extends
neew / function / prototype

-   Object[[call]]

JavaScript中存在一些特殊的对象，如函数对象，除了一般的对象属性和原型，函数对象还有一个行为[[call]]，可以利用JavaScript中的funtion关键字、箭头运算符或者Function构造器创建的对象，会有[[call]]这个行为，当用类似f()的语法吧对象仿作函数调用时，会方位到[[call]]这个行为，如果对象中没有[[call]]行为，则会报错

- 数组对象
- Object.prototype（没有setPrototypeOf方法）
- Host Object（也可以支持[[call]] [[construct]]方法）

## Symbol

一般用于对象的属性名表示，一般常用的如

Symbol、Symbol.for

## 总结

学习巩固JavaScript基础知识，可以了解相关理论知识，加深对JavaScript的理解