---
title: "博客升级到Wordpress 3.2"
date: 2011-07-06T22:12:00+08:00
categories: 技术
tags: [wordpress]
---

![](/uploads/2011/07/wordpress-3.2.jpg)

确切的说是昨天升级的，基本上没有什么兼容性问题，不过Twenty Eleven主题的问题还算不少。<!--more>

首先是原来的AI Loader(就是JQuery Lazy Load)不能用了，用YS images lazyload來代替了，並且Twenty Eleven默认没有引用JQuery，所以需要在header.php中添加。

找到

```htmlbars
<!--[if lt IE 9]>
    <script src="<?php echo get_template_directory_uri(); ?>/js/html5.js" type="text/javascript"></script>
<![endif]-->
```

在上面添加

```php
<?php wp_enqueue_script( 'jquery' ); ?>
```

Wordpress 3.2的后台要紧凑了一些，不过heartnn用的Admin Drop Down Menu，所以没感觉有太大变化。(Ozh' Admin Drop Down Menu更新很及时了，刚装上新版Wordpress就马上提示插件更新了～)

PageNavi的问题比较麻烦，原来的主题是自带的页面导航，比较方便，现在Twenty Eleven和以前的Twenty Ten一样麻烦，我是修改的functions.php。

```php
/**
 * Display navigation to next/previous pages when applicable
 */
function twentyeleven_content_nav( $nav_id ) {
    global $wp_query;
    if(function_exists('wp_pagenavi')) { wp_pagenavi();}
    else if ( $wp_query->max_num_pages > 1 ) : ?>
    <nav id="<?php echo $nav_id; ?>">
        <h3 class="assistive-text"><?php _e( 'Post navigation', 'twentyeleven' ); ?></h3>
	    <div class="nav-previous"><?php next_posts_link( __( '<span class="meta-nav">&larr;</span> Older posts', 'twentyeleven' ) ); ?></div>
	    <div class="nav-next"><?php previous_posts_link( __( 'Newer posts <span class="meta-nav">&rarr;</span>', 'twentyeleven' ) ); ?></div>
	</nav><!-- #nav-above -->
<?php endif;
}
```

注意上面6、7行的修改。 另外页面导航会同时出现在文章列表顶部和底部，不需要顶部的就去index.php注释掉nav-above一行。
