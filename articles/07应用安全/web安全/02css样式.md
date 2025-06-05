---
layout: default
title: css样式
---

# 内嵌样式

```html
<head>
<!-- 内嵌样式 -->
    <!-- 整个html页面使用同一个CSS文件 -->
    <!-- 内嵌样式写到head里 -->
    <!-- 内嵌样式的标识是<style> -->
    <style>
        h2{
            color: darkorange;
            background-color: aquamarine;
            text-align: center;
        }
    </style>
</head>
```

# 外部样式

```html
<head>
<!-- 外部样式 -->
    <!-- 整个网站的所有页面使用相同的样式 -->
    <!-- 写一个单独的.css样式文件 -->
    <!-- 页面头部引入样式文件 -->
    <!-- 外部样式的标识是rel="stylesheet" -->
    <link rel="stylesheet" href="style.css">
    <!-- relationship关系样式表 -->
</head>
```

```css
h3{
    color: brown;
    background-color: azure;
    text-align: left;
}
```

# 行内样式

```html
<body>
<!-- 行内样式 -->
    <!-- 行内样式给单独的标签设置样式，实现精细化控制 -->
    <!-- 行内样式的标识是style= -->
    <h1 style="background-color: black;color: gold;text-align:center ;">行内样式</h1>
    <!-- badkground-color背景颜色，color字体颜色，text align文本排列 -->
    <h2>内嵌样式</h2>
    <h3>外部样式</h3>
<!-- 查看别人网页的样式文件 -->
    <!-- ctrl + u 查看源码 -->
    <!-- ctrl + f 搜索 -->
    <!-- 行内样式style= -->
    <!-- 内嵌样式<style> -->
    <!-- 外部样式rel="stylesheet" -->
</body>
```
