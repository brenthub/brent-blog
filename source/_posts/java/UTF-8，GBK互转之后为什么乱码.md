layout: post
categories:
- java

tags: 
- java

comments: true
title: UTF-8，GBK互转之后为什么乱码
permalink: utf8-gbk-transfer
date: 2016-07-27 09:03:15
---

在开发过程中，我们会遇到UTF-8跟GBK互转的时候，当中文字符为偶数时一般没有问题，但为奇数时互转之后最后一个字会出现乱码，为此，我们来看下到底是为什么。

单元测试类如下：
```java
@Test
public void test2() throws UnsupportedEncodingException{
	String s = "中国字";
	
	s=new String(s.getBytes(),"UTF-8");
	System.out.println("UTF8:"+s);
	print(s.getBytes("UTF-8"));
	
	s=new String(s.getBytes("UTF-8"),"GBK");
	System.out.println("GBK:"+s);
    //GBK编码是偶数位编码，当为奇数时，最后一位标为不识别使用符号?代替对应ASCII代码就是63
	print(s.getBytes("GBK"));
	
	s=new String(s.getBytes("GBK"),"UTF-8");
	System.out.println("UTF8:"+s);
	print(s.getBytes("UTF-8"));
}

public void print(byte[] bs){
	for(byte b:bs){
		System.out.print(b+" ");
	}
	System.out.println("\n");
}
```

打印如下：
![image](https://cloud.githubusercontent.com/assets/10822807/17161061/b0f75756-53dc-11e6-8204-d2340018e89a.png)

###结论

就象测试类注释里写的GBK编码是偶数位编码，当byte数组为奇数时的UTF-8转成GBK时，最后一位标为不识别使用符号?代替对应ASCII代码就是63，再转回UTF-8时就会出现最后一位是乱码了。