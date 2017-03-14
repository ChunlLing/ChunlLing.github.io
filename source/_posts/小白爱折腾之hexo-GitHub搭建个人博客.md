---
title: 小白爱折腾之hexo+GitHub搭建个人博客
date: 2017-03-08 14:51:01
tags: hexo
description:
---

非教程帖，仅做挖坑记录和反思   

# 前期准备：
-  安装git
-  安装node.js
-  安装hexo
-  申请GitHub账号

# 搭建步骤：

1 创建仓库：用户名.github.io

2 在本地指定根目录输入命令（cmd或git）：hexo init 用户名.github,io
结果： 生成“用户名.github,io”文件夹

**文件结构树：**
- 用户名.github,io
	- node_mdules
	- scaffolds
	- source		[资源文件]
	- themes		[主题文件]
	- .gitgnore
	- _config.yml	[站点配置文件]
	- package.json	[应用程序数据]

3 在“用户名.github,io”根目录输入命令（cmd或git）：hexo s，在浏览器输入localhost:4000

至此，初级搭建成功。

本地预览： `hexo s`或`hexo server`

_ _ _

# 进阶-写博客：

创建新文章： `hexo new "文章标题"` （存在source\_posts目录中）

创建草稿： `hexo new draft "文章标题"` （会在source文件夹中新建一个文件夹“_drafts”）
发布草稿： `hexo publish "文章标题"` （会将存在source\_drafts的草稿发布到"_posts"文件夹中）

安装自动部署发布工具：`hexo-deployer-git`
没安装的后果就是后面`hexo d`会出现 “cannot found git” 的错误

清除缓存：`hexo clean`
生成静态文件：`hexo generate`或`hexo g`
部署到git上：`hexo deploy`或`hexo d`


_ _ _

# 进阶-更换主题：

到[Hexo](https://hexo.io/themes/)选择喜欢的主题。这里选了[Yelee](https://github.com/MOxFIVE/hexo-theme-yelee)这个主题。
[Yelee安装教程](http://moxfive.coding.me/yelee/)。教程很详细，这里就不细述了b(￣▽￣)d

趁着还有半小时断网，再试一个主题[Hexo 主题：SPFK](http://luuman.github.io/2015/12/27/Hexo/HexoTheme/)


最后本博客选择Yelee和SPFK两个主题混搭。主要用了SPFK的样式，但用了Yelee的一些配置。


## **站点Swiftype搜索框**
1. 目标文件：`\themes\spfk\source\css\_partial`
	操作：添加文件【添加`\themes\yelee\source\css\_partial`目录下的`search.styl`文件】

2. 目标文件：`\themes\spfk\source\js\_partial`
    操作：添加文件【添加`\themes\yelee\source\js\_partial`目录下的`search.js`文件】

3. 目标文件：`\themes\spfk\layout\_partial\left-col.ejs`
    操作：替换代码【将第一段代码替换成第二段代码】
    ```
    <% if (theme.search_box){ %>
        <form>
            <input type="text" class="st-default-search-input search" id="search" placeholder=" Search...">
        </form>
    <%}%>

    ```

    ```
    <% if (theme.search.on){ %>
        <form id="search-form">
        <input type="text" id="local-search-input" name="q" placeholder="search..." class="search form-control" autocomplete="off" autocorrect="off" searchonload="<%= theme.search.onload %>" />
        <i class="fa fa-times" onclick="resetSearch()"></i>
        </form>
        <div id="local-search-result"></div>
        <p class='no-result'>No results found <i class='fa fa-spinner fa-pulse'></i></p>
    <%}%>
    ```

4. 目标文件：`\themes\spfk\source\js\pc.js`
    操作：添加代码【添加到`define([], function(){})`的`function`里面`return`之前】
    ```
    if (yiliaConfig.search) {
        var search = function(){
            require([yiliaConfig.rootUrl + 'js/search.js'], function(){
                var inputArea = document.querySelector("#local-search-input");
                var $HideWhenSearch = $("#toc, #tocButton, .post-list, #post-nav-button a:nth-child(2)");
                var $resetButton = $("#search-form .fa-times");
                var $resultArea = $("#local-search-result");

                var getSearchFile = function(){
                    var search_path = "search.xml";
                    var path = yiliaConfig.rootUrl + search_path;
                    searchFunc(path, 'local-search-input', 'local-search-result');
                }

                var getFileOnload = inputArea.getAttribute('searchonload');
                if (yiliaConfig.search && getFileOnload === "true") {
                    getSearchFile();
                } else {
                    inputArea.onfocus = function(){ getSearchFile() }
                }

                var HideTocArea = function(){
                    $HideWhenSearch.css("visibility","hidden");
                    $resetButton.show();
                }
                inputArea.oninput = function(){ HideTocArea() }
                inputArea.onkeydown = function(){ if(event.keyCode==13) return false}

                resetSearch = function(){
                    $HideWhenSearch.css("visibility","initial");
                    $resultArea.html("");
                    document.querySelector("#search-form").reset();
                    $resetButton.hide();
                    $(".no-result").hide();
                }

                $resultArea.bind("DOMNodeRemoved DOMNodeInserted", function(e) {
                    if (!$(e.target).text()) {
                        $(".no-result").show(200);
                    } else {
                      $(".no-result").hide();
                    }
                })
            })
        }()
    }
    ```

5. 目标文件：`\themes\spfk\layout\_partial\after-footer.ejs"`
    操作：删除代码
    ```
    <% if (theme.search_box){ %>
        <script type="text/javascript">
          window.onload = function(){
            document.getElementById("search").onclick = function(){
                console.log("search")
                search();
            }
          }
          function search(){
            (function(w,d,t,u,n,s,e){w['SwiftypeObject']=n;w[n]=w[n]||function(){
            (w[n].q=w[n].q||[]).push(arguments);};s=d.createElement(t);
            e=d.getElementsByTagName(t)[0];s.async=1;s.src=u;e.parentNode.insertBefore(s,e);
            })(window,document,'script','//s.swiftypecdn.com/install/v2/st.js','_st');

            _st('install','A1Pz-LKMXbrzcFg2FWi6','2.0.0');
          }
        </script>
    <%}%>
    ```


6. 目标文件：`\themes\spfk\source\css\style.styl`
    操作：添加代码【添加到最后防止被覆盖】
    ```
    if search
      @import "_partial/search"

    ```

上面主要是实现本地搜索功能，修改样式看个人喜好，主要修改的样式文件是：`\themes\spfk\source\css\_partial\main.styl`
小技巧：`Ctrl+F`调出查找栏，输入“search”，对包含有这个对象的文件进行相应的修改，耐力活 =_=


## **阅读全文设置**
[Hexo自动添加ReadMore标记](https://twiceyuan.com/2014/05/25/hexo%E8%87%AA%E5%8A%A8%E6%B7%BB%E5%8A%A0readmore%E6%A0%87%E8%AE%B0/)

_ _ _

# 进阶-使用插件

## **使用七牛云**
1. 注册[七牛](https://www.qiniu.com/)云账号。注册完成后添加对象存储，记住空间名称，后面要用到
2. 在根目录下执行：` npm install hexo-qiniu-sync --save `
3. 在站点配置文件`_config.yml`（在根目录下）中配置：
    ```
    # 七牛云
    qiniu:
      offline: false
      sync: true
      bucket: #空间名称
    # secret_file: sec/qn.json or C:
      access_key: #七牛个人中心/密钥管理复制
      secret_key: #七牛个人中心/密钥管理复制
      dirPrefix: static
      urlPrefix: http://bucket_name.qiniudn.com/static
      up_host: http://upload.qiniu.com
      local_dir: static
      update_exist: true
      image: 
        folder: images
        extend: 
      js:
        folder: js
      css:
        folder: css
    ```

    这里注释了secret_file: sec/qn.json or C:是因为会报错

    ```
     FATAL incomplete explicit mapping pair; a key node is missed at line 74, column 32:
          secret_file: sec/qn.json or C:
          ...
    ```

4. 执行`hexo qiniu s`同步上传

_ _ _

# 参考：
- [免费个人博客搭建详解](http://www.jianshu.com/p/380290deb8f0)
- [手把手教你使用Hexo + Github Pages搭建个人独立博客](http://div.io/topic/1691)
- [Yelee —— 简而不减 Hexo 双栏博客主题](http://moxfive.coding.me/yelee/)


- - -

# 遇到的问题：
1. 使用hexo d之后直接输入`https://用户名.github.io`显示404
答： 没有配置deploy的type和repo

2. 更改以前的文章内容，使用hexo clean、hexo g、hexo d没有用，而且localhost:4000也一样
答： 因为修改的文件不是_post里面的文件...(╯‵□′)╯︵┻━┻z
反思： 要细心阿！！！

3. 编辑输入时替换后一格内容【这个不在hexo配置范畴内】
答：按下`Insert`键试试？

4. 关于markdown的有序列表一点小问题。编辑的时候应该为子项内容缩进一格，否则如果当子项内容长时该子项后面的子项序号会重新从1开始。

5. markdown表格？似乎没有被渲染成表格。
	```
    | column | column |
    |--------|--------|
    |        |        |
    ```
    因为被浏览器（Chrome）检测为不安全脚本了```

```


