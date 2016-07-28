layout: post
categories:
- 软件

tags: 
- PLSQL
- Oracle
- 数据库

comments: true
title: PLSQL使用调优手册
permalink: plsql-use-optimize
date: 2016-07-28 10:03:15
---
PL/SQL Developer是一个集成开发环境，专门面向Oracle数据库存储程序单元的开发，为了提高工作效率和个性化配置，在此列举一些常用的调优配置。

## 运行环境
PL/SQL是以oracle客户端为基础运行的，故运行PL/SQL需要安装Oracle服务端（可不用创建实例）或者Oracle瘦客户端（部分功能将不可用，如表的导入导出）。

### Oracle瘦客户端安装
* 下载瘦客户端（略）
* 将解压之后的瘦客户端地址加入系统环境变量，如D:\Program Files\instantclient_10_2
* 添加新的环境变量：变量名：NLS_LANG 值：AMERICAN_AMERICA.ZHS16GBK

### Oracle服务端安装
* 服务端下载安装（略）
* 安装的时候可不用安装实例
![image](https://cloud.githubusercontent.com/assets/10822807/17198882/4b8cf16c-54ab-11e6-8fc8-8fbfda37a4d2.png)

## 运行
* 当环境准备好之后就可以运行PL\SQL了，推荐使用9.0以上的版本。
* 可以在Database直接填写ip/实例名，如
![image](https://cloud.githubusercontent.com/assets/10822807/17198919/96aff0a4-54ab-11e6-904e-6a5ccc9f3060.png)

## 调优
### 调整布局
登录进去，推荐将其它组件都关闭，只保留Objects组件，这个我们用到最多，其它的都没怎么用到，可以按自己习惯调整。如
![image](https://cloud.githubusercontent.com/assets/10822807/17198935/c532999a-54ab-11e6-9dd6-e4c5eb8eeb4b.png)
再点击菜单Window-》Save Layout 将适合自己的布局保存，如
![image](https://cloud.githubusercontent.com/assets/10822807/17198946/dadc6da2-54ab-11e6-9339-5355c96f451f.png)

### 标记表和视图并排序
点击菜单Tools-》Object Browser Folders，如
![image](https://cloud.githubusercontent.com/assets/10822807/17198955/f124c014-54ab-11e6-8e52-dd533abe6177.png)
将Tables和Views文件夹移到前两位，并加其它颜色以区分
![image](https://cloud.githubusercontent.com/assets/10822807/17198967/ff53d3d2-54ab-11e6-95d3-c2d8c587f7be.png)
调整效果如下：
![image](https://cloud.githubusercontent.com/assets/10822807/17198978/1217b790-54ac-11e6-94b3-6b5d390932f2.png)

### 登录之后默认选中My Objects
默认登录只后，会列出All Objects，在实际运用中，我们只关心自己的对象。
点击菜单Tools-》Object Browser Filters，如
![image](https://cloud.githubusercontent.com/assets/10822807/17198993/2ed707f0-54ac-11e6-95a3-d59795c43d05.png)
选中My Object并勾上Default
![image](https://cloud.githubusercontent.com/assets/10822807/17199004/44130bc8-54ac-11e6-906b-212f88d0b75d.png)
最终效果如下：
![image](https://cloud.githubusercontent.com/assets/10822807/17199016/54f1fac6-54ac-11e6-885a-f433edf66abb.png)

### 记住密码
点击菜单Tools --> Preferences --> Oracle --> Logon History --> Store With Password
![image](https://cloud.githubusercontent.com/assets/10822807/17199039/863d3ed8-54ac-11e6-95a9-30a74d6c9427.png)

### 双击显示表数据
点击菜单Preferences --> User Interface -->Object Browser --> Object Type
![image](https://cloud.githubusercontent.com/assets/10822807/17199060/ab5e0f80-54ac-11e6-90bc-696d373d440c.png)
选中Table 将双击改成查询数据
对于视图也可以做一样的设置

### SQL语句字符全部大写
references --> User Interface --> Editor --> Keyword Case --> Uppercase
![image](https://cloud.githubusercontent.com/assets/10822807/17199070/c923cc44-54ac-11e6-84e7-1119bd0c9bd4.png)

### 自动替换
Preferences --> User Interface --> Editor --> AutoReplace
![image](https://cloud.githubusercontent.com/assets/10822807/17199085/e4b415d6-54ac-11e6-8407-316c5b55b3a6.png)
这样我们就可输入sf空格，工作会将自动替换SELECT * FROM
可以根据习惯定义其它的规则
如s=SELECT等等

### 自定义快捷键
PLSQL Developer里预留了很多键让用户自定义，通常情况下，打开PLSQL Developer后，最经常干的事就是打开SQL Window和Command Window，就给这两个操作定义了快捷键，ALT+S和ALT+ C，这样拿鼠标点三下的事情只需要按一下键。
设置方法：菜单Preferences --> User Interface --> Key Configuration
![image](https://cloud.githubusercontent.com/assets/10822807/17199113/0ec7bb8e-54ad-11e6-9242-2da28a330cca.png)

### 显示行数
Preferences --> Window Types -->SQLＷindow -->show gutter
![image](https://cloud.githubusercontent.com/assets/10822807/17199122/2560b90e-54ad-11e6-9650-0dd92e716448.png)
效果如：
![image](https://cloud.githubusercontent.com/assets/10822807/17199129/3467c74e-54ad-11e6-932c-c4383b2620f7.png)

### 根据光标位置自动选择语句
Preferences --> Window Types -->SQLＷindow -->AutoSelect statement
![image](https://cloud.githubusercontent.com/assets/10822807/17199144/4a1bfb64-54ad-11e6-9a51-6892c06bb718.png)
注意，每条语句后面要加分号。

### 管理你的sql文件
点击菜单Tools-》File Browser Locations，如
![image](https://cloud.githubusercontent.com/assets/10822807/17199157/60a25d74-54ad-11e6-953e-231c40cd5644.png)
定义一个文件保存目录如下图：
![image](https://cloud.githubusercontent.com/assets/10822807/17199160/70101de6-54ad-11e6-93b9-5b73f8008d45.png)
并将普通的SQL文件保存默认保存这个文件下
Preferences --> Files --> Directories --> SQL scripts
![image](https://cloud.githubusercontent.com/assets/10822807/17199170/8479e58c-54ad-11e6-8a04-4f71e253c8b4.png)
使用效果如：
![image](https://cloud.githubusercontent.com/assets/10822807/17199180/94c9e5d6-54ad-11e6-8dc6-b2725005cac1.png)

### 快捷登录
![image](https://cloud.githubusercontent.com/assets/10822807/17199197/b130ead0-54ad-11e6-8845-667938450efe.png)

最终效果：
![image](https://cloud.githubusercontent.com/assets/10822807/17199294/6ed9f8ba-54ae-11e6-8776-1f5e54e5484c.png)


### 字体大小调整
![image](https://cloud.githubusercontent.com/assets/10822807/17199213/cad3732c-54ad-11e6-9231-cbd3c5c33fc3.png)

最终效果：
![image](https://cloud.githubusercontent.com/assets/10822807/17199312/8e70ca3c-54ae-11e6-9ac0-b0ed36aa297d.png)

## 常见问题
### Win7系统下登录弹白框
PL/SQL没有权限所致，找到plsqldev.exe右键->属性->兼容性->以管理员身份运行此程序。如下图：
![image](https://cloud.githubusercontent.com/assets/10822807/17199334/af97f500-54ae-11e6-97c5-5a3b89ca0cf4.png)


