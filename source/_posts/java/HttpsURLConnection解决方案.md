layout: post
categories:
- java

tags: 
- java
- httpClient

comments: true
title: HttpsURLConnectionOldImpl cannot be cast to javax.net.ssl.HttpsURLConnection解决方案
permalink: httpsurlconnection-cast-error
date: 2016-07-26 09:03:15
---

在用httpClient过程中，发现抛出以下异常
HttpsURLConnectionOldImpl cannot be cast to javax.net.ssl.HttpsURLConnection

## 解决方案一
```java
try{
	String handler ="sun.net.www.protocol";
	if (StringUtils.isNotEmpty(System.getProperty("java.protocol.handler.pkgs"))) {
		handler = handler + "|" + System.getProperty("java.protocol.handler.pkgs");
	}
	System.setProperty("java.protocol.handler.pkgs", handler);
} catch (Exception e) {
	e.printStackTrace();
}
URL u = new URL("https://www.baidu.com");
HttpsURLConnection conn = (HttpsURLConnection)u.openConnection();
```

## 解决方案二
```java
URL u = new URL(null, "https://www.baidu.com", new sun.net.www.protocol.https.Handler());
HttpsURLConnection conn = (HttpsURLConnection)u.openConnection();
```

## 解决方法三

```java
URL.setURLStreamHandlerFactory(new URLStreamHandlerFactory() {
	@Override
	public URLStreamHandler createURLStreamHandler(String protocol) {
		try {
			String clsName = "sun.net.www.protocol" + "." + protocol + ".Handler";
			Class cls = null;
			try {
				cls = Class.forName(clsName);
			} catch (ClassNotFoundException e) {
				ClassLoader cl = ClassLoader.getSystemClassLoader();
				if (cl != null) {
					cls = cl.loadClass(clsName);
				}
			}
			if (cls != null) {
				return (URLStreamHandler) cls.newInstance();
			}
		} catch (Exception e) {
			e.printStackTrace();
		}
		return null;
	}
});
URL u = new URL("https://www.baidu.com");
HttpsURLConnection conn = (HttpsURLConnection)u.openConnection();
```

## 原理

源码分析如下：
![image](https://cloud.githubusercontent.com/assets/10822807/17124022/a44a5ee4-531a-11e6-904e-fcdb1da1b542.png)
java.protocol.handler.pkgs 环境变量中，以|号分隔，靠前的位置优先查找，直到找到为止。