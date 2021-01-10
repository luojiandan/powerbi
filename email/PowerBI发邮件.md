# PowerBI邮件预警视觉对象

- **文档目录**
  - [实现目的](#1实现目的)
  - [暂未实现的功能](#2暂未实现的功能)
  - [技术路线与使用说明](#3技术路线与使用说明)
    - [emailjs注册](#31注册)
    - [创建邮件服务](#32-创建邮件服务add-new-service)
    - [创建邮件模板](33-创建邮件模板)
    - [获取相关参数](34-获取信息)
    - [在PBI中使用](35-在powerbi中使用邮件预警视觉对象)
  - [获取方式](#5获取方式)
  - [更新记录](#6更新记录)
  - [其他](#7其他)





## 1、实现目的

​	在PowerBI中开发一个视觉对象，实现邮件发送功能，既可定时执行，也可添加触发条件，邮件正文内容可以设定模板。

​	**目前即可在桌面端、也可在服务端运行。**



## 2、暂未实现的功能

- **截图并附到邮件中**

  当初设定此功能是希望对整个页面进行截图并添加至邮件内容中一并发送，但由于PowerBI自定义视觉对象使用的是iframe，存在跨域问题，不能获取到父页面数据，故技术上暂不能实现。

  **PS：***截图使用[html2canvas](http://html2canvas.hertzen.com/)库*



## 3、技术路线与使用说明

​	邮件发送采用第三方库：[emailjs](https://www.emailjs.com/)，共提供了4种版本，我们可以根据需要选择，其中免费版每月可发200封，能创建2个邮件模板，在无特殊需求情况下，可选择使用免费版(注册账号后，默认为免费版)。

![img](https://raw.githubusercontent.com/luojiandan/imgs/main/imgs/OE1MB%7D0KRG6XZXJH8Y5PSYT.png)

​	账号注册后，我们还需要创建邮件服务与邮件模板。

### 3.1**注册**

​	只需填写**用户名、邮箱、密码**即可。

![image-20210110124418140](https://raw.githubusercontent.com/luojiandan/imgs/main/imgs/image-20210110124418140.png)

​	登录后主界面如下，上方提示剩余配额200封，点击“Add New Service”创建邮件服务。

![image-20210110124633585](https://raw.githubusercontent.com/luojiandan/imgs/main/imgs/image-20210110124633585.png)

### 3.2 **创建邮件服务（Add New Service）**

根据自身情况选择相关服务，作者以“Gmail”服务为例

![image-20210110124942788](https://raw.githubusercontent.com/luojiandan/imgs/main/imgs/image-20210110124942788.png)

​	点击“Gmail”，弹出窗口，先单击“Connect Account”按钮，链接gmail邮箱进行授权，成功后点击“Create Service”按钮。

<img src="https://gitee.com/luojiandan/imgs/raw/master/image-20210110125208829.png" alt="image-20210110125208829" style="zoom:80%;" />

​	创建成功后，将在面板中显示服务列表，注意“**Service ID**”，这是我们视觉对象需要用到的信息。

![image-20210110125427924](https://gitee.com/luojiandan/imgs/raw/master/image-20210110125427924.png)

 - **QQ邮件服务配置（SMTP服务）**

   ![image-20210110130925188](https://gitee.com/luojiandan/imgs/raw/master/image-20210110130925188.png)

   注意密码填写的是授权码，关于QQ邮箱配置，请点[这里](https://service.mail.qq.com/cgi-bin/help?subtype=1&&id=28&&no=1001256)，163邮箱配置，请点[这里](https://help.mail.163.com/faqDetail.do?code=d7a5dc8471cd0c0e8b4b8f4f8e49998b374173cfe9171305fa1ce630d7f67ac2cda80145a1742516)。

### 3.3 **创建邮件模板**

![image-20210110131048646](https://gitee.com/luojiandan/imgs/raw/master/image-20210110131048646.png)

​	邮件模板是重点，我们可以在这里邮件标题、正文内容、收件人、发件人等信息，所有信息都可以使用[模板变量](https://www.emailjs.com/docs/user-guide/dynamic-variables-templates/)，通过使用模板，可以设计出漂亮的邮件内容页面，能极大方便用户的使用。

![image-20210110131222184](https://gitee.com/luojiandan/imgs/raw/master/image-20210110131222184.png)

### 3.4 获取信息

至此，我们在emailjs网站做完了相关配置，接下来就可以进入PowerBI桌面端使用邮件预警视觉对象，在此之前，我们需要先记录相关信息，分别为：UserID、ServiceID、TemplateID以及模板参数(上图内容)。

**UserID查看：**

![image-20210110132055390](https://gitee.com/luojiandan/imgs/raw/master/image-20210110132055390.png)

**ServiceID查看：**

![image-20210110132159927](https://gitee.com/luojiandan/imgs/raw/master/image-20210110132159927.png)

TemplateID查看：

![image-20210110132247304](https://gitee.com/luojiandan/imgs/raw/master/image-20210110132247304.png)

### 3.5 在PowerBI中使用邮件预警视觉对象

 - 通过文件方式加载视觉对象，界面如下：

   ![image-20210110152959474](https://gitee.com/luojiandan/imgs/raw/master/image-20210110152959474.png)

   **参数设置：**

   ![image-20210110153548572](https://gitee.com/luojiandan/imgs/raw/master/image-20210110153548572.png)

- **定时发送：**通过设置“星期、小时、分钟”这3个参数的组合实现**定时发送**。

- **DAX度量值编写**

  - **条件**

    度量值返回类型为boolean，可以多条件组合使用，例如：

    ```javascript
    conditionFalse = 1>2  //返回false，表示不满足条件
    conditionTrue = 11>2  //返回ture，表示满足条件
    
    //多条件组合，使用&&符号进行连接，表示同时满足这3个条件
    condition = 
        var cond1=1>2
        var cond2=3>2
        var cond3=5>10
    
        return  cond1 && cond2 && cond3  //返回false，表示不满足
    
    //多条件组合，使用||符号进行连接，表示只要满足3个条件中的一个即可触发
    condition = 
        var cond1=1>2
        var cond2=3>2
        var cond3=5>10
    
        return  cond1 || cond2 || cond3  //返回true，表示满足条件
    ```

  - **邮件模板参数**

    邮件模板参数的编写是重点，首先，模板中定义的变量(使用"**{{}}**")都需要进行响应，在这里，我们采用json格式进行管理，例如：

    对应的模板文件：

    ![image-20210110173414976](https://gitee.com/luojiandan/imgs/raw/master/image-20210110173414976.png)

    相关的参数编写，以上模板共有四个参数，分别为：from_name、to_name、to_mail、message。

    ```javascript
    templateParameters = 
        "{
            'from_name': '西安极泰信息科技',
            'message': 'Check this out!',
            'to_name': '测试账号to_name:',
            'to_mail':'luojiandan@163.com,45096732@qq.com'
        }"
    ```

    **注意：**

    - 在PowerBI的DAX编辑器中，参数名称和值使用单引号进行标记。
    - to_mail表示邮件接收者，多个地址可以使用“,"隔开。
    - 若通过变量链接，则同样需要添加单引号，且字符串链接符号**使用“&"**，<u>不能使用”+“号</u>，如图：

    ![image-20210110192753849](https://gitee.com/luojiandan/imgs/raw/master/image-20210110192753849.png)

- **运行过程**

  程序会先判断触发条件是否满足，触发条件包含逻辑条件和时间条件，其中逻辑条件从Condition字段获取，时间条件则从属性设置中获取。

![image-20210110174400616](https://gitee.com/luojiandan/imgs/raw/master/image-20210110174400616.png)

- **运行结果**

  **运行成功**，按照用户模板和参数值生成邮件信息，并发送，收到邮件结果如下：

  ![image-20210110180506734](https://gitee.com/luojiandan/imgs/raw/master/image-20210110180506734.png)

  **运行失败**，则会显示错误代码与原有，比如下图显示templateID错误。

  ![image-20210110180958548](C:\Users\l\AppData\Roaming\Typora\typora-user-images\image-20210110180958548.png)

## 5、获取方式

- **免费版：**

  没有定时监测功能，只执行一次，通过百度云盘进行下载

  链接：https://pan.baidu.com/s/1Smy2ScDDP9HHnmrg7hTjlw 
  提取码：a05u 

- **收费版：**

  收费版与免费版的区别是，收费版将按照用户设置的时间间隔持续运行。

  **价格为¥599元，享有终身免费升级服务，可开票**。



## 6、更新记录

### v1.0.0版本

​	于2021年1月10日，首次发布，完成了以下功能：

- 定时运行设置
- 条件运行设置
- 邮件模板参数动态设置
- 通过DAX度量值进行参数与逻辑条件设置
- ~~截屏并发送[由于技术原因，暂不能实现]~~



## 7、其他

- **问题咨询、建议与留言：**

  [https://github.com/luojiandan/powerbi/issues](https://github.com/luojiandan/powerbi/issues)，请在github中咨询，以便统一回复

- **未来设计实现与钉钉、微信公众号的结合**，有需要者请与作者联系。

- **其他联系方式：**

  微信：luojiandan

  QQ：45096732



