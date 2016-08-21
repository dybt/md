---
layout: post
title: "如何运用更聪明的办法进行Core Data数据迁移"
date: 2014-04-04 22:49:25 +0800
comments: true
categories: [iOS, Core Data]
---

这一段时间一个Core Data数据迁移的问题快把我搞死了，上面给的压力也非常大。先说一下来龙去脉吧~

## 第一次犯错

首先以我为主程的一个应用（绝大部分代码是我写的）由于之前数据库设计的不合理导致应用总是无规律可循的崩溃。这个问题一直存在了好几个月，怎么研究都是无果。后来直到看到Stack Overflow上[Core Data: EXC_BAD_ACCESS accessing relationship](http://stackoverflow.com/a/20446844/397718)的帖子（后来还看到这个[Semantic Issue: Property's synthesized getter follows Cocoa naming convention for returning 'owned' objects](http://stackoverflow.com/a/6327448/)）才意识到自己的愚蠢和无知！我忘记了Objective-C的属性名（Core Data中包括属性名和关系名）不能以new打头，否则就违反了Objective-C的[内存管理策略](https://developer.apple.com/library/ios/documentation/Cocoa/Conceptual/MemoryMgmt/Articles/mmRules.html)。

**请记住：尽量不要使用以alloc、new、copy和mutableCopy等打头的属性名、关系名和方法名，如果必须使用请在声明中使用`NS_RETURNS_RETAINED`或`NS_RETURNS_NOT_RETURNED`，否则会导致内存管理问题！！！**

<!-- more -->

## 第二次犯错

既然知道了错了那就改吧！于是就对Core Data的字段进行修改。之前只是大致了解过Core Data数据库迁移的基本知识，之前也做过一些（只不过之前都是轻量级迁移）。直到这次改关系名是个重量级的迁移，所以就以我自己的理解做了个mapping model。结果把`Relationship Mappings`中的`Mapping Name`给搞错了，搞成它自己了~。最终导致应用上架后升级用户大面积崩溃，后果很严重，老板和领导很生气。后来应用直接下架了，据说公司损失严重，还说开发的能力不行，给我制造了不少压力。

## 第三次犯错

既然有知道原因了那就及时改正吧~，更正了`Relationship Mappings`中的`Mapping Name`，然后经过粗略的测试紧急上架。这次走了苹果的加急通道，上架速度果然很快~，但是第二天又有很多人反馈崩溃问题，看了Crashlytics的后台，发现全是iOS 6及以下的设备。最终研究后发现当用户本地数据库数据量比较大的时候老版本升上来很多都会崩溃（性能好一点机器的几率比较小），后来找到原因了，我又犯了一个低级的错误！当时没考虑到很多人数据量比较大，没不清楚Core Data进行重量级迁移的时候需要耗费很久的时间。我把Core Data的自动迁移放到了应用的入口函数，阻碍了入口函数的返回，导致应用启动时间过长直接被系统咔嚓掉了，也有一部分用户由于迁移失败导致数据库数据残缺导致的崩溃。

## 深入的研究

以前买过一本Marcus S. Zarra的书[Core Data (2nd edition): Data Storage and Management for iOS, OS X, and iCloud](http://pragprog.com/book/mzcd2/core-data)，只是看了部分内容而且Core Data数据迁移这一块没怎么看。这次翻来自己看了一下，对一些概念搞清楚了，后来有看到[objc.io](http://objc.io)上的[Custom Core Data Migrations](http://www.objc.io/issue-4/core-data-migration.html)（里面也引用到了一部分[Core Data (2nd edition): Data Storage and Management for iOS, OS X, and iCloud](http://pragprog.com/book/mzcd2/core-data)中的代码）和 [BookMigration](https://github.com/hwaxxer/BookMigration)实例。都讲到了Progressive Migrations，搞的我也想尝试一下手动的数据迁移。于是有搜索了Google、StackOverflow和苹果文档等，最后发现mapping model是可以拆分的，也就是说一次升级可以拆分成多个mapping model(**每个mapping model不能和另外的mapping model有任何关系，逻辑上要是独立的**)，这样可以减少内存开支。在[BookMigration](https://github.com/hwaxxer/BookMigration)中又发现NSMigrationManager有一个migrationProgress属性，用来获取当前迁移进度，感觉真是太棒了，不用一直在那里转菊花了。。。

经过我自己的深入研究（实践）发现`+[NSMappingModel mappingModelFromBundles:forSourceModel:destinationModel:]`不是真正的靠source model和destination model来匹配的，而是通过mapping model与source model还有destination model的entity version hashes来进行对比的，而且它也只返回一个mapping model，如果拆分成多个mapping model的话就无法找到其他的了。而`+[NSManagedObjectModel mergedModelFromBundles:forStoreMetadata:]`也不是找出下一个版本的model，而是如果发现有`+[NSMappingModel mappingModelFromBundles:forSourceModel:destinationModel:]`和当前（source）model匹配的话就会返回那个model。

## 进行改进

后来我想到了一个绝佳的迁移Core Data数据库的方法：如果是轻量级迁移则用轻量级迁移，否则进行重量级迁移的逐步迁移方法，这样就可以用极少的mapping model做大跨度的版本迁移了。摸清了这些就可以对上述的实例进行一些改造了

1. 首先将原来的一个mapping model分成两个（这两个在逻辑上是相互独立的）；
2. 将每个Core Data的model文件按照版本进行1、2、3、4等进行标示（1表示第一个版本，2表示第二个版本……），这样做的目的是方便由source（当前的） model找到其下一个版本的model；  
![version identivers](https://farm4.staticflickr.com/3792/13716379303_bd0a494373_o_d.png) 
3. 通过当前的model找到下一个版本的model；
4. 找到所有的mapping model：遍历bundle中的每一个cdm文件（mapping model的二进制文件）并通过`-[NSMappingModel initWithContentsOfURL:]`生成NSMappingModel对象,将source model与destination model的entity version hashes与mapping model的`sourceEntityVersionHash`和`destinationEntityVersionHash`进行对比。如果mapping model等于或是source model和destination的子集则说明这个mapping model是升级到下一个版本的mapping model
5. 如果找到的所有mapping model的`sourceEntityVersionHash`与`destinationEntityVersionHash`交集等于source model与destination model的entity version hashes则说明这是个重量级的迁移，否则说明出轻量级迁移。

具体方法请参见：[WYZCoreDataMigrationManager](https://github.com/azone/WYZCoreDataMigrationManager)，觉得还是有一些地方需要改进的，只能等后面有机会再改进了。

另外还需要一些值得注意的地方，比如如果之前的Core Data的journal_mode是WAL，迁移的时候也要吧shm文件和wal文件一块迁移，否则可能会导致数据会乱；有时候需要自己定义Migration Policy等，我在这里就不多说了，实际情况还是需有具体问题具体对待。