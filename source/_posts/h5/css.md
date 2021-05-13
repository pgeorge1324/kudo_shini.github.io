---
date: 2021-05-13 14:26:52
title: CSS常见问题及解决
tags:
- h5
- css
categories:
- [h5, css]
---

# CSS常见问题及解决：

- ##  background-size使用不生效

```css
// 1.注意顺序background-size必须定义在background:url("")后
// background-size:contain;---图片自身的宽高比不变，缩放至图片自身能完全显示出来，所以容器会有留白区域；
// background-size：100% 100%;---按容器比例撑满，图片变形；
// background-size:cover;---把背景图片放大到适合元素容器的尺寸，图片比例不变，但是要注意，超出容器的部分可能会裁掉。
```

- ## background常见设置


```css
// background:url("") no-repeat center center
// background:url("") no-repeat 0 0
// background:bg-color bg-image position/bg-size bg-repeat bg-origin bg-clip bg-attachment initial|inherit;
```

- ## 控制行内元素不换行而是横向溢出


```css
// 父节点 white-space:no-wrap
// 子节点 inline-block
```

- ## 文字溢出省略


```css
// -webkit-line-clamp: 2;
    display: -webkit-box;
    overflow: hidden;
    -webkit-box-orient: vertical;
```

- ## 初始化所有元素的margin和padding

```css
*{
	margin:initial;
	padding:initial;
}
```

- ## 控制文字为不可复制


```css
user-select: none;
```

- ## 设置全局高度


```css
html,html>*{
  height: 100%;
}
```

- ## 灰度图


```css
filter: grayscale(100%) brightness(200%);
```

- ## css前缀与浏览器关联


```css
-moz代表firefox浏览器私有属性
-ms代表ie浏览器私有属性
-webkit代表safari、chrome私有属性
```

- ## absolute和zIndex的层级关系


```css
对于没有设置 position:absolute 属性的元素 不管 z-index 设置多少都为 0
但低于 position:absolute 中 z-index:0 的元素
```



