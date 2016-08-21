title: git 协同开发时一些pull,push,merge下的一些情况
date: 2016-05-27 17:42:14
tags:
---
> 本博文参考了[git](https://git-scm.com/book/zh/v1/Git-%E5%88%86%E6%94%AF-%E5%88%86%E6%94%AF%E7%9A%84%E6%96%B0%E5%BB%BA%E4%B8%8E%E5%90%88%E5%B9%B6)意在说明在使用git进行团队开发时的一些提交状态。

<!-- more -->


#### 一个分支的情况
+ 目前master分支只有user one进行开发，已经push了一次，相应的commit为c0, 分支情况是这样的 ![picture](http://7xncgn.com1.z0.glb.clouddn.com/16-5-27/16529266.jpg)
+ user two加入开发，但是是在同一分支进行开发，他pull了commit为c0的master,并进行开发，本地的情况成了这样 ![picture](http://7xncgn.com1.z0.glb.clouddn.com/16-5-27/25280459.jpg)
+ user one修改了master.txt文件，成了这样，![picture](http://7xncgn.com1.z0.glb.clouddn.com/16-5-27/64199265.jpg)并进行push，相应的commit为C1;并要求user two 同步他的更新
+ 问题来了，此时如果pull了的话，会发生什么，user tow自己修改maste.txt的东西会如何，新增加的文件又会如何？
+ 直接pull，提示错误，git要求我们先commit，不然我们的修改就都没有了。我们先要进行commit，再pull，发生了问题，如下![picture](http://7xncgn.com1.z0.glb.clouddn.com/16-5-27/42100032.jpg)可以看出maste.txt发生了冲突，我们需要进行手工对文件进行修改，还好我们的user-two.txt 还在。

> 所以我们可以看出来当本地分支与远程分支的文件有冲突时，对于同一个文件的冲突，会进行merge并要求我们手工修改；对于本地文件增加则不会有影响


#### 多分支协同开发的情况
+ 一开始只有master分支，它的文件目录是这样的，![picture](http://7xncgn.com1.z0.glb.clouddn.com/16-5-27/42135075.jpg)它对应的提交为C0
+ 新创建两个分支，branch1 and branch2; 目录说明，master文件大家都可以修改，但是branch1文件只有branch1可以修改，branch2.txt只有branch2可以修改
+ branch1开发了自己的东西，目录如下![picture](http://7xncgn.com1.z0.glb.clouddn.com/16-5-27/49029717.jpg)：并进行merge，push到远程，对应的commit为C1
+ branch2也在一开始的情况下进行了开发，注意此时它的基础是C0的master，但是目前master已经到C1了(保证本地的maste是最新的)。branch2目录结果如下, ![picture](http://7xncgn.com1.z0.glb.clouddn.com/16-5-27/97882007.jpg)此时master要合并branch2,maste前前后后的状态如下 ![picture](http://7xncgn.com1.z0.glb.clouddn.com/16-5-27/71271994.jpg)

+ !!!! merge是以本地的分支为基础的，也就是说如果你本地的maste是在C0状态下的，那么就会用co进行merge，而不是以远程的C1，比如看如下，此时它与远程的master是不同步的。我们进行merge时，情况如下![picture](http://7xncgn.com1.z0.glb.clouddn.com/16-5-27/40715574.jpg)

> 可以看出，单在多分支的情况下进行开发时，由于不同的分支的状态的不同，在进行merge时，可能会发生合并冲突，此时需要我们手工进行文件的合并，清理冲突，然后再进行push。