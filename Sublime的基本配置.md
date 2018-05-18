---
title: Sublime的基本配置
tags:配置
grammar_cjkRuby: true
---

## Sublime Text使用入门

### 1.下载
和正常的安装软件一样，没啥区别。

> 下载地址：http://www.sublimetext.com/3

这个网站可能上不去，因为是外国的网站，中国给屏蔽了。所以，你可以百度一下，搜索关键词[Sublime Text 3下载][1]

要还是找不到，记得跟我说声。

### 2.安装
双击下载的文件，然后一直下一步就可以了。

### 3.第一次打开
当你第一次打开的时候，你会感觉好丑的界面啊。这么丑的界面，怎么能配得上我家女神呢。所以，咱们来一步一步的把他整的偏亮一些，整的好用一些。

![第一次启动的界面][2]

sublime是有很多插件的，你可以去网上下载，然后安装。这种方式挺复杂的。但是人家既然有插件功能，自然有管理插件的插件了。


### 4.安装插件管理器
按`Ctrl+`，弹出命令行终端，然后把下面这句话复制进去，等待一段时间，即可安装。

![使用终端][3]

需要输入的内容：

```
import urllib.request,os,hashlib; h = '6f4c264a24d933ce70df5dedcf1dcaee' + 'ebe013ee18cced0ef93d5f746d80ef60'; pf = 'Package Control.sublime-package'; ipp = sublime.installed_packages_path(); urllib.request.install_opener( urllib.request.build_opener( urllib.request.ProxyHandler()) ); by = urllib.request.urlopen( 'http://packagecontrol.io/' + pf.replace(' ', '%20')).read(); dh = hashlib.sha256(by).hexdigest(); print('Error validating download (got %s instead of %s), please try manual install' % (dh, h)) if dh != h else open(os.path.join( ipp, pf), 'wb' ).write(by)
```

是的，你没有看错，就是这么多，全部复制，然后粘贴到“输入命令”的那个地方，然后回车，等着就可以了

![如何判断正在加载网络数据][4]


安装成功后，会在Preferences中看到Package Control选项。

![如何判断插件管理器已经安装成功][5]

现在我们已经把插件安装成功了，接下来，就是华丽的配置一下了。

### 5.插件的安装
界面是在是太丑了，不行不行，我要换了他。
首先是把插件管理器打开，有两种方法：
- 在菜单栏中的Preferences->Package Control中打开
- 按`shift+ctrl+p`，然后输入`install package`，选择第一项，等一会会出现一个列表。
	- 这个时候就可以输入你需要安装的插件名了，然后选择，就能安装了

![输入install package，选择install package][6]

![输入插件名][7]

![例如，这是一个主题，选择一个，就能安装了][8]

### 6.插件的使用
比如说，我们刚刚安装的那个主题，我们去哪了找呢？这个可以去Preferences里的Theme中去选择，选择一个喜欢的，就能成不同的基面了。

其他插件也可以在这里使用或者配置。当然，还有很多插件是需要使用命令行的或者是需要自己使用配置文件进行配置的。这些事比较深度的定制或使用了，可以暂时先不用管。

### 打开文件列表
有时候，你要操作很多文件，不可能操作完一个去打开一个吧？这个时候，我们就可以把文件列表栏打开。

![打开步骤][9]

![未显示文件列表前][10]

![显示文件列表后][11]

看到区别了没？一般情况先，我们都是打开文件列表的，因为这样很方便我们去切换文件。


### 调节字体大小
还是在Preferences菜单中，其中有个Font选项，你如果想让字体大一点，就Larger，想小就Smaller。或者直接按住Ctrl键，然后滚动鼠标中键，也可以调节字体的大小。

  [1]: https://www.baidu.com/s?ie=utf-8&f=8&rsv_bp=1&tn=baidu&wd=sublime%20text%203%E4%B8%8B%E8%BD%BD&oq=sublime%2520text%2520%2526lt%253B&rsv_pq=d292ace800004c7a&rsv_t=d54cdiww8SeoTXOkYwvOwV2xyD44wsung0e7V5BmtsSpYh9m7At8yXMYFHs&rqlang=cn&rsv_enter=1&inputT=1146&rsv_sug3=52&rsv_sug2=0&rsv_sug4=1796
  [2]: ./images/1516262054263.jpg
  [3]: ./images/1516264085033.jpg
  [4]: ./images/1516264961276.jpg
  [5]: ./images/1516265628805.jpg
  [6]: ./images/1516266343163.jpg
  [7]: ./images/1516266412309.jpg
  [8]: ./images/1516266463606.jpg
  [9]: ./images/1516266908632.jpg
  [10]: ./images/1516267064396.jpg
  [11]: ./images/1516267090487.jpg