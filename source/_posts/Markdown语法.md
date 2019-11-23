---
title: Markdown语法
toc: true
mathjax: true
date: 2019-01-09 16:05:14
tags: markdown
---

`Markdown`是一款简洁实用的文本标记语言，可以在`mkdocs`,`hexo`中使用

<!--more-->

```
# 一级标题
## 二级标题
### 三级标题
#### 四级标题
##### 五级标题
###### 六级标题
```

下划线
```
---
***
****
----
```
---

#### 目录
`[TOC]`

---
#### 文字样式
**`** **`加粗**<br/>
*`* *`倾斜*<br/>
***`*** ***`倾斜加粗***<br/>
`~~ ~~`~~删除~~

---
#### 引用
`>`一级 `>>`二级
>引用
>>二级引用

>引用

---
#### 空行
`&nbsp; 或 <br/>`
&nbsp;
<br/>

---
#### 空格 
`&emsp;`<br/>
&emsp;&emsp;空格

---

#### 图片
`![图片名](图片地址 "title")`
![markdown](https://gss2.bdstatic.com/9fo3dSag_xI4khGkpoWK1HF6hhy/baike/c0%3Dbaike180%2C5%2C5%2C180%2C60/sign=d997317c11ce36d3b6098b625b9a51e2/00e93901213fb80ef9ceac7132d12f2eb938947d.jpg)

或使用html标签`<img src="..." width="..." height="..." />`

---

#### 链接
`[网页名](地址 "title")`
[百度](http://www.baidu.com/ "百度一下")

---

#### 代码块

```cpp
#include<bits/stdc++.h>
using namespace std;

int main()
{
	printf("hello markdown");
	return 0;
}
```

---

#### 序表
1. 有序1
2. 有序2
3. 有序3

---
`+ 或 - 或 *`
+ 无序
* 无序
- 无序
---
- 一级无序
    - 二级无序
        - 三级无序
            - 四级无序
---

#### 任务列表
`- [ ] ...`<br/>
`- [x] ...`<br/>
`mkdocs`需要`pymdown`中的`pymdownx`模块<br/>
`GitHub`支持

---

#### 表格

```
 |表头|表头|表头|
 |:----------|:----------:|----------:|
 | 左对齐 |居中|右对齐|
```

 |表头|表头|表头|
 |:----------|:----------:|----------:|
 | 左对齐 |居中|右对齐|
 
 ---
 
#### 内联CSS
`<p style="color: #AD5D0F;font-size: 30px; font-family: 'Consolas';">CSS</p>`
<p style="color: #AD5D0F;font-size: 30px; font-family: 'Consolas';">CSS</p>

---

#### 语义标记
```
 <i>斜体</i>
 <b>加粗</b>
 <em>强调</em>
 上标： Z<sup>a</sup>
 下标： Z<sub>a</sub>
 键盘文本： <kbd>Ctrl</kbd>
```
 <i>斜体</i><br/>
 <b>加粗</b><br/>
 <em>强调</em><br/>
 上标： Z<sup>a</sup><br/>
 下标： Z<sub>a</sub><br/>
 键盘文本： <kbd>Ctrl</kbd><br/>
 
---
 
#### 公式

文档末尾添加
```
<script type="text/javascript" async
  src="https://cdnjs.cloudflare.com/ajax/libs/mathjax/2.7.5/latest.js?config=TeX-MML-AM_CHTML">
</script>
<script type="text/x-mathjax-config">
MathJax.Hub.Config({
	  tex2jax: {inlineMath: [['$','$'], ['\\(','\\)']]}
});
</script>
```
使用$\LaTeX$语法编写公式
$$
x \href{why-equal.html} {=} y^2 + 1
$$
$$
x = {-b \pm \sqrt{b^2-4ac} \over 2a}
$$


<script type="text/javascript" async
  src="https://cdnjs.cloudflare.com/ajax/libs/mathjax/2.7.5/latest.js?config=TeX-MML-AM_CHTML">
</script>
<script type="text/x-mathjax-config">
MathJax.Hub.Config({
	  tex2jax: {inlineMath: [['$','$'], ['\\(','\\)']]}
});
</script>
