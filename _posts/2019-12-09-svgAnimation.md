---
layout:     post
title:      "SVG Animation"
subtitle:   "如何不使用js、css3做出簡單的動畫效果？"
date:       2019-12-09 12:00:00
author:     "Han"
header-img: "img/post-bg-svg.jpg"
catalog: true
tags:
    - Learning
---
> Finally got the chance to organize something after 8 months. There are projects through projects I've been involved. Sometimes need to work overtime a bit, sometimes dedicate myself into learning. I dived into learning SVG animation recently. This article is just for myself as a learning note. And hope it can help someone who's learning as well.

Speaking about SVG animation, we'll directly think to `SMIL`. The whole word is `Synchronized Multimedia Integration Language`.
There's five main attributes we can use in `SMIL`.

***SVG Animation***
・<set>
沒有動畫效果，不是連續的動畫但可以實現基本的延遲功能。=>可以在特定時間後修改某一個attribute(也可以是CSS attribute)。
範例：
	<set attributeName="x" attributeType="XML" to="60" begin="3s" />

・<animate>
基礎動畫元素，實現單attribute的動畫過度效果(與set差別在於set不重複)
範例：
	<animate attributeName="x" from="160" to="60" begin="0s" dur="3s" repeatCount="indefinite" />

・<animateColor>
已被廢棄

・<animateTransform>
類似於CSS3 transform / Snap.svg.js transform效果
範例：
```
  <animateTransform attributeName="transform" begin="0s" dur="3s" type="scale" from="1" to="1.5" repeatCount="indefinite”/>
```
・<animateMotion>
可以讓svg圖形沿著特定的path移動，為svg增加了很大部分的可動畫性。
範例：
```
	<svg width="360" height="200" xmlns="http://www.w3.org/2000/svg”>
		<text font-family="microsoft yahei" font-size="40" x="0" y="0" fill="#cd0000”>马
			<animateMotion path="M10,80 q100,120 120,20 q140,-50 160,0" begin="0s" dur="3s" repeatCount="indefinite”/>
		 </text>
		<path d="M10,80 q100,120 120,20 q140,-50 160,0" stroke="#cd0000" stroke-width="2" fill="none" />
	</svg>
```

以上我自己最常使用到的是animate，使用<<animate>>時在begin的attribute放入click，可以在不用到js的情況下操縱頁面的點擊事件，能夠利用<<g>>或是<id>來控制各個元素是否要受到點擊事件的影響。

其餘有趣的內容會找時間再整理codepen分享。
