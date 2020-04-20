---
title: manim教程系列-颜色 笔记
mathjax: true
toc: true
date: 2020-04-18 13:18:52
tags: [manim, python, 笔记]
categories: [manim, 笔记]
description: 这篇文章是在写 manim教程系列视频 的 颜色 部分时做的一些笔记,包括整个视频的结构和写代码时了解的一些用法的笔记。视频目前已经完成,还没有发布
---

## 视频结构大纲
0. [x] 开头
    - 开始,标题,展示所有要将的方法
1. [x] 颜色的表示
    - 所有`constants.py`中的颜色常量
    - 使用hex表示颜色
    - 使用rgb的ndarray表示颜色
2. [x] 颜色之间的转换
    - `rgb_to_hex`
    - `hex_to_rgb`
    - `color_to_rgb`
    - `rgb_to_color`
    - `color_to_int_rgb`
3. [x] 颜色的运算函数
    - `invert_color`
    - `color_gradient`
    - `interpolate_color`
    - `average_color`
    - `random_color`
4. [x] 设置颜色
    - `Mobject`略,一般上色的都为`VMobject`
    - `color`分为`stroke_color`和`fill_color`
    - 传入`color`, `stroke_color`, `fill_color`
    - `set_color`, `set_stroke`, `set_fill`方法的`color`和`opacity`
5. [x] 给子物体上色
    - `set_color`
    - `set_color_by_gradient`
    - `set_colors_by_radial_gradient`
6. [x] 光泽与渐变色
    - `set_sheen`
    - `set_color`中使用列表达到渐变色

## 一些码视频时的笔记

- `isinstance`函数检测对象的类型
- 对一个字符串进行format时,想要用空格补齐左边到一定个数,可以使用`str(...).rjust(num)`
- 涉及到`Transform`Text的地方,在Text里面不可以有空格,需要用白色的`~`来做出伪空格
- 字符串中查找一个字符的下标可以用`.index(" ", beg=..., end=...)`方法来查找第一次出现的位置,第二次出现需要传入`beg`为第一次位置+1
- manim的`rgb_to_color`函数传入的rgb的值为0~1,不是0~255
- 用for循环遍历字典键值对`for key, value in dic.items():`,遍历其中一部分`for key, value in list(dic.items())[1:3]`将键值对转化为列表,并用切片
- `Arrow`的箭头为`.tip`
- `.keys(),.values()`不为列表,需要套在`list()`里面
- `set_colors_by_radial_gradient`利用中心与center的距离对颜色进行插值,radius外的所有子物体全为outer_color颜色
- 含有`sheen_factor`的物体设置渐变色后与sheen_factor无关