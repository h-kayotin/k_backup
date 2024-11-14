一个Wordpress的博客已经搭建完成了，那么为了让它看起来更有人间烟火气一点，有一些常用的初始配置，这里整理一下。

### 修改页脚

页脚这里默认会显示Powered by Wordpress，还有一个原因是这里要加上备案信息。在主题里找到页脚，然后修改即可。比如我使用的主题，是修改**site-info.php (template-parts/footer/site-info.php)**

如下，我注释掉了它本来的内容，然后加上了我的内容。

```html
<div class="site-info">
	<?php
	if ( function_exists( 'the_privacy_policy_link' ) ) {
		the_privacy_policy_link( '', '<span role="separator" aria-hidden="true"></span>' );
	}
	?>
	<a href="<?php echo esc_url( __( 'https://wordpress.org/', 'twentyseventeen' ) ); ?>" class="imprint">
<!-- 		<?php
			/* translators: %s: WordPress */
		printf( __( 'Proudly powered by %s', 'twentyseventeen' ), 'WordPress' );
		?> -->
		Copyright © Kayotin. All Rights Reserved.&nbsp;&nbsp;
	</a>
	<a href="https://beian.miit.gov.cn/#/Integrated/index" target="_blank">豫ICP备19030615号</a>
</div><!-- .site-info -->
```

### 网站logo设置

默认Wordpress站点logo是一个W，作为一个专业的博客肯定要修改成自己的logo。

![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/6d90d715-36a6-4383-80ae-2060c8094f8e/c2f65bbf-4529-4d4b-a8d8-d01b4b39da8e/image.png)

### 添加看板娘

我们二次元就是要搞看板娘，首先准备好代码，比如从这里下载。

https://github.com/h-kayotin/k_backup

这个是带换装的，还带hitokoto，也就是过一会显示一句动漫台词。

下载完毕后，放在主目录下，目录名称是live2d

```bash
[root@node wordpress]# ls
index.php    readme.html      wp-blog-header.php    wp-config.php         wp-cron.php        wp-load.php   wp-settings.php   xmlrpc.php
license.txt  wp-activate.php  wp-comments-post.php  wp-config-sample.php  wp-includes        wp-login.php  wp-signup.php
live2d       wp-admin         wp-config-docker.php  wp-content            wp-links-opml.php  wp-mail.php   wp-trackback.php
```

在header.php中，引入css

```python
<link rel="stylesheet" href="/live2d/css/live2d.css" />
<script type="text/javascript" src="https://apps.bdimg.com/libs/jquery/2.1.4/jquery.js"></script>
```

在你博客程序页脚文件（footer.php）引入脚本，在最后一个 </body> 标签前插入如下代码**：**

```python
<div id="landlord">
    <div class="message" style="opacity:0"></div>
    <canvas id="live2d" width="280" height="250" class="live2d"></canvas>
    <div class="hide-button">隐藏</div>
    <div class="switch-button">换装</div>
</div>

<script type="text/javascript">
    var message_Path = '/live2d/'
    var home_Path = 'https://kayotin.cn/'  //此处修改为你的域名，必须带斜杠
</script>
<script type="text/javascript" src="/live2d/js/live2d.js"></script>
<script type="text/javascript" src="/live2d/js/message.js"></script>
<script type="text/javascript">
    var index = Math.ceil(Math.random()*37)
    loadlive2d("live2d", "/live2d/model/pio/model_"+index+".json");
</script>
```

如果需要修改看板娘的词条，可以修改message.json；

修改格式啥的就在css里。

### 禁用主题更新

事实上Wordpress的所有自定义设置都是在主题文件中编辑的。这就意味着，如果更新了主题，这些自定义设置都会化为乌有，解决这个问题也很简单。我们直接禁止主题更新就完事了。

使用插件**Easy Updates Manager，安装后，在仪表板-updates options中进行设置即可。**

### 修改上传文件限制

修改文件.htaccess 注意这个是隐藏文件，就是网站根目录下面。

```python
php_value upload_max_filesize 64M
php_value post_max_size 64M
php_value max_execution_time 300
php_value max_input_time 300
```