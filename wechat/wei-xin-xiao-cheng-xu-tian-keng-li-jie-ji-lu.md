---
cover: >-
  https://images.unsplash.com/photo-1611746869696-d09bce200020?crop=entropy&cs=srgb&fm=jpg&ixid=MnwxOTcwMjR8MHwxfHNlYXJjaHwxfHx3ZWNoYXR8ZW58MHx8fHwxNjQ2MDA5OTcw&ixlib=rb-1.2.1&q=85
coverY: 0
---

# 📞 微信小程序：填坑，理解，记录

### 引言

开门见山，这是我用来记录微信小程序的注意点的博客，也希望与大伙互相交流

### 微信小程序开发工具

#### 上服务器坑

当你殚精竭虑的写完微信小程序并为此舒一口气时，你是否以为已经完成这个项目了呢？\
不好意思，你还必须配置服务器的tomcat或者其他的动态服务器驱动，而且更重要的是需要配置**域名，SSL（HTTPS），实名备案**，如果你问为什么我测试的时候就可以，那么麻烦把上面的勾去掉，你就会发现报错了。（早配置，早舒心）👐\


![](<../.gitbook/assets/0 (1) (1) (1)>)

### 页面路由坑

在小程序中所有页面的路由全部由框架进行管理，且框架以栈的形式维护了当前的所有页面\
注意：

* navigateTo, redirectTo 只能打开非 tabBar 页面，可以传递信息。
* switchTab 只能打开 tabBar 页面，**不可以传递信息**，**wx.navigateTo和 wx.redirectTo不允许跳转到 tab 所包含的页面，只能用 wx.switchTab跳转。**
* reLaunch 可以打开任意页面。
* 页面底部的 tabBar 由页面决定，即只要是定义为 tabBar 的页面，底部都有 tabBar。
* 调用页面路由带的参数可以在目标页面的onLoad中获取。

#### URL传递信息

```javascript
 wx.redirectTo({
     url: '../survey/survey?quesnaire_id=' + ques + '&place=' + place + '&district=' + district + '&store_id=' + inputValue + '&manager_name=' + res.data.data.manager_name + '&store=' + res.data.data.store
     })
```

#### URL接受信息

```javascript
onLoad: function (options) {
var that=this
  that.setData({
    place: options.place,
    district: options.district,
    store_id: options.store_id,
    quesnaire_id: options.quesnaire_id,
    manager_name: options.manager_name,
    store: options.store
  })
}
```

#### rpx与px的关系坑

英寸Inch:英寸表示屏幕斜对角线的长度\
像素Pixel:像素是图像的基本采样单位\
分辨率:分辨率是屏幕像素的数量，一般用屏幕宽度的像素点乘以屏幕高度的像素点。分辨率又分为【物理分辨率】和【逻辑分辨率】，值得注意的是**实际工作中设计师常常给的是物理分辨率，程序中用到的是逻辑分辨率**，但是都称为分辨率.【物理分辨率】是硬件所支持的分辨率，【逻辑分辨率】是软件可以达到的分辨率。\
px:Pixel的缩写，就是指像素\
rem:在做移动端适配是最常用的方法就是使用rem作为单位，因为rem是根据html的fontsize去动态计算实际的px的\
rpx:微信对于rem的一种应用的规定，或者说一种设计的方案，官方上规定屏幕宽度为20rem，规定屏幕宽为750rpx,1rem=750/20rpx。如在 iPhone6 上，屏幕宽度为375px，共有750个物理像素，则750rpx = 375px = 750物理像素，1rpx = 0.5px = 1物理像素。

| 设备      | rpx换算px (屏幕宽度/750) | px换算rpx (750/屏幕宽度) |
| ------- | ------------------ | ------------------ |
| iPhone5 | 1rpx = 0.42px      | 1px = 2.34rpx      |
| iPhone6 | 1rpx = 0.5px       | 1px = 2rpx         |

\


![](../.gitbook/assets/2)

![](<../.gitbook/assets/1 (1)>)

### 理解微信小程序渲染过程

![](<../.gitbook/assets/3 (1)>)

WXML 模板和 WXSS 样式工作在渲染层，JS 脚本工作在逻辑层。\
小程序的渲染层和逻辑层分别由2个线程管理：渲染层的界面使用了WebView 进行渲染；逻辑层采用JsCore线程运行JS脚本。一个小程序存在多个界面，所以渲染层存在多个WebView线程，这两个线程的通信会经由微信客户端（下文中也会采用Native来代指微信客户端）做中转，逻辑层发送网络请求也经由Native转发。

#### 渲染层

WXML(WeiXin Markup language) 用于描述页面的结构。

WXS(WeiXin Script) 是小程序的一套脚本语言，结合 WXML，可以构建出页面的结构。

WXSS(WeiXin Style Sheet) 用于描述页面的样式。

组件(Component)是视图的基本组成单元。

#### 逻辑层

逻辑层将数据进行处理后发送给视图层，同时接受视图层的事件反馈。

开发者写的所有代码最终将会打包成一份 JavaScript 文件，并**在小程序启动的时候运行，直到小程序销毁**。这一行为类似 ServiceWorker，所以逻辑层也称之为 App Service。

注意：小程序框架的逻辑层并非运行在浏览器中，因此 JavaScript 在 web 中**一些能力都无法使用**，如 window，document 等。

### 理解小程序的生命周期

![](<../.gitbook/assets/4 (1)>)

### 理解微信小程序的WXML与WXSS如何沟通

首先，我们需要明白你的this.setData设置到哪里去了\
![fig:](../.gitbook/assets/5)\
如图，要理解我们把data设置到哪里去了，这里的data是在页面的data中的数据，此时在JS中调用时使用this.data.data,在WXML只需要直接使用data就可以了，因为WXML是直接读到Page的data的

### 记录wx\_request格式

```javascript
wx.request({
          url: '',		//URL
            data: {
            },
            method: '',  //OPTIONS|GET|POST|HEAD|PUT|DELETE|TRACE|CONNECT
            header: {
                'Content-Type': ''//application/x-www-form-urlencoded|application/json
            },
            success: function (res) {
                },
            fail: function (err) {
                console.log(err);
                wx.showToast({
                    title: '错误',
                    icon: 'none'
                })
            }
        })
```

### 记录微信小程序如何通过WXML保留一位小数



```html
<wxs module="filters" src="../../utils/filter.wxs"></wxs>    //引入wxs

<view> filters.toFix(num)</view>    //将num中的数据保留一位小数
```

```html
var filters = {
  toFix: function (value) {
    return value.toFixed(1)//此处n为保留n位小数
  }
}
module.exports = {
  toFix: filters.toFix
}
```
