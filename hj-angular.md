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
