# 小程序开发

## 小程序注册

邮箱激活+身份验证+手机验证码+微信扫码认证

先看一下小程序的后台 https://mp.weixin.qq.com/


### 小程序的缺点
*频繁调用 setData及 setData过程中页面跳闪

*组件化支持能力太弱(几乎没有)

*不能使用 less、scss 等预编译器

*request 并发次数限制

### 为什么要使用第三方框架
*只要熟悉vue或react即可快速上手,学习成本低

*一套代码可在多端编译运行(微信,支付宝,h5,RN)  支付宝小程序暂不完善

*组件化开发，完美解决组件隔离，组件嵌套，组件通信等问题

*支持使用第三方 npm 资源 （小程序已经支持）

*使小程序可支持 Promise，解决回调烦恼 （小程序已经支持）

*对小程序本身的优化，如生命周期的补充，性能的优化等等

*支持样式编译器: Scss/Less，模板编译器，代码编译器：Babel/Typescript。（在 0.10.101000 以及之后版本的开发工具中，会默认使用 babel 将开发者 ES6 语法代码转换为三端都能很好支持的 ES5 的代码，帮助开发者解决环境不同所带来的开发问题。）


## 第三方框架对比 wepy mpvue taro
![image](https://user-images.githubusercontent.com/19573429/48775390-df32c300-ed07-11e8-9343-ea053545ce09.png)


## 生命周期

  ### 同为vue规范的mpvue和wepy的生命周期和各种方法不尽相同

<font color=red size=20>wepy</font>

wepy生命周期基本与原生小程序相同,再此基础上糅合了一些vue的特性; 对于WePY中的methods属性，因为与Vue中的使用习惯不一致，非常容易造成误解，这里需要特别强调一下：WePY中的methods属性只能声明页面wxml标签的bind、catch事件，不能声明自定义方法，这与Vue中的用法是不一致的。

```javascript
import wepy from 'wepy';

export default class MyPage extends wepy.page {
// export default class MyComponent extends wepy.component {
    customData = {}  // 自定义数据

    customFunction ()　{}  //自定义方法

    onLoad () {}  // 在Page和Component共用的生命周期函数

    onShow () {}  // 只在Page中存在的页面生命周期函数

    config = {};  // 只在Page实例中存在的配置数据，对应于原生的page.json文件

    data = {};  // 页面所需数据均需在这里声明，可用于模板数据绑定

    components = {};  // 声明页面中所引用的组件，或声明组件中所引用的子组件

    mixins = [];  // 声明页面所引用的Mixin实例

    computed = {};  // 声明计算属性（详见后文介绍）

    watch = {};  // 声明数据watcher（详见后文介绍）

    methods = {};  // 声明页面wxml中标签的事件处理函数。注意，此处只用于声明页面wxml中标签的bind、catch事件，自定义方法需以自定义方法的方式声明

    events = {};  // 声明组件之间的事件处理函数
}

```

<font color=red size=20>wepy</font>

mpvue 除了 Vue 本身的生命周期外，还兼容了小程序生命周期，这部分生命周期钩子的来源于微信小程序的 Page， 除特殊情况外，不建议使用小程序的生命周期 钩子。

![image](https://cn.vuejs.org/images/lifecycle.png)

```
小程序生命周期回调函数

onLoad(Object query)
页面加载时触发。一个页面只会调用一次，可以在 onLoad 的参数中获取打开当前页面路径中的参数。

onShow()
页面显示/切入前台时触发。

onReady()
页面初次渲染完成时触发。一个页面只会调用一次，代表页面已经准备妥当，可以和视图层进行交互。

onHide()
页面隐藏/切入后台时触发。 如 navigateTo 或底部 tab 切换到其他页面，小程序切入后台等。

onUnload()
页面卸载时触发。如redirectTo或navigateBack到其他页面时。
```

<font color=red size=20>taro</font>

taro与react生命周期完全相同



## 腾讯云开发环境部署
https://console.qcloud.com/lav2/dev









