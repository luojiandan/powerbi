# PowerBI邮件预警视觉对象

- **文档目录**
  - [实现目的](#1实现目的)
  - [暂未实现的功能](#2暂未实现的功能)
  - [技术路线与使用说明](#3技术路线与使用说明)
  - [获取方式](#5获取方式)
  - [更新记录](#6更新记录)



## 1、实现目的

​	在PowerBI中开发一个视觉对象，实现邮件发送功能，既可定时执行，也可添加触发条件，邮件正文内容可以设定模板。



## 2、暂未实现的功能

- **截图并附到邮件中**

  当初设定此功能是希望对整个页面进行截图并添加至邮件内容中一并发送，但由于PowerBI自定义视觉对象使用的是iframe，存在跨域问题，不能获取到父页面数据，故技术上暂不能实现。

  **PS：***截图使用[html2canvas](http://html2canvas.hertzen.com/)库*



## 3、技术路线与使用说明

​	邮件发送采用第三方库：[emailjs](https://www.emailjs.com/)，共提供了4种版本，我们可以根据需要选择，其中免费版每月可发200封，能创建2个邮件模板，在无特殊需求情况下，可选择使用免费版(注册账号后，默认为免费版)。

![img](https://raw.githubusercontent.com/luojiandan/imgs/main/imgs/OE1MB%7D0KRG6XZXJH8Y5PSYT.png)

​	账号注册后，我们还需要创建邮件服务与邮件模板。

- **注册页面**

  只需填写**用户名、邮箱、密码**即可。

![image-20210110124418140](https://raw.githubusercontent.com/luojiandan/imgs/main/imgs/image-20210110124418140.png)

- **主界面**

  登录后主界面如下，上方提示剩余配额200封，点击“Add New Service”创建邮件服务。

![image-20210110124633585](https://raw.githubusercontent.com/luojiandan/imgs/main/imgs/image-20210110124633585.png)

- **创建邮件服务（Add New Service）**

  根据自身情况选择相关服务，作者以“Gmail”服务为例

![image-20210110124942788](https://raw.githubusercontent.com/luojiandan/imgs/main/imgs/image-20210110124942788.png)

​	点击“Gmail”，弹出窗口，先单击“Connect Account”按钮，链接gmail邮箱进行授权，成功后点击“Create Service”按钮。

<img src="https://gitee.com/luojiandan/imgs/raw/master/image-20210110125208829.png" alt="image-20210110125208829" style="zoom:80%;" />

​	创建成功后，将在面板中显示服务列表，注意“**Service ID**”，这是我们视觉对象需要用到的信息。

![image-20210110125427924](https://gitee.com/luojiandan/imgs/raw/master/image-20210110125427924.png)

 - **QQ邮件服务配置（SMTP服务）**

   ![image-20210110130925188](https://gitee.com/luojiandan/imgs/raw/master/image-20210110130925188.png)

   注意密码填写的是授权码，关于QQ邮箱配置，请点[这里](https://service.mail.qq.com/cgi-bin/help?subtype=1&&id=28&&no=1001256)，163邮箱配置，请点[这里](https://help.mail.163.com/faqDetail.do?code=d7a5dc8471cd0c0e8b4b8f4f8e49998b374173cfe9171305fa1ce630d7f67ac2cda80145a1742516)。

- **创建邮件模板**

![image-20210110131048646](https://gitee.com/luojiandan/imgs/raw/master/image-20210110131048646.png)

​	邮件模板是重点，我们可以在这里邮件标题、正文内容、收件人、发件人等信息，所有信息都可以使用[模板变量](https://www.emailjs.com/docs/user-guide/dynamic-variables-templates/)。![image-20210110131222184](https://gitee.com/luojiandan/imgs/raw/master/image-20210110131222184.png)

- 至此，我们在emailjs网站做完了相关配置，接下来就可以进入PowerBI桌面端使用邮件预警视觉对象，在此之前，我们需要先记录相关信息，分别为：UserID、ServiceID、TemplateID以及模板参数(上图内容)。

  **UserID查看：**

  ![image-20210110132055390](https://gitee.com/luojiandan/imgs/raw/master/image-20210110132055390.png)

  **ServiceID查看：**

  ![image-20210110132159927](https://gitee.com/luojiandan/imgs/raw/master/image-20210110132159927.png)

  TemplateID查看：

  ![image-20210110132247304](https://gitee.com/luojiandan/imgs/raw/master/image-20210110132247304.png)

- 

- 3、powerbi示例文件下载



## 5、获取方式

- 费用：599元，终身免费
- 获取方式：扫以下微信号，付款，由客服拉进群
- 开票：



## 6、更新记录



绿夏(454751596) 2021/1/3 22:08:33
目前没有触发邮件的需求，倒是出发钉钉机器人发送指定群到挺好。而且除了条件触发，其实定时触发的也有用。比如固定的日报数据

[emailjs](https://www.npmjs.com/package/emailjs)

https://www.emailjs.com/
[简单5步配置EMailJS](https://developer.51cto.com/art/202005/616427.htm?pc)

