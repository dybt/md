title: vs2015下opengl和opencv环境搭建
date: 2016-06-13 14:42:14
tags:
---
> 以下配置是在64位机上配置32位的环境

#### opengl
+ 安装opengl 请[参考](http://www.xiaobao1993.com/31.html) 
+ 主要是要把解压目录下的文件放到对应的文件里面

<!-- more -->

#### opencv
+ 在opencv[官网](http://opencv.org/)下载对应的版本之后，解压到对应的路径，本人的路径为`D:\programming_software\opencv\`
+ 在系统变量中的PATH中加入`D:\programming_software\opencv\build\x64\vc12\bin;D:\programming_software\opencv\build\x86\vc12\bin;`
+ 新建一个"空项目"mainProject后，进行配置，打开`项目---->mainProject属性`
+ 配置`配置属性`
	- `配置属性--->VC++目录-->可执行文件目录`，在可执行文件目录下加`D:\programming_software\opencv\build\x86\vc12\bin;`
	- `配置属性--->VC++目录-->包含目录`， 在包含目录下加`D:\programming_software\opencv\build\include;D:\programming_software\opencv\build\include\opencv;D:\programming_software\opencv\build\include\opencv2`
	- `配置属性--->VC++ 目录--->库目录`， 在库目录下加`D:\programming_software\opencv\build\x86\vc12\lib;`
+ 配置`链接器`，将`D:\programming_software\opencv\build\x86\vc12\lib`下的文件都加入到`链接器-->输入--->附加依赖项`
```
opencv_calib3d2413d.lib
opencv_contrib2413d.lib
opencv_core2413d.lib
opencv_features2d2413d.lib
opencv_flann2413d.lib
opencv_gpu2413d.lib
opencv_highgui2413d.lib
opencv_imgproc2413d.lib
opencv_legacy2413d.lib
opencv_ml2413d.lib
opencv_nonfree2413d.lib
opencv_objdetect2413d.lib
opencv_photo2413d.lib
opencv_stitching2413d.lib
opencv_ts2413d.lib
opencv_video2413d.lib
opencv_videostab2413d.lib
opencv_calib3d2413.lib
opencv_contrib2413.lib
opencv_core2413.lib
opencv_features2d2413.lib
opencv_flann2413.lib
opencv_gpu2413.lib
opencv_highgui2413.lib
opencv_imgproc2413.lib
opencv_legacy2413.lib
opencv_ml2413.lib
opencv_nonfree2413.lib
opencv_objdetect2413.lib
opencv_photo2413.lib
opencv_stitching2413.lib
opencv_ts2413.lib
opencv_video2413.lib
opencv_videostab2413.lib
```

#### 问题解决
+ 对于64位机要使用32位环境，需要在路径里面同时加入64位和32位的bin目录
+ 安装opencv运行时出现msvcp120d.dll丢失问题，[解决](http://www.cnblogs.com/ronaldHU/p/5000275.html)