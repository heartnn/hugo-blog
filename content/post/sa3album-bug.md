---
title: 万恶的GAE图床
date: 2010-11-12T09:07:00+08:00
categories: 技术
tags: [wordpress]
---

一直用的[Sa3album](http://sa3.org/program/gae-album/)，是从[大菠萝相册]({{< relref "diabloimage.md" >}})进化而来的，主要是增加了多相册的功能，生成的图片地址也很短了，但是有一个问题，没有扩展名的后缀，导致绝大多数的lightbox都无法使用，Python代码又完全不会改，各种郁闷。

但是要仅仅如此郁闷也就算了，这两天折腾wordpress themes，换了theme以后发现原来对图片控制的width="570"是死板的，现在不适合了，于是更郁闷了。

于是乎开始搜索能自动调整大小的插件，可是那些插件大多是针对本地上传图片所用的，对外链调用的完全不起作用，于是一度想放弃现在的相册，但是又不太舍得，最后不在google搜索插件了，找到了一段css代码：<!--more-->

```css
img{
    max-width:95%;
    width: expression(this.width > 570 ? "570px" : true);
    height:auto;
}
```

当然，其中img的定义要在正文的定义之中，具体每个theme的css定义都不太相同。其中那个选择题是做给IE6用的，max-width也是可以调节的，heartnn现在用的是90%，这也具体问题具体分析吧。另外网上还有一个jQuery代码，如果只为实现这么简单的问题来说，有点大材小用了，不过要配合lightbox是个不错的选择了。

类似的插件倒是有一个，实现的方法也很死板：

```php
<?php
/*
Plugin Name: Fix Image/Flash Width
Plugin URI: http://dev.ixiezi.com
Description: 限制页面中显示的图片、Flash等对象的宽度最高为99%，避免撑破页面布局
Version: 0.1
Author: 爱写字
Author URI: http://ixiezi.com
*/

add_action('wp_head', 'ixiezi_fix_image_width');

function ixiezi_fix_image_width()
{
    echo '<style type="text/css" media="screen">img,embed,object{max-width:99%;}</style>';
}
?>
```

现在自动调节算是猥琐的实现了，另一个问题出现了，小图片怎么变大呢，虽然是不会撑开模板了，但是图片总得让人家看清楚吧，于是又在网上折腾。。。终于被我找到一个[Zoom-Highslide](http://wordpress.org/extend/plugins/zoom-highslide/)，我用的是[Sa3album](http://sa3.org/program/gae-album/)，当然这个插件是不能直接用的了，小小的分析了一下，修改了zoom-highslide.php中的一点点代码，竟然可以用了。

```php
define("IMAGE_FILETYPE", "(bmp|gif|jpeg|jpg|png)", true);
```

其中，把那些扩展名全都删掉了吧，成了这个样子：

```php
define("IMAGE_FILETYPE", "()", true);
```

大功告成，顺便提供[两套放大镜的图标](/uploads/2010/11/zoomcur.7z)，用来替换原版highslide那个难看的放大镜(来自<http://tiant.me>)。

![](/uploads/2010/11/zoomcur.png)

2010-11-15 补记: 其实highslide是不需要prototype库的，不知道作者为什么要载入它。可以去掉zoom-highslide.php中下面的代码来取消prototype的加载。

```php
function zoom_highslide_javascript() {
    if ( !function_exists('wp_enqueue_script') || is_admin() ) return;
    wp_enqueue_script('prototype');
    wp_enqueue_script('scriptaculous-effects');
}
```

2010-11-18 补记: [sa3album](http://sa3.org/program/gae-album/)的作者ben发布了新的0.2版本，可以进行切换主题了，而且作者在评论中给出了[外链地址的修改方法](http://sa3.org/program/sa3album-copy/)，这下连Zoom Highslide都不用修改了，顺便把修改方法搬到这里来，很简单，只要修改一处代码就好了。

打开models.py，找到

```python
@property
def copyurl(self):
return "http://%s/f/%s/" %(os.environ['HTTP_HOST'],self.key().name())
```

修改为

```python
@property
def copyurl(self):
return "http://%s/f/%s/#%s" %(os.environ['HTTP_HOST'],self.key().name(),self.name)
```

这样，外链地址的结果就是在原来的基础上加了“#文件名”的状态，再调用的时候就不怕没有扩展名了～～
