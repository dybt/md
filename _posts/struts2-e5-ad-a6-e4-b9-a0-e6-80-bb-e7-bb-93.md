---
title: struts2 学习总结
id: 200
categories:
  - struts2
date: 2015-07-26 15:35:01
tags:
	- strus2
	- 学习总结
---

# 
	从昨天开始看李刚的&lt;&lt;轻量级JavaEE&gt;&gt;的深入使用struts2,到现在感觉学的东西有点多,有点乱,有点杂,于是静下心来,整理一下.

<div>
	<span style="font-size:20px;">&nbsp;&nbsp; 深入使用struts2,我看了struts2的类型转换,字段的校验,文件的上传,文件的下载.</span>
</div>

<div>
	&nbsp;
</div>

	<span style="font-size:20px;">&nbsp;&nbsp;首先谈一下类型的转换,在此之前我想先说一下Action类中的属性,action类中的每一个属性都会对应着web应用的值,当你请求action的时候,然后转发到某个视图资源,通过&lt;s:debug /&gt;标签,你会看到,你的action中的每个属性都会存到valueStack中.那么问题就来了,这就牵扯到了类型的转换,valueStack中的对象都是Object类型的所以都要进行类型转换.struts2已经内建了int,double,float,bool,char.boolean以及它们的包装类和Date类型的参数.其余的复合类型就需要自己定义了.</span>

	<span style="font-size:20px;"><span style="line-height: 32px;">假如有一个Action类中定义一个名为user的对象,为User类型,而User类型有name和password两个属性,假如前台给你传过来的对象是一个字符串:user=leilei:helloworld这个时候 你就需要自己定义类型转换器,从字符串中解析出想要的东西,生成相应的User类型的对象.再比如前台跟你传过来的是user.name=&quot;&quot;和use.password=&quot;&#39;这种格式,这是给你传过来的一个user对象,如果你要给别的地方传一个String类型的对象,这个时候你也要进行类型转换,转换成String对象.&nbsp;</span></span>

	<span style="font-size: 20px; line-height: 32px;">自定义的类型转换器,要求实现DefaultTypeConverter类后重写其中的convertvalue方法或者实现StrutsTypeConverter类重写ConvertToString(Map content, Object value) 和convertFromString(Map content, String [] value, Class toType)方法.</span>

<div>
	<span style="font-size: 20px; line-height: 32px;">类型转换器定义好之后,还需要注册类型转换器,注册类型转换器有局部和全局两种.</span>
</div>

<div>
	<span style="font-size: 20px; line-height: 32px;">局部类型转换器:局部类型转换器要写在资源文件里,资源文件的命名规则是ActionName-conversion.properties,文件内容的格式为:对象名=相应的转换类的名字.</span>
</div>

<div>
	<span style="font-size: 20px; line-height: 32px;">全局类型转换器:在xwork-conversion.properties(此文件在class类的加载路径下),文件内容的格式和局部的内容格式是相同的.</span>
</div>

<div>
	<span style="font-size: 20px; line-height: 32px;">如果在Action中用到了集合类,而集合类的泛型没有明确指出,则需要在相应的Action-conversion.properties中配置,List类型的参数格式为Element_类型名=相应的类型定义类</span>
</div>

<div>
	<span style="font-size: 20px; line-height: 32px;">Map类型的参数格式为Key_&lt;MapPropName&gt;=&lt;key_type&gt; Element_&lt;MapPropName&gt;=&lt;key_type&gt;</span>
</div>

<div>
	&nbsp;
</div>

<div>
	<span style="font-size: 20px; line-height: 32px;">如果在类型转换中出现类型转换错误,则会返回一个input字符串,进入input的视图,在input视图中可以通过&lt;s:filederror /&gt;打印出错误信息,如果想更改这个信息的格式可以在资源文件中设置xwork.default.invalid.fieldvalue值.如果想对某一字段转换失败的错误消息作出更改,可在相应的Action的资源文件中配置invalid.fieldvalue.&lt;propName&gt;=&lt;tipMsg&gt;.</span>
</div>

<div>
	&nbsp;
</div>

<div>
	<span style="font-size: 20px; line-height: 32px;">接下来谈一下字段校验,字段校验是指某个字段必须满足某些条件,否则就要转发到input视图,编写校验规则一般要通过配置相应的xml文件实现,文件名为Action名-validation.xml名,validation的格式书写可以分为字段式的校验和非字段式的校验,字段式的校验格式:</span>
</div>

<div>
	<span style="font-size: 20px; line-height: 32px;">&lt;valiators&gt;</span>
</div>

<div>
	<span style="font-size: 20px; line-height: 32px;">&nbsp; &nbsp;&lt;fieldName=&quot;name&quot; &gt;(可以有多个)</span>
</div>

<div>
	<span style="font-size: 20px; line-height: 32px;">&nbsp; &nbsp; &nbsp; &lt;fileld-validator type=&quot;type&quot;&gt;(可以有多个)</span>
</div>

<div>
	<span style="font-size: 20px; line-height: 32px;">&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;&lt;param name=&quot;条件&#39;&gt;条件&lt;/param&gt;(可以有多个)</span>
</div>

<div>
	<span style="font-size: 20px; line-height: 32px;">&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &lt;message key=&quot;国际化提示消息&quot;&gt;如果校验错误,就输出这个信息&lt;/message&gt;(这个是必须的)</span>
</div>

<div>
	<span style="font-size: 20px; line-height: 32px;">&nbsp; &nbsp; &nbsp; &nbsp; &lt;/field-validator&gt;&nbsp;</span>
</div>

<div>
	<span style="font-size: 20px; line-height: 32px;">&nbsp;&nbsp; &nbsp; &nbsp;&lt;field-validator type=&quot;&quot;&gt;</span>
</div>

<div>
	<span style="font-size: 20px; line-height: 32px;">&nbsp;&nbsp; &nbsp; ...........</span>
</div>

<div>
	<span style="font-size: 20px; line-height: 32px;">&nbsp;&nbsp; &nbsp; &nbsp;&lt;/field-validator&gt;</span>
</div>

<div>
	<span style="font-size: 20px; line-height: 32px;">&nbsp; &nbsp; &nbsp; &nbsp;&lt;/fieldName&gt;</span>
</div>

<div>
	<span style="font-size: 20px; line-height: 32px;">&nbsp;&lt;/valiators&gt;</span>
</div>

<div>
	&nbsp;
</div>

<div>
	<span style="font-size: 20px; line-height: 32px;">客户端校验:在&lt;s:form中设置 validate=&quot;true&quot; &gt;但是这样之后这个界面不能直接访问,否则会报错,必须通过其它的界面进行转发.</span>
</div>

<div>
	&nbsp;
</div>

<div>
	<span style="font-size: 20px; line-height: 32px;">除了字段校验风格的校验器还有非字段校验风格的校验器.</span>
</div>

<div>
	<span style="font-size: 20px; line-height: 32px;">&lt;validators&gt;</span>
</div>

<div>
	<span style="font-size: 20px; line-height: 32px;">&nbsp;&nbsp;&lt;validator&nbsp;type=&quot;&quot;&gt;</span>
</div>

<div>
	<span style="font-size: 20px; line-height: 32px;">&nbsp;&nbsp; &nbsp;&lt;param name=&quot;filelName&quot;&gt;字段的名字&lt;/param&gt;</span>
</div>

<div>
	<span style="font-size: 20px; line-height: 32px;">&nbsp;&nbsp; &nbsp;&lt;param name=&quot;参数名&quot;&gt;参数值&lt;/param&gt;</span>
</div>

<div>
	<span style="font-size: 20px; line-height: 32px;">&nbsp; &nbsp;...</span>
</div>

<div>
	<span style="font-size: 20px; line-height: 32px;">&nbsp;&nbsp; &nbsp; &lt;message key=&quot;国际化消息&quot;&gt;..&lt;/message&gt;</span>
</div>

<div>
	<span style="font-size: 20px; line-height: 32px;">&nbsp;&lt;/validator-type&gt;</span>
</div>

<div>
	&nbsp;
</div>

<div>
	<span style="font-size: 20px; line-height: 32px;">短路校验:是指对于一个字段如果有多个校验条件,第一个校验规则不满足的时候会输出消息,阻止后面的校验输出消息.</span>
</div>

<div>
	<span style="font-size: 20px; line-height: 32px;">设置格式是:在&lt;filed-name short-circuit=&quot;true&quot; &gt; 和&lt;validator short-circuit=&quot;true&quot; &gt;</span>
</div>

<div>
	<span style="font-size: 20px; line-height: 32px;">校验文件的搜索规则:</span>
</div>

<div>
	<span style="font-size: 20px; line-height: 32px;">&lt;SuperActonName&gt;-validation.xml</span>
</div>

<div>
	<span style="font-size: 20px; line-height: 32px;">&lt;SuperActionName&gt;-&lt;MethodName&gt;-validation.xml</span>
</div>

<div>
	<span style="font-size: 20px; line-height: 32px;">&lt;ActionName&gt;-validation.xml</span>
</div>

<div>
	<span style="font-size: 20px; line-height: 32px;">&lt;ActionName&gt;-&lt;methodName&gt;-validation.xml</span>
</div>

<div>
	<span style="font-size: 20px; line-height: 32px;">规则是:从上往下搜索,但是下面的会覆盖上面的,所有的xml文件都加载.</span>
</div>

<div>
	<span style="font-size: 20px; line-height: 32px;">除了以上配置xml校验外,还有基于注解的校验,和手动完成输入校验.</span>
</div>

<div>
	<span style="font-size: 20px; line-height: 32px;">手动完成的输入校验,需要重写Action中的validate方法,或者重写validateXxx方法.</span>
</div>

<div>
	&nbsp;
</div>

<div>
	<span style="font-size: 20px; line-height: 32px;">输入校验的步骤:</span>
</div>

<div>
	<span style="font-size: 20px; line-height: 32px;">(1)类型转换器对字符串进行解析,将字符串解析成相应的对象形式</span>
</div>

<div>
	<span style="font-size: 20px; line-height: 32px;">(2)如果在解析过程中出现异常,conversionError拦截器将错误信息封装到FieldError里,然后执行第3步,;如果没有错误,直接执行第3步.</span>
</div>

<div>
	<span style="font-size: 20px; line-height: 32px;">(3)使用struts2配置的校验器进行输入校验</span>
</div>

<div>
	<span style="font-size: 20px; line-height: 32px;">(4)通过反射调用validteXxx方法</span>
</div>

<div>
	[![2015-07-26 23:11:15 的截屏](http://www.dreamleilei.com/wordpress/wp-content/uploads/2015/07/2015-07-26-231115-的截屏-300x82.png)](http://www.dreamleilei.com/wordpress/wp-content/uploads/2015/07/2015-07-26-231115-的截屏.png)
</div>

<div>
	<span style="font-size: 20px; line-height: 32px;">(6)如果以上都没有出现fieldError,调用Action里处理用户请求的方法,如果有fieldError,系统将转入input视图资源.</span>
</div>

<div>
	&nbsp;
</div>

<div>
	<span style="font-size: 20px; line-height: 32px;">利用struts2实现文件的上传</span>
</div>

<div>
	<span style="font-size: 20px; line-height: 32px;">在strust包中,org.apache.struts.default.properties中有这样几行:你会发现这样一行</span>
</div>

<div>
	<span style="font-size: 20px; line-height: 32px;">struts.multipart.parser=jakarta</span>
</div>

<div>
	<span style="font-size: 20px; line-height: 32px;">说明struts2的解析器是用到jakarta</span>
</div>

<div>
	<span style="font-size: 20px; line-height: 32px;">然后你需要额外引入common-io-2.2.jar和commons-fieldupload-1.3.1.jar</span>
</div>

<div>
	&nbsp;
</div>

<div>
	<span style="font-size: 20px; line-height: 32px;">文件上传中用到struts标签库中的一个标签&lt;s:file &gt;</span>
</div>

<div>
	<span style="font-size: 20px; line-height: 32px;">然后需要配置相应的action:</span>
</div>

<div>
	<span style="font-size: 20px; line-height: 32px;">需要额外配置三个参数:</span>
</div>

<div>
	<span style="font-size: 20px; line-height: 32px;">File xxx;</span>
</div>

<div>
	<span style="font-size: 20px; line-height: 32px;">String xxxContentType;</span>
</div>

<div>
	<span style="font-size: 20px; line-height: 32px;">String xxxFileName;</span>
</div>

<div>
	<span style="font-size: 20px; line-height: 32px;">这三个参数由上传的File给指定,xxx代表这File的name,然后在action中配置好存储路径,实现好文件的复制即可.</span>
</div>

<div>
	&nbsp;
</div>

<div>
	<span style="font-size: 20px; line-height: 32px;">实现文件下载:</span>
</div>

<div>
	<span style="font-size: 20px; line-height: 32px;">配置Action:需要一个方法返回类型为InputStream 方法名为getXxxx(),然后实现下载只需要配置相应的action即可.</span>
</div>

<div>
	<span style="font-size: 20px; line-height: 32px;">&lt;action&gt;的配置:</span>
</div>

<div>
	<span style="font-size: 20px; line-height: 32px;">&lt;result type=&quot;stream&quot;&gt;</span>
</div>

<div>
	<span style="font-size: 20px; line-height: 32px;">&nbsp;&nbsp; &nbsp; &lt;param name =&quot;inputName&#39; &gt;xxxx(和上面的返回的InputStream的方法名相对应)&lt;/param&gt;</span>
</div>

<div>
	<span style="font-size: 20px; line-height: 32px;">&nbsp;&nbsp; &lt;param name=&quot;contentType&quot;&gt;&lt;/param&gt;</span>
</div>

<div>
	<span style="font-size: 20px; line-height: 32px;">&nbsp;&nbsp;&lt;param name=&quot;contentDisposition&quot; /&gt;下载的文件名&lt;/param&gt;</span>
</div>

<div>
	<span style="font-size: 20px; line-height: 32px;">&lt;param name=&quot;bufferSize&quot;&gt;下载缓冲区大小&lt;/param&gt;</span>
</div>

<div>
	&nbsp;
</div>

<div>
	&nbsp;
</div>
