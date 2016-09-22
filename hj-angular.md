title:  汇聚前端angular架构分享
speaker: 汇聚前端组
url: https://github.com/ksky521/nodePPT
transition:slide2
files: /js/demo.js,/css/demo.css
theme:moon
[slide]

#  汇聚前端angular架构分享
## 演讲者：汇聚前端组

[slide]

# 背景
## 汇聚业务的特点

----

1. 任务多,开发时间紧,典型的CRUD场景 {:&.rollIn}
2. 业务多变,迭代快
3. 历史backbone架构代码冗余高
4. 面向用户为运营人员,或其他公司内部代理商,用户人数不到500
5. 汇聚需要的是一个完整的前端解决方案,而不是一个简单的类库

[slide]
###为什么要从backbone迁移到angularjs架构
----

1. 旧架构历史代码冗余严重，可复用化组件较为粗糙;
2. 无法很好的更细粒度的抽象出常用功能。比如多功能的表单控件，表格插件，表单验证，多角色访问视图状态控制;
3. 样式较为呆板，没有像bootstrap这样快速搭建页面布局的样式框架;
4. 使用人数不超过1000级别的规模，对网站的性能要求较低，对快速实现业务功能的要求较高;
     用backbone这种以轻量级著称的框架有些不太符合应用场景，相关功能组件库较少;
5. 数据双向绑定功能较弱，代码较为繁琐，数据与DOM没有很好的分离，大量存在操作dom的代码，历史代码维护性较低;

[slide]
 ##契机
            在实现订单1.0的时候发现，对表单的应用较为繁琐，随着业务量的持续加重，代码可维护性，新需求的加入，对其他功能的干扰
        较为重，导致测试不顺利，开发也倍加提心吊胆，尝试在局部页面单点引入angularjs作为一个工具库使用，并且与后端紧密讨论
        沟通下，采用可配置式的开发方式，以实现订单产品种类持续增加的特性情况下，让前端更稳定，更较少的调整业务代码，而后端人员
        能够自由的配置业务。结果是在代码量变的更简洁的情况下，测试团队测试工作量大大降低，以及使用过程中BUG量，不足之前的一半。并且
        在后期产品又有新的符合配置规则的需求，80%情况下前端无需调整代码。
        在过程开发中，发现angular的指令以及内置的表单校验，有出奇的效果，让之前大量冗余的状态控制，表单校验代码，数据与dom逻辑上
        彻底的解耦。
[slide]
##架构规划路线
* webpack(自动构建打包工具)
* angularjs(前端mvvm)
* angular-bootstrap-ui(成熟的UI组件库)
[slide]
#webpack
**（目前最强大的自动构建工具）**
1. 让我们可以很方便的引用npm上的相关组件
2. 打包机制让样式与JS合并解决在angular架构中路由处理上的麻烦
3. webpack-dev-server极高效的监听及反向代理功能，摆脱了nginx繁琐的配置，让各个项目间proxy配置彼此独立且由自动构建自动运行
        更有利于开发协作
[slide]
#angularjs
1. 强大的双向绑定机制（当然也踩过不少坑）
2. 丰富的组件库
3. 自定义指令功能非常强大，内置指令ng-form表单验证功能效果极佳
4. 事情监听机制完善
5. 纵向切分业务组件，再横向组合，代码逻辑耦合度低
[slide]
#angular-bootstrap-ui
1. 集成bootstrap3样式框架，能快速的搭建出界面且比之前的美观
2. 常用的UI组件基本集成，但需要经过二次封装，以便于统一调整及调用
3. angularJS目前最强大的UI插件库，侧重点在pc端后台业务系统场景

[slide]
#选择angularjs1.X原因
>  我们为什么不使用angular2.0,react,vue,这些呼声正高的框架

[slide]
#个人思考
1. 我们为什么不用react
   react面向移动端为主，相关PC端业务系统的插件较少，只是MVC中的V层，在大型业务系统中，
        要搭配flux redux以及其他插件实现完整的架构功能，需要在架构开发以及组员培训上投入更多的时间。不符合程度3颗星
2. 我们为什么不使用vue   ?
   与angular同源，语法功能集多家所长，侧重点是全方向发展，以轻快著称，PC端业务系统应用较少，与业务系统相关的插件较少。
         不符合程度4颗星
[slide style="background-image:url('/img/bg1.png')"]

## angular的基础架构 

####目录划分   

----
```javascript
+---build-scripts       //自动构建目录
+---dist                //生产模式构建生成目录
+---build                //开发模式构建生成目录
+---node_modules        //npm库
\---src                 //前端目录
    +---assets          //静态资源目录 如:图片,mp3,此类文件
    |   \---images 
    +---common          //项目中公共文件 公共的样式,项目中有共性的模块(以指令或者服务的方式)
    |   +---images
    |   +---plugins 
    |   +---services
    |   \---styles
    +---common-components //git子仓库 多项目间共用 封装常用功能
    |   +---directives    //多项目公共的指令 比如表格,表单,提示控件的封装
    |   +---filters       //多项目公共的过滤器
    |   \---services      //多项目公共的服务 比如:弹窗服务,ibssUtil工具类
    +---common-frame      //git子仓库 项目的界面框架部分抽离 顶部栏,左侧导航栏,登录退出等  
    |   +---build-scripts //子仓库拥有单独的创建功能能够独立运行,或者嵌入到业务项目中 
    +---configs           //项目的全局性配置,比如ajax超时时间,开发时左侧导航栏菜单
    +---views             //angular路由页面
    |---app.js              //单页面入口js
    |---index.html         //单页面入口html
```

[slide]

## angular的基础架构  
####页面自动化路由处理
* 路由功能由angular-ui-router库提供
```javascript
//html部分
 <div ui-view></div>
//注册路由
  app.config(["$stateProvider", function ($stateProvider){       
    $stateProvider     
    .state("home", { //导航用的名字，如<a ui-sref="login">login</a>里的login
        url: '/',    //访问路径 
        template:'<div>模板内容......</div>'
    })      

 }]);
```
* /src/views   下面目录带有controller.js以及index.js会在构建的时候认为是路由页面
* 根据文件路径拼装一个唯一性的state以及相应的url
