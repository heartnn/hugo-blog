---
title: Ghost博客修正时区的方法
date: 2015-12-17T09:39:22+08:00
categories: 技术
tags: [ghost,"moment.js"]
---

最近翻看以前的文章时，偶尔发现有的文章显示时间不正确，编辑器里显示的时间是正常的，才知道Ghost里只是显示UTC时间。

我不知道调整服务器的时区的方法是否可行，因为Ghost在Github上似乎也没有给出绝对的答案，所以解决的办法只能是对应浏览者的本地时区。

Ghost官方基本给出了[解决办法](http://dev.ghost.org/local-dates-themes/)，利用Moment.js，只需要稍微修改即可。<!--more-->

```htmlbars
<script type="text/javascript" src="https://cdnjs.cloudflare.com/ajax/libs/moment.js/2.10.6/moment.min.js"></script>  
<script type="text/javascript" src="https://cdnjs.cloudflare.com/ajax/libs/moment.js/2.10.6/locale/zh-cn.js"></script>  
<script type="text/javascript" src="https://cdnjs.cloudflare.com/ajax/libs/moment-timezone/0.4.1/moment-timezone-with-data-2010-2020.min.js"></script>

<script type="text/javascript">  
  $(document).ready(function () {
    moment.locale('zh-cn');

    $('.post-date').each(function (i, date) {
      var $date = $(date);

      $date.html(
        moment($date.attr('datetime'))
          .tz('Asia/Shanghai')
          .format('LLL')
       );
    });
  });
</script>
```

假设你用的是Ghost默认模板，将上面一段插入到`default.hbs`中jQuery后面。并将模版中涉及到时间的部分修改为：

```htmlbars
<time class="post-date" datetime="{{date format="YYYY-MM-DDTHH:mm:ss.SS\Z"}}"></time>
```

关于上面`format('LLL')`，下面是对应zh-cn的详细解释：

```js
moment().format('L');    // 2015-12-17
moment().format('l');    // 2015-12-17
moment().format('LL');   // 2015年12月17日
moment().format('ll');   // 2015年12月17日
moment().format('LLL');  // 2015年12月17日晚上10点36分
moment().format('lll');  // 2015年12月17日晚上10点36分
moment().format('LLLL'); // 2015年12月17日星期四晚上10点36分
moment().format('llll'); // 2015年12月17日星期四晚上10点36分
```

关于更多的Moment.js用法请前往[主页](http://momentjs.com/)。
