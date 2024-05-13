---
title: Pyqt Designer
categories:
  - 杂谈
tags:
  - 笔记， pyqt
date: 2023-03-29 21:01:48
---

## 一些常用属性

1. **minimumSize**：

   表示部件能被缩小到的最小尺寸，单位为像素，缩小到该尺寸后不能再进一步缩小了。如果部件在布局管理器中，且布局管理器也设置了最小尺寸，则部件本身的最小尺寸以部件的minimumSize为准，布局管理器设置的不起作用。

2. **sizePolicy**

   部件的sizePolicy属性用于说明部件在布局管理中的缩放方式，当部件没有在布局管理器中时，该设置无效。

   ![img](https://raw.githubusercontent.com/WMGray/blog.images/master/images/20191020165934198.png)

   其中的常量值本身是由枚举类型PolicyFlag 的多个值组合而成，PolicyFlag 的取值及含义如下

   ![a](https://raw.githubusercontent.com/WMGray/blog.images/master/images/20191020170110365.png)

3. **contextMenuPolicy**

   contextMenuPolicy为部件的快捷菜单策略，快捷菜单通过在部件上点击鼠标右键触发。

4. 