---
title: Hibernate学习总结(一)
id: 204
categories:
  - hibernate
date: 2015-08-03 03:45:32
tags: hibernate
---

# 
	&nbsp; &nbsp;最近一直在学Hibernate框架,因为比较懒,好久没有写博客了,今天强制要求自己把学过的东西总结一下.

	&nbsp; &nbsp;&nbsp;<span style="font-size:20px;">&nbsp; &nbsp; 首先介绍一下什么是Hibernate,我认为的hibernate是一个ORM框架,是一个对象/关系映射的框架,所为对象关系映射,就是把数据库的各种关系映射成对象,然后对数据库的操作只需要对一个一个的对象操作就行,这种操作的好处是脱离了底层数据库的复杂,只需要对对象操作即可改变数据库.</span>

	<span style="font-size:20px;">&nbsp; &nbsp; &nbsp; &nbsp;然后谈一下映射的方式,关系映射把数据库中的表映射成对象中的一个类,而每一条记录映射成类的一个实例,数据库的字段映射成对象的属性.</span>

	<span style="font-size:20px;">&nbsp; &nbsp; &nbsp; 安装包的下载,因为我用的是idea + maven,所以下载的时候只需要复制ibernate-core的依赖包导入即可,然后如果你用的是jdbc连接数据库,还应该导入相应的jdbc所需要的包,因为还要用到数据源,应该把所用到的数据源的包导入,例如我用的是e3p0的数据源,就依赖导入e3p0-hibernate的包.</span>

	<span style="font-size:20px;">&nbsp;&nbsp; &nbsp; &nbsp; 相应的包导入完成之后,接下来就需要进行hibernate.cfg.xml的配置,hibernate.cfg.xml是hibernate的默认配置文件名,相应的配置格式,你可以参照hibernate-core-XXX.Final.jar的org.hibernate包下的hibernate-configuration3.0.dtd&nbsp;,xml的根元素是hibernate-configuration,然后还有&lt;session-factory&gt;子元素,里面是&lt;property&nbsp;name = &quot;&quot; &gt; &lt; /property&gt;的配置,具体的配置不再详细的陈述,除了配置xml文件外,还可以配置相应的资源文件,即hibernate.cfg.properties文件配置,格式为属性名 值名.</span>

	<span style="font-size:20px;">&nbsp;&nbsp; &nbsp; &nbsp;说完了配置文件,再谈一下如果利用配置文件创建Configuration对象,利用配置文件配置很简单,直接 &nbsp;Configuration conf &nbsp;= new Configuration().configure();后面的configure()方法的参数默认为hibernate.cfg.xml文件,你也可以手动的自己添加配置文件的路径.</span>

	<span style="font-size:20px;">&nbsp; &nbsp; 除了上述方法外,也可以通过new Configuration().addAnnotaedClass(Item.class).addAnnotatedClass()把所有的映射类添加进去,这种添加是默认以hibernate.properties文件为configuration的文件.也可以不配置properties文件,而是通过不停的.setProperty(&quot;name&quot;, &quot;value&#39;)来进行创建.</span>

	<span style="font-size: 20px; line-height: 32px;">&nbsp; &nbsp; &nbsp; &nbsp; Hibernate的持久化对象有3中状态:(1)瞬态 (2)持久化状态 (3) 脱管状态.当你new一个对象的时候,对象是处于瞬态的,这个时候可以被垃圾回收.当调用session.load() 或则save() 或者get(), saveOrUpdate方法的时候,就变成持久化状态,当持久化状态调用delete方法,就成了瞬态的对象,持久化的状态通过evict,close或者clear方法就变成了托管的对象.</span>

	<span style="font-size: 20px; line-height: 32px;">&nbsp;&nbsp; &nbsp; &nbsp; 接下来谈一下Hibernate映射,hibernate映射可以通过xml的方式或者是通过注解的方式.在这里我们谈一下通过用注解的方式.</span>

	<span style="font-size: 20px; line-height: 32px;">&nbsp; &nbsp; &nbsp; &nbsp;要把一个类映射成你一个table,这个时候要用到@Entity映射,然后通过@Table(name = &quot; &quot;)制定表的名称,而类中的属性就会自动的映射成数据库中的字段,但是我们往往要通过@Column(name=&quot; &quot;, length = &quot; &quot;, unique= &quot; &quot;, nullable = )等进行字段的限制.也可以通过@Check(constraints=&quot;&quot;)对整个表进行check约束. 对字段进行映射的时候可以通过@Transient设置不在数据库中生成相应的字段,也可以通过@Formlu(value= &quot;()&quot;)惊醒为字段制定赋值规则,使用此注解也不会在数据库中生成相应的字段.特殊的字段如果是enum类型的话,需要用@Enumerated进行修饰,通过(EnumeType.ORDINAL或者EnumType.STRING)进行设置显示方式,如果是数据库中存放的是大数据例如图片,可以用@Lob注解修饰,用@Basic注解辅助修饰时间类型的话用@Temporal()设置数据库中存储的时间类型.</span>

	<span style="font-size: 20px; line-height: 32px;">&nbsp; &nbsp; &nbsp; &nbsp; 谈完了单个表和字段的映射,再谈一下如何映射主键,一般主键用@Id注解修饰,然后通过GeneratedValue()设置主键的值,GeneratedValue的值可以有GenerationType.*指定,也可以通过自定义的主键生成器指定.</span>

	<span style="font-size: 20px; line-height: 32px;">&nbsp;&nbsp; &nbsp; &nbsp; 如果一张表中的属性是一个集合,那应该怎么映射呢,那就应该通过外键,重新创建一张新表,通过@ElementCollection(targetClass=) @CollectionTable(name = &quot; &quot;, joinColumns={@JoinColumn(name = &quot; &quot;, referenceColumn=&quot; &#39; ... ) })映射,</span>

	<span style="font-size: 20px; line-height: 32px;">&nbsp;&nbsp; &nbsp; 如果集合的数据类型是组件,只需要组件的类用@Embeddable修饰即可,其它的都是一样的.</span>

	<span style="font-size: 20px; line-height: 32px;">如果是List,需要指定@OrderColumn(name = &quot;&quot; )如果是Set不用指定,如果是Map,需要指定@MapKeyClass和@MapKeyColumn().</span>

	<span style="font-size: 20px; line-height: 32px;">&nbsp; &nbsp;如果表中的某个属性是组件怎么呢,所为的组件就是不被持久化的类,这个时候需要在组件的类注解@Embeddable,然后增加@Parent注解.或者在持久化类中用@Embedded &nbsp;@AttributeOverrides(@AttributeOverride(name = &quot; &quot;, column=@Column()),..);进行注解修饰.</span>

	<span style="font-size: 20px; line-height: 32px;">&nbsp;&nbsp; &nbsp;如果组件的属性中有集合的话,就和非组件的集合属性的修饰是一样的.</span>

	&nbsp;

	<span style="font-size: 20px; line-height: 32px;">&nbsp;&nbsp;</span>