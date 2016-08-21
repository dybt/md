title: gulp 工具的使用
date: 2016-04-28 20:57:52
tags: Web
---

#### Gulp是什么
+ gulp是基于Node.js的前端构建工具，是一个基于流的自动化构建工具，通过gulp的插件可以实现前端代码的编译(sass, jade, livescript)，压缩，测试，图片的压缩包，浏览器自动刷新等；
+ 相关的工具网址：
    - [gulp插件网址](http://gulpjs.com/plugins/)
    - [npm网址](https://www.npmjs.com/)
    - [gulp API](https://github.com/gulpjs/gulp/blob/master/docs/API.md)

<!-- more -->

#### Gulp的小使用
> 以下的使用主要是通过gulp来实现前端jade代码，sass代码，livescript代码的自动编译成相关的html代码，css代码，javascript代码,同时实现文件修改的监察和浏览器的自动刷新

+ 环境要求
    - 安装 node

+ 建立一个文件夹gulp来放代码，然后使用npm初始化;
```
cd gulp
npm init
```
+ 安装相关的插件, package.json如下
```
{
  "name": "gulp",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "author": "",
  "license": "ISC",
  "devDependencies": {
    "browser-sync": "^2.12.5",
    "del": "^2.2.0",
    "gulp-jade": "^1.1.0",
    "gulp-livescript": "^3.0.0",
    "gulp-load-plugins": "^1.2.2",
    "gulp-sass": "^2.3.1",
    "gulp-watch": "^4.3.5",
    "gutil": "^1.6.4",
    "livescript": "^1.4.0"
  }
}
```
+ 安装依赖
```
npm install
```
+ 建立相关目录结构，并于其中放一些代码文件
```
mkdir app
cd app
mkdir src
cd src
mkdir jade
mkdir livescript
mkdir sass
... #自己放一些相关的文件到目录下
```
+ 加载相关的插件(相关的插件使用可以在gulp的插件网站上看)
```
var gulp = require('gulp');
var browserSync = require('browser-sync').create();

var $ = require('gulp-load-plugins')({
    pattern: ['gulp-*', 'del', 'gutil']
});
```
+ 实现jade，sass, livescript的自动编译和监查；在有些task里面使用return是因为gulp是一个基于流的构建工具，使用return 返回流中，否则在些之后的函数将不会执行，比如在task default里面，如果clean没有return的话，那么default里面的函数将不会运行；
```
gulp.task('jade', function() {
  return gulp.src('./app/src/jade/*.jade')
    .pipe($.jade())
    .pipe(gulp.dest('./app/build'))
})

gulp.task('sass', function() {
  return gulp.src('./app/src/sass/*.sass')
    .pipe($.sass().on('error', $.sass.logError))
    .pipe(gulp.dest('./app/build/css'))
})

gulp.task('ls', function() {
  return gulp.src('./app/src/liveScript/*.ls')
    .pipe($.livescript({bare:true}))
    .on('error', $.gutil.log)
    .pipe(gulp.dest('./app/build/js'));
})

gulp.task('watch', function() {
  gulp.watch('./app/src/sass/*.sass', ['sass']);
  gulp.watch('./app/src/jade/*.jade', ['jade']);
  gulp.watch('./app/scr/livescript/*.js', ['ls']);
})

gulp.task('clean', function(cb) {
  return $.del(['./app/build/js', './app/build/css', './app/build/html'], cb);
})

gulp.task('build', ['jade', 'sass', 'ls']);

gulp.task('default', ['clean'], function () {
  gulp.start('build', 'watch');
});
```

+ 使用browser-sync来实现浏览器自动刷新
```
gulp.task('ls-watch', ['ls'], browserSync.reload);
gulp.task('jade-watch', ['jade'], browserSync.reload);
gulp.task('sass-watch', ['sass'], browserSync.reload);

gulp.task('file-watch', function() {
  gulp.watch('./app/src/sass/*.sass', ['sass-watch']);
  gulp.watch('./app/src/jade/*.jade', ['jade-watch']);
  gulp.watch('./app/scr/livescript/*.js', ['ls-watch']);
});

gulp.task('serve', ['build'], function() {
    browserSync.init({
        server: {
            baseDir: "./app/build" //这个目录也是服务器的根目录，也就是可以访问的最高层目录了
        }
    });
    gulp.start('file-watch');
});
```
+ 相关代码托管在 [Github](https://github.com/sysuKinthon/MyWeb/tree/master/gulp)

#### 参考网址
+ http://javascript.ruanyifeng.com/tool/gulp.html
+ https://markpop.github.io/2014/09/17/Gulp%E5%85%A5%E9%97%A8%E6%95%99%E7%A8%8B/
+ https://github.com/gulpjs/gulp/blob/master/docs/API.md
