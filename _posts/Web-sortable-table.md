title: javascript实现排序表格
date: 2016-06-14 19:36:18
tags: Web

---
### 需求
+ 要求html文件里面只有表格
+ 通过一个util.js引入来实现，并将对应的选择器传入对应的功能函数来实现对表格功能的增强，包括样式，包括排序功能
+ 主要是想通过在控制台实现导入util对象，并操作对应的选择器，就可以增强表格

<!-- more -->

### 例子说明 
+ js代码
```
$(function(){
  window.util.enabledSort('#sortTable');
});
```
+ 以上代码将sortTable这个表格，利用util.enabledSort来实现功能增强

### 代码实现说明
+ 为了实现上述例子的使用方式，首先需要有一个util的对象，可以 使用类似jQuery的实现方式，其核心代码如下
```
(function(window){
  var util = {}
  
  //....实现代码

  window.util = util;
})(window)
```

+ 实现enabledSort方法，因为要考虑多个表格的情况，所以需要使用实例化对象的方式，每个表格对应一个实例化的对象，同时为了防止命名冲突，所以enabledSort应该对应一个实例化方法，核心代码如下

```
(function(window){
  var util = {}
  
  // enabledSort 实例化方式
  (function(util){
    
    var sortable = function(id) {}
    sortable.prototype = {}

    enabledSort = function(id) {
      sortObj = new sortable(id);
      sortObj.init();
    }
    util.enabledSort = enabledSort;
  })(util)

  window.util = util;
})(window)
```

#### 完整代码
+ 使用了jade, sass, ls的语法，并用gulp来实现自动化编译和运行
+ [github](https://github.com/sysuKinthon/ife/tree/master/task_3/task_38)