---
layout: post
title: "Selecting HTML Comments with jQuery"
date: 2013-03-13 12:40
comments: true
categories: [javascript, jQuery]
---

<div class='begin-indent2em' filter='p:not(:has(a.fancybox :first-child))'></div>
前几日想在博客blogs列表上上做一个read more & less的效果，几经周折，总算做了一个比较满意的版本。其实原理很简单只是细节问题接踵而至。octopress中有*&lt;!&minus;&minus; more &minus;&minus;&gt;*这个注释把文章分成两截，其实在执行*Read more*的时候只要从文章中找到*&lt;!&minus;&minus; more &minus;&minus;&gt;*注释并截取下半截，并添加到原来的“.entry-content div”中即可。一开始也没有多想，在我看来jQuery应该是没有选取注释的选择器的，的确最后证明我的想法的确是正确的。于是又用了在文档中添加display为none的元素的方法。于是我就是在*&lt;!&minus;&minus; more &minus;&minus;&gt;*的后面增加了一个*&lt;div class='read-more-mark'&gt;&lt;/div&gt;*这样的标记性元素。于是要选取后半截只要这样就行了。

``` javascript
$('.read-more-mark').nextAll();
```

<!-- more --><div class='read-more-mark'></div>
但在文档中插入没什么用的标记元素总觉得不太好，经过google关键词检索搜到了一篇题为[jQuery Comments() Plug-in To Access HTML Comments For DOM Templating](http://www.bennadel.com/blog/1563-jQuery-Comments-Plug-in-To-Access-HTML-Comments-For-DOM-Templating.htm)文章提供的代码如下：

{% codeblock jQuery Comments() Plug-in lang:javascript http://www.bennadel.com/index.cfm?event=blog.downloadcode&id=1563&index=1 Download %}
// This jQuery plugin will gather the comments within
// the current jQuery collection, returning all the
// comments in a new jQuery collection.
//
// NOTE: Comments are wrapped in DIV tags.
 
jQuery.fn.comments = function( blnDeep ){
	var blnDeep = (blnDeep || false);
	var jComments = $( [] );
 
	// Loop over each node to search its children for
	// comment nodes and element nodes (if deep search).
	this.each(
		function( intI, objNode ){
			var objChildNode = objNode.firstChild;
			var strParentID = $( this ).attr( "id" );
 
			// Keep looping over the top-level children
			// while we have a node to examine.
			while (objChildNode){
 
				// Check to see if this node is a comment.
				if (objChildNode.nodeType === 8){
 
					// We found a comment node. Add it to
					// the nodes collection wrapped in a
					// DIV (as we may have HTML).
					jComments = jComments.add(
						"<div rel='" + strParentID + "'>" +
						objChildNode.nodeValue +
						"</div>"
						);
 
				} else if (
					blnDeep &&
					(objChildNode.nodeType === 1)
					) {
 
					// Traverse this node deeply.
					jComments = jComments.add(
						$( objChildNode ).comments( true )
						);
 
				}
 
				// Move to the next sibling.
				objChildNode = objChildNode.nextSibling;
 
			}
 
		}
		);
 
	// Return the jQuery comments collection.
	return( jComments );
}
{% endcodeblock %}

上面的代码可以揣摩出，使用xml dom element的.nextSlibling方法是可以获得任何类型的xml dom element的。下表是从w3schools.com中摘录的*NodeTypes - Named Constants*表格。

<table>
    <tr>
        <th>NodeType</th>
        <th>Named Constant</th>
    </tr>
    <tr>
        <td>1</td>
        <td>ELEMENT_NODE</td>
    </tr>
    <tr>
        <td>2</td>
        <td>ATTRIBUTE_NODE</td>
    </tr>
    <tr>
        <td>3</td>
        <td>TEXT_NODE</td>
    </tr>
    <tr>
        <td>4</td>
        <td>CDATA_SECTION_NODE</td>
    </tr>
    <tr>
        <td>5</td>
        <td>ENTITY_REFERENCE_NODE</td>
    </tr>
    <tr>
        <td>6</td>
        <td>ENTITY_NODE</td>
    </tr>
    <tr>
        <td>7</td>
        <td>PROCESSING_INSTRUCTION_NODE</td>
    </tr>
    <tr>
        <td>8</td>
        <td>COMMENT_NODE</td>
    </tr>
    <tr>
        <td>9</td>
        <td>DOCUMENT_NODE</td>
    </tr>
    <tr>
        <td>10</td>
        <td>DOCUMENT_TYPE_NODE</td>
    </tr>
    <tr>
        <td>11</td>
        <td>DOCUMENT_FRAGMENT_NODE</td>
    </tr>
    <tr>
        <td>12</td>
        <td>NOTATION_NODE</td>
    </tr>
</table>

在重新检索jQuery API文档之后得到了一个[*.contents()*](http://api.jquery.com/contents/)方法。

>Given a jQuery object that represents a set of DOM elements, the .contents() method allows us to search throughthe immediate children of these elements in the DOM tree and construct a new jQuery object from the matching elements. The .contents() and .children() methods are similar, except that the former includes text nodes as well as HTML elements in the resulting jQuery object.
>
>The .contents() method can also be used to get the content document of an iframe, if the iframe is on the same domain as the main page.

利用*.contents()*方法可以下面的方式获得文档中的注释。

``` javascript
$(".entry-content").contents().filter(function(){
    return this.nodeType === 8;
})
```
