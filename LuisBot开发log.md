## Luis开发Blog

这个blog主要记录开发过程中出现的问题，以及相应的解决方案.

#### 2017.6.22

1. 为什么在luisDialog中重新发起一个dialog并调用这个dialog，
再调用预定义的在这个会话结束调用的函数会导致同一会话被多个handler接受？？

    ==还没搞明白！==
    现在应该算是比较明白了！就是一个context只能由一个wait或者done结束！

2. entity部分缺省的查询可能不太需要，可以把这部分代码给去掉，
但是我还是想把这部分代码给跑明白了。


3. entity recommendation应该用`!`来判断，
这样就可以处理luis返回的entity recommendation为空的情况，
即设定默认的entity recommendation了。

4. mac如何远程控制Windows？？
    
    用chrome的远程控控制，team viewer也不错。

5. 为什么数据库添加元素不成功？？？怎么看debug信息？？
    
    调用的方式不对，也有可能是别的问题。


6. 用VS做开发，一台高性能的Windows笔记本确实是很必要了！


----

#### 2017.6.23

1. SSMS上是可以选择数据库的，操作不成功，总是提示找不到table

    这是是因为没切换到对应的database中。我想做的是更改mydatabase数据库，
    但是连接使用的是master数据库！驴头不对马嘴，当然出错了！
    
2. sql查询语句中的字符串应该用单引号？？？有待验证！

    是这个样子的！
    

----

#### 2017.6.25

1. sql server的中文支持的问题

    * 所有含中文的column都要用nvarchar,或者ntext，表示Unicode。
    * 在查询的时候要先声明，nvarchar变量，再用这个变量查询！
    ```sql
    declare @s nvarchar(64) set @s=N'%电子%'
    select * from PersonNameIntent where department like @s;
    ```

2. luis返回的entity字符串中会有空白字符，
要先将空白字符先删掉再使用sql查询语句进行查询。

3. 回话结束多个句柄收到信息的问题怎么解决？问杰神。

    ```
    IDialog method execution finished with multiple resume handlers specified through IDialogStack.
    ```
    然而杰神也没遇到过这个异常，这就比较尴尬了！
    
4. await context.PostAsync("a")和context.Wait(this.MessageReceived)一定要成对出现？

    有待验证！和上面一个issue有很强的关联！
    跟for循环之前的wait有关！
    ==一个luis intent中没有wait时可以不用done，一个wait就可以，
    但是在已经有wait的情况下，必须使用done！==
    
    上面的解释还是不对！！！这个wait到底应该怎么弄 ？？？
    一个对话应该由一个wait或者done结束！
    
    
#### 2017.6.26

1. 怎么优化bot？？


2. 在有些方法不是在原始对象上改动的！比如string类的Replace方法。

```javascript
s.Replace(" ", "");
// 上面的方法是无法改变s的！必须使用下面的方法！

s = s.Replace(" ", "");

```

3. datetime的类型应该什么？？

    很迷！！！应该问其他人！应该是builtin.datatime.datetime!

4. 他们都是用QnA maker!

    要赶紧搭一个出来，这两周主要就干这个事情了！



----


####  2017.7.2

1. 多轮对话的问题：

    如何精准地判断intent？？？重新开启一个会话？？用前几个会话和当前会话连起来？？
    
2. 把第一版做好，赶紧发到票圈装个逼！


#### 2017.7.3

1. azure数据库C#使用文档

    [sql server的C#版官方使用文档](https://docs.azure.cn/zh-cn/sql-database/sql-database-connect-query-dotnet)

2. 将bot的地址发到票圈是真的有用，也暴露了我们很多缺点，这就是一个产品的生成过程吧，
或者说这就是我们做事情的节奏吧。

3. 很多问题，其中比较重要的就是空框和对问题的开放性的约束不够，要多加提示，
多加限制，展示效果可以最厚再加，这个是加分项。

4. 总是报错，却又不知道错误原因在哪？？为什么就崩了呢？？？感觉很奇怪，好气哦

    错误是因为luis的问题，luis重新生成key就可以了，呵呵呵！
    软软家的东西不稳定啊！qna maker也是这样！呵呵呵。
    
    **事实证明是因为luis的key只能访问1000次，必须换其他的key才行！**

5. 终于弄明白了Done和Wait方法的区别，一个对话只需要其中的一个。
Wait的话仍留在当前的dialog，Done的话会返回到上一级的dialog。
这个调用forward时，指定子dialog出栈时的操作，基本做法就是保留在
root dialog中，并且等待用户的下一轮输入。

6. 解决了不少bug，但是还是有很多问题，明天争取中午之前改出来一个版本！
能回答大多数问题，同时有更多的提示。

    提示这个还是有问题，暂时先不做了，先把多轮对话搞定。


----

#### 2017.7.4

1. 多轮对话的问题通过子对话的方法解决了一部分。


-----

#### 2017.7.5

1. location有问题，没有知识的情况下返回的东西太多！

2. 把图片放到网上，现在不只有道云笔记这一种方法了，还可以放到github上，
这就方便多了，把图片放到一个repo里面，上传上去，几条命令就完事了，
而且文件名是固定的，是可预测的，这真是太棒了！



----


#### Bot搭建流程总结
1. 个人微软账号与Azure账号：

    使用个人微软账号登陆比赛网站

    使用Azure账号登陆bot framework、Azure、QNA和LUIS网站
    
    在VS中使用个人微软账号登陆
    
    VS在发布到Azure上时选择用Azure账号（注意发布到国际版Azure）

2. 具体流程
    1. 下载Bot Framework的Visual Studio的模板
    直接放置到
    `C:\Users\你的用户名\Documents\Visual Studio 2015\Templates\ProjectTemplates\Visual C#` 下面
    2. 使用NuGet来下载SDK
    右键你的C#项目，选择 `Manage NuGet Packages`.
    在`Browse`的tab页，输入`Microsoft.Bot.Builder`.
    点击"Install"的按钮
    3. 创建一个`Bot Application`的应用
    4. 登陆`luis.ai`创建Luis
        1. 创建`Intent、Entities、Feature、Prebuild domain`等相关属性
        2. 点击`train&test` 测试Luis是否正确
        3. 正确则`publish` 注意`publish`时选择使用给定的`primary key`
        4. 保存dashboard上的Appid和my keys页面的programmatic api key
        5. 将LUIS集成到Bot上：在bot的dialog中新建`Luisdialog.cs`。并将上面的appid和programmatic api key添加到正确位置
        6. 在本地运行bot 测试是否和Luis联通
    5. 将bot发布到Azure上
		1. 在`解决方案资源管理器`中右键单击项目，然后选择`发布`。 确保已选择`Azure 应用服务`，
		然后单击`发布`
		2. 在弹出页面右上角选择添加账户 将Azure账户添加进去。并点击change type，选择web app
		3. 在`资源组`旁边单击`新建`。给资源组命名，然后单击`确定`
        4. 在`资源组`旁边单击`新建`。给资源组命名，然后单击`确定`
        5. 在`应用服务计划`旁边单击`新建`。在`配置应用服务计划`对话框中，使用默认配置
        6. 为 Web 应用命名。单击`创建`开始创建 Azure 资源。记住你的服务器端运行的地址。
    6. 在Bot Framework网站上注册应用
	    1. 登录 Bot Fraework网站，点击Register a Bot注册一个bot。填写相关的信息：
            
            "Name"：你的bot的名字。
            
            "Bot Handle"：其实是你的Bot的id。
            
            "Description"：你的Bot的描述。
            
            下面需要填写你的endpoint，就是你后台服务的地址：https://你的服务器地址/api/messages。服务器地址就是Azure发布后的网址。
	    2. 点击`Create Microsoft App ID and password`，创建App ID和Password（注意，切记把这个app password记下来），点击保存。
	    3. 在右边的`Channels`里头可以看到`Web Chat`，这个网页端的一个channel，已经写好一个frame，点击"Edit"更新。
            生成Web Chat的密钥之后，把密钥复制，点击"I'm done configuring Web Chat"。
    7. 在本地Bot上添加刚刚创建的bot信息
	    1. 在本地bot的Web.config文件里，填上你的BotId，刚才创建的App ID和app password。

3. 最后
  重新发布本地bot 并在bot framework网页上点击test进行测试。

