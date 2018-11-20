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


## 第三方框架对比 wepy(star15,154) mpvue(star14,354) taro(star11,288)
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

    computed = {};  // 声明计算属性

    watch = {};  // 声明数据watcher

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

## request请求
*wepy对wx.request做了接受参数的修改,值得一提的是它提供了针对全局的intercapter拦截器

```javascript
// 原生代码:

wx.request({
    url: 'xxx',
    success: function (data) {
        console.log(data);
    }
});

// WePY 使用方式, 需要开启 Promise 支持，参考开发规范章节
wepy.request('xxxx').then((d) => console.log(d));

// async/await 的使用方式, 需要开启 Promise 和 async/await 支持，参考 WIKI
async function request () {
   let d = await wepy.request('xxxxx');
   console.log(d);
}
```

拦截器
```javascript
import wepy from 'wepy';

export default class extends wepy.app {
    constructor () {
        // this is not allowed before super()
        super();
        // 拦截request请求
        this.intercept('request', {
            // 发出请求时的回调函数
            config (p) {
                // 对所有request请求中的OBJECT参数对象统一附加时间戳属性
                p.timestamp = +new Date();
                console.log('config request: ', p);
                // 必须返回OBJECT参数对象，否则无法发送请求到服务端
                return p;
            },

            // 请求成功后的回调函数
            success (p) {
                // 可以在这里对收到的响应数据对象进行加工处理
                console.log('request success: ', p);
                // 必须返回响应数据对象，否则后续无法对响应数据进行处理
                return p;
            },

            //请求失败后的回调函数
            fail (p) {
                console.log('request fail: ', p);
                // 必须返回响应数据对象，否则后续无法对响应数据进行处理
                return p;
            },

            // 请求完成时的回调函数(请求成功或失败都会被执行)
            complete (p) {
                console.log('request complete: ', p);
            }
        });
    }
}

```

*taro对request进行了二次封装,可以使用Taro.request(OBJECT)发起网络请求，支持 Promise 化使用。

*mpvue没有对request做特殊优化,与原生相同,可以自己根据需要进行封装

## 腾讯云开发环境部署
https://console.qcloud.com/lav2/dev

## 小程序hello world目录

![image](https://user-images.githubusercontent.com/19573429/48777312-3ab37f80-ed0d-11e8-93cd-97a6b4eaf7db.png)


