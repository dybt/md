---
title: Hexo+Next主题集成Algolia搜索
date: 2016-8-12 20:22:33
tags:
  - Hexo
  - Next
  - Algolia
categories: 个人博客
---

## 前言

本来这个网站是没有做搜索功能的，因为各种解决方案都不是很完美，而且现在文章也不多，就干脆没有做。偶然间看到了`Next`主题作者[iissnan](http://notes.iissnan.com/)的个人网站的搜索功能不仅速度很快而且也比较符合我的审美，就研究了一下。写下此文章作为记录。

[Algolia](https://www.algolia.com/)是一家致力于为用户提供毫秒级的数据库搜索服务的法国初创公司。其初衷是致力于让客户可以获得“100ms”等级的实时搜索服务。

<!-- more -->

本文要实现的效果如下：

![](/images/2016/08/2.png)

## 更新

如果你是使用的[Next](https://github.com/iissnan/hexo-theme-next)主题，其5.1以上版本已经集成该功能(Github Branch目前最新版本为5.1)。

### 激活方法

在站点配置文件`_config.yml`中写入

```
algolia:
  applicationID: 'applicationID'
  apiKey: 'apiKey'
  adminApiKey: 'adminApiKey'
  indexName: 'indexName'
  chunkSize: 5000
```

在主题配置文件`_config.yml`中写入

```
algolia_search:
  enable: true
```

Enjoy :)

## 上传数据到`Algolia`数据库

### 注册帐号并获得需要的信息

进入[官网地址](https://www.algolia.com/)注册帐号

![](/images/2016/08/3.png)

右上角`NEW INDEX`如图所示

![](/images/2016/08/4.png)

来到[API-KEYS](https://www.algolia.com/api-keys)页面，这里有我们待会儿需要的信息（还有上面的`INDEX`名）。

![](/images/2016/08/5.png)

### 安装[hexo-algoliasearch](https://github.com/LouisBarranqueiro/hexo-algoliasearch)插件并填写配置信息

在Hexo的根目录下执行

```
npm install hexo-algoliasearch --save
```

在根目录的站点配置文件`_config.yml`中加入如下配置，注意改成前面第一步注册获得的数据

```
algolia:
  appId: 'appId'
  apiKey: 'apiKey'
  adminApiKey: 'adminApiKey'
  indexName: 'indexName'
  chunkSize: 5000
  fields:
    - title
    - slug
    - content:strip
```

接着执行，确保得到提交成功提示

```
hexo algolia
```

如下图

![](/images/2016/08/6.png)

然后在`Algolia`后台的`Indices`看是否有我们网站的文章信息，如果有就说明我们的数据提交成功了。

![](/images/2016/08/7.png)

Tips:如果无法运行成功，可以试着先执行如下命令：

```
hexo clean
```

## 主题集成`Algolia`

### 新建`algolia.swig`：

```html
<div class="site-search">
  <div class="algolia-popup popup">
    <div class="algolia-search">
      <div class="algolia-search-input-icon">
        <i class="fa fa-search"></i>
      </div>
      <div class="algolia-search-input" id="algolia-search-input"></div>
    </div>

    <div class="algolia-results">
      <div id="algolia-stats"></div>
      <div id="algolia-hits"></div>
      <div id="algolia-pagination" class="algolia-pagination"></div>
    </div>

    <span class="popup-btn-close">
      <i class="fa fa-times-circle"></i>
    </span>
  </div>
</div>
<script src="//cdn.bootcss.com/instantsearch.js/1.7.1/instantsearch.min.js"></script>
```

### 引入`algolia.swig`

在`search.swig`引入`algolia.swig`：

```
 {% elseif theme.algolia_search %}
  {% include 'search/algolia.swig' %}
```

由于`hexo`的`Template render`跟`swig`部分语法冲突，所以涉及到`swig`的部分代码用图片代替。

注意将上面这段代码插入`endif`之前（如果你需要保留其他搜索接口）。

### 添加触发节点

在要触发搜索的`HTML`节点加入一个`CLASS`名为`popup-trigger`：

```
{% raw %}
{% elseif theme.algolia_search %}
  <a href="#" class="popup-trigger">
    {{ __('menu.search') }}
  </a>
{% endraw %}
```

Tips：请不要将以下代码复制，因为`hexo`的模板引擎会将`{{`和`}}`中的内容当做模板渲染，下面的代码是为了告诉它不要将`content`里的内容当做模板渲染，[详情见此](https://github.com/hexojs/hexo/issues/697)：

```
{% raw %}

	//content

{% endraw %}
```

在这里我们将以上代码加入到`header.swig`的`menu`里面（同样插入`endif`之前）。

注意`header.swig`里面有这样一段代码：

```
{% set hasSearch = theme.swiftype_key || theme.tinysou_Key || config.search %}
```

改为

```
{% set hasSearch = theme.swiftype_key || theme.tinysou_Key || config.search || theme.algolia_search %}
```

### 新建`search.js`

新建`search.js`，注意需要将第一步注册得到的信息写入头部的`CONFIC`对象。

```js
$(document).ready(function() {
    var CONFIG = {
        root: '/',
        algolia: {
            applicationID: 'applicationID',
            apiKey: 'apiKey',
            indexName: 'indexName',
            hits: { "per_page": 10 },
            labels: { "input_placeholder": "Searching...", "hits_empty": "未发现与 「${query}」相关的内容", "hits_stats": "${hits} 条相关条目，使用了 ${time} 毫秒" }
        }
    };
    var algoliaSettings = CONFIG.algolia;
    var isAlgoliaSettingsValid = algoliaSettings.applicationID &&
        algoliaSettings.apiKey &&
        algoliaSettings.indexName;

    if (!isAlgoliaSettingsValid) {
        window.console.error('Algolia Settings are invalid.');
        return;
    }

    var search = instantsearch({
        appId: algoliaSettings.applicationID,
        apiKey: algoliaSettings.apiKey,
        indexName: algoliaSettings.indexName,
        searchFunction: function(helper) {
            var searchInput = $('#algolia-search-input').find('input');

            if (searchInput.val()) {
                helper.search();
            }
        }
    });

    // Registering Widgets
    [
        instantsearch.widgets.searchBox({
            container: '#algolia-search-input',
            placeholder: algoliaSettings.labels.input_placeholder
        }),

        instantsearch.widgets.hits({
            container: '#algolia-hits',
            hitsPerPage: algoliaSettings.hits.per_page || 10,
            templates: {
                item: function(data) {
                    return (
                        '<a href="' + CONFIG.root + data.slug + '" class="algolia-hit-item-link">' +
                        data._highlightResult.title.value +
                        '</a>'
                    );
                },
                empty: function(data) {
                    return (
                        '<div id="algolia-hits-empty">' +
                        algoliaSettings.labels.hits_empty.replace(/\$\{query}/, data.query) +
                        '</div>'
                    );
                }
            },
            cssClasses: {
                item: 'algolia-hit-item'
            }
        }),

        instantsearch.widgets.stats({
            container: '#algolia-stats',
            templates: {
                body: function(data) {
                    var stats = algoliaSettings.labels.hits_stats
                        .replace(/\$\{hits}/, data.nbHits)
                        .replace(/\$\{time}/, data.processingTimeMS);
                    return (
                        stats +
                        '<span class="algolia-powered">' +
                        '  <img src="' + CONFIG.root + 'images/algolia_logo.svg" alt="Algolia" />' +
                        '</span>' +
                        '<hr />'
                    );
                }
            }
        }),

        instantsearch.widgets.pagination({
            container: '#algolia-pagination',
            scrollTo: false,
            showFirstLast: false,
            labels: {
                first: '<i class="fa fa-angle-double-left"></i>',
                last: '<i class="fa fa-angle-double-right"></i>',
                previous: '<i class="fa fa-angle-left"></i>',
                next: '<i class="fa fa-angle-right"></i>'
            },
            cssClasses: {
                root: 'pagination',
                item: 'pagination-item',
                link: 'page-number',
                active: 'current',
                disabled: 'disabled-item'
            }
        })
    ].forEach(search.addWidget, search);

    search.start();

    $('.popup-trigger').on('click', function(e) {
        e.stopPropagation();
        $('body').append('<div class="popoverlay">').css('overflow', 'hidden');
        $('.popoverlay').fadeIn(300);
        $('.popup').fadeIn(300);
        $('#algolia-search-input').find('input').focus();
    });

    $('.popup-btn-close').click(function() {
        $('.popoverlay').fadeOut(300);
        $('.popup').fadeOut(300);
        $('.popoverlay').remove();
        $('body').css('overflow', '');
    });

});
```

### 新建`search.css`

```css
ul.search-result-list {
  padding-left: 0px;
  margin: 0px 5px 0px 8px;
}
p.search-result {
  border-bottom: 1px dashed #ccc;
  padding: 5px 0;
}
a.search-result-title {
  font-weight: bold;
}
a.search-result {
  border-bottom: transparent;
  display: block;
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
}
.search-keyword {
  border-bottom: 1px dashed #4088b8;
  font-weight: bold;
}
#local-search-result {
  height: 90%;
  overflow: auto;
}
.popup {
  display: none;
  position: fixed;
  top: 10%;
  left: 50%;
  width: 700px;
  height: 80%;
  margin-left: -350px;
  padding: 3px 0 0 10px;
  background: #fff;
  color: #333;
  z-index: 9999;
  border-radius: 0px;
}
@media (max-width: 767px) {
  .popup {
    padding: 3px;
    top: 0;
    left: 0;
    margin: 0;
    width: 100%;
    height: 100%;
    border-radius: 0px;
  }
}
.popoverlay {
  display: none;
  position: fixed;
  width: 100%;
  height: 100%;
  top: 0px;
  left: 0px;
  z-index: 2080;
  background-color: rgba(0,0,0,0.3);
}
#local-search-input {
  margin-bottom: 10px;
  width: 50%;
}
.popup-btn-close {
  position: absolute;
  top: 6px;
  right: 14px;
  color: #4ebd79;
  font-size: 14px;
  font-weight: bold;
  text-transform: uppercase;
  cursor: pointer;
}
#no-result {
  position: absolute;
  left: 44%;
  top: 42%;
  color: #ccc;
}
.busuanzi-count:before {
  content: " ";
  float: left;
  width: 260px;
  min-height: 25px;
}
@media (min-width: 768px) and (max-width: 991px) {
  .busuanzi-count {
    width: auto;
  }
  .busuanzi-count:before {
    display: none;
  }
}
@media (max-width: 767px) {
  .busuanzi-count {
    width: auto;
  }
  .busuanzi-count:before {
    display: none;
  }
}
.site-uv,
.site-pv,
.page-pv {
  display: inline-block;
}
.site-uv .busuanzi-value,
.site-pv .busuanzi-value,
.page-pv .busuanzi-value {
  margin: 0 5px;
}
.site-uv {
  margin-right: 10px;
}
.site-uv::after {
  content: "|";
  padding-left: 10px;
}
.algolia-popup {
  overflow: hidden;
  padding: 0;
}
.algolia-popup .popup-btn-close {
  padding-left: 15px;
  border-left: 1px solid #eee;
  top: 10px;
}
.algolia-popup .popup-btn-close .fa {
  color: #999;
  font-size: 18px;
}
.algolia-popup .popup-btn-close:hover .fa {
  color: #222;
}
.algolia-search {
  padding: 10px 15px 5px;
  max-height: 50px;
  background: #f5f5f5;
  border-top-left-radius: 5px;
  border-top-right-radius: 5px;
}
.algolia-search-input-icon {
  display: inline-block;
  width: 20px;
}
.algolia-search-input-icon .fa {
  font-size: 18px;
}
.algolia-search-input {
  display: inline-block;
  width: calc(90% - 20px);
}
.algolia-search-input input {
  padding: 5px 0;
  width: 100%;
  outline: none;
  border: none;
  background: transparent;
}
.algolia-powered {
  float: right;
}
.algolia-powered img {
  display: inline-block;
  height: 18px;
  vertical-align: middle;
}
.algolia-results {
  position: relative;
  overflow: auto;
  padding: 10px 30px;
  height: calc(100% - 50px);
}
.algolia-results hr {
  margin: 10px 0;
}
.algolia-results .highlight {
  font-style: normal;
  margin: 0;
  padding: 0 2px;
  font-size: inherit;
  color: #f00;
}
.algolia-hits {
  margin-top: 20px;
}
.algolia-hit-item {
  margin: 15px 0;
}
.algolia-hit-item-link {
  display: block;
  border-bottom: 1px dashed #ccc;
  transition-duration: 0.2s;
  transition-timing-function: ease-in-out;
  transition-delay: 0s;
}
.algolia-pagination .pagination {
  margin-top: 40px;
  border-top: none;
  padding: 0;
  text-align: center;
}
.algolia-pagination .pagination-item {
  display: inline-block;
}
.algolia-pagination .page-number {
  border-top: none;
}
.algolia-pagination .page-number:hover {
  border-bottom: 1px solid #222;
}
.algolia-pagination .disabled-item {
  visibility: hidden;
}
.fa-search::before{
  color: #999999;
}
```

将上述`search.js`和`search.css`文件引入`footer.swig`文件

```
<link href="{{ url_for(theme.css) }}/search.css" rel="stylesheet" type="text/css" />
<script type="text/javascript" src="{{ url_for(theme.js) }}/search.js"></script>
```

将下面这张图片拷贝到`source`目录的`images`目录下

![](/images/2016/08/algolia_logo.svg)

至此，我们的工作就完成了。

