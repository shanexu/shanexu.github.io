---
layout: post
title: "Java Code Conventions"
date: 2013-05-22 21:33
comments: true
categories: [java]
---

今天想聊一下java的代码规范的事情。

想写这样的篇文章并不是什么空穴来风，我想起过去在某公司实习的日子，被当时的主管不停的叮嘱coding style的日子，终于现在轮到自己看代码的时候，才意识coding style的重要性。

于是在google搜索java coding style，结果出来的sun在1997年的时候写的[*Java Code Conventions*](http://www.oracle.com/technetwork/java/codeconv-138413.html)，笔者觉得这是一篇非常值得仔细阅读的文章。在这里笔者摘出一些，自己认为比较重要的条目。

## 1.1 Why Have Code Conventions

Code conventions are important to programmers for a number of reasons:

• 80% of the lifetime cost of a piece of software goes to maintenance.  
软件开发的生命周期中80%时间花在维护上。  
• Hardly any software is maintained for its whole life by the original author.  
在软件开发的整个生命周期中，很少有软件的维护完全是由原开发者完成的。  
• Code conventions improve the readability of the software, allowing engineers to understand new code more quickly and thoroughly.  
代码约定能够提高代码的可读性，能够使得工程师们更快更透彻地理解新代码。  
• If you ship your source code as a product, you need to make sure it is as well packaged and clean as any other product you create.  
如果你要将你的源代码随着你的产品一起发布，你必须确保它们如你的别的产品一样的封装完备且干净。  

### 3.1.3 Class and Interface Declarations

1\. Class/interface documentation comment(/\*\*..\*/)
类或接口的文档  
2\. Class or interfaces statement
类或接口声明  
3\. Class/interface implementation comment(/\*...\*/), if necessary
类或接口的实现注释（可选）  
4\. Class (static) variables
类（静态）变量  
5\. Instance variables
实例变量  
6\. Constructors
构造方法  
7\. Methods
类方法（类方法的组织不需要按照scop来排序，而是按照业务需求来进行排序）  


## 6.1 Number Per Line
One declaration per line is recomended since it encourages commenting. In other words,  
每个声明独占一行，这样也更容易对变量进行注释，也就是说，

``` java
int level; // indentation level
int size;  // size of table
```

is preferred over  
上面的代码比下面的代码更符合规范。

``` java
int level, size;
```

## 6.2 Placement

Put declarations only at the beginning of blocks. (A block is any code surrounded by curly braces "{" and "}".) Don’t wait to declare variables until their first use; it can confuse the unwary programmer and hamper code portability within the scope.  
将变量声明放置在代码块开始的地方。

``` java
void MyMethod() {
    int int1;              // beginning of method block
    if (condition) {
        int int2;          // beginning of "if" block
        ...
    }
}
```

The one exception to the rule is indexes of for loops, which in Java can be declared in the for statement:  
对于这一条，只有一个特例，就是for循环。

``` java
for (int i = 0; i < maxLoops; i++) { ...
```

Avoid local declarations that hide declarations at higher levels. For example, do not declare the same variable name in an inner block:  
注意不要在更深层的代码块中使用与外部代码块相同的变量名。

``` java
int count;
...
func() {
    if (condition) {
        int count;
        ...
    }
    ...
}
```

# 9\. Naming Conventions
命名规范
<table>
<tr>
<th>Identifier Type</th>
<th>Rules for Naming</th>
<th>Rules for Naming|Examples</th>
</tr>
<tr>
<td>Classes</td>
<td>Class names should be nouns, in mixed case with the first letter of each internal word capitalized. Thy to keep your class names simple and descriptive. Use whole words--avoid acronyms and abbreviations (unless the abbreviation is much more widely used than the long form, such as URL or HTML).</td>
<td>class Raster;<br> class ImageSprite; </td>
</tr>
<tr>
<td>Interfaces</td>
<td>Interface names should be capitalized like class names. </td>
<td>interface RasterDelegate;<br>interface Storing;</td>
</tr>
<tr>
<td>Methods</td>
<td>Methods should be verbs, in mixed case with the first letter lowercase, with the first letter of each internal word capitalized. </td>
<td>run();<br>runFast();<br>getBackground();</td>
</tr>
<tr>
<td>Variables</td>
<td>Except for variables, all instance, class, and 
class constants are in mixed case with a lower- 
case first letter. Internal words start with capi- 
tal letters. 
Variable names should be short yet meaningful. The choice of a variable name should be mnemonic--that is, designed to indicate to the casual observer the intent of its use. One-char-acter variable names should be avoided except for temporary “throwaway” variables. Common names for temporary variables are i, j, k, m, and n for integers; c, d, and e for characters.</td>
<td>int i;<br>char cp;<br>flaot myWidth;</td>
</tr>
<tr>
<td>Constants</td>
<td>The names of variables declared class constants and of ANSI constants should be all uppercase with words separated by underscores ("_"). (ANSI constants should be avoided, for ease of debugging.)</td>
<td>int MIN_WIDTH = 4;<br>int MAX_WIDTH = 999;<br>int GET_THE_CPU = 1;</td>
</tr>
</table>

# 10\. Programming Practices

## 10.1 Providing Access to Instance and Class Variables
防止直接访问实例或者类的变量

