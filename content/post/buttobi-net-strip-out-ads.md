---
title: "buttobi.net去广告攻略"
date: 2010-10-26T23:42:00+08:00
categories: 网络
tags: [html]
---

今天整理ftp软件的时候发现的，buttobi.net是一个日本空间，号称是无限容量，支持php+mysql，唯一缺点是不能绑定域名，貌似小日本的空间都这样，而且是不支持`.htaccess`的。一直没怎么用，传了个wordpress上去后，发现上下都有广告，于是很郁闷，在网上搜索也找不到去广告的方法，于是自己动手。

过程是复杂的，结果是开心的。以下攻略仅针对wordpress，其他请自行修正。

其实下面的广告超级好去，只要修改footer模板，在`</body>`前加`<style>`就可以了。

难的是上面的广告，由于`<div style="position: relative; z-index:255;">`得控制，`<DIV style="MARGIN-TOP: -60px">`的方法也不管用了，于是超级郁闷，而且一开始感觉这个广告的插入代码位置不定。在`</header>`两旁加入了`<style>`和`</style>`之后也没什么反应。

由于wordpress的header中含有部分php代码，开始怀疑插入位置和php标记有关，一开始证明好像是这样的，因为php标记中正好是meta和style的部分。(事实证明我一开始的设想是错误的，那个广告代码插入的位置正好是在style之前)<!--more-->

现在感觉越来越郁闷了，于是在`</title>`两边加上了`<style>`和`</style>`，结果广告代码去标题栏了，更证明了我上面括号里说的，我也是从现在才真正知道了广告的插入位置。

剩下的过程就稍微简单点了，直接出结果吧，我用的是`<noscript>`，是因为怕`<style>`出现混乱。把下面的代码部分加入到`</title>`后面就好了。

```htmlbars
<title><noscript></title></noscript><noscript></noscript>
```

写双重`<noscript>`的目的是防止广告中以后出现类似script的代码。

另有个ohost.de去廣告的方法很有意思：

把含有`</body>`的全部改成`<xml></body></xml>`并且在`</html>`后加上`<comment>`，不知道`<comment>`标签起了什么作用。
