## h5页面分享微信、qq等注意事项和配置

#### 背景：
    在运营活动页中，往往涉及到将页面分享出去来进行推广的功能，踩了很多坑，这里总结记录一下，不足之处多多指教

#### 使用前准备：
    要想使用微信的分享，那就只能点击“...”来调起微信分享面板，不能直接由页面行为唤起！所以就需要微信的js-SDK，当然高手这个也可以直接写。

使用：
===

#### 1，绑定域名：
    登录微信公众号平台--> 公众号设置 --> 功能设置 --> 填写js安全域名（设置JS接口安全域名后，公众号开发者可在该域名下调用微信开放的JS接口。）

#### 2，在页面引入js文件
    jweixin-1.2.0.js，目前H5项目里有，也是这么做的

#### 3，配置config
    想使用这个分享的js-SDK的页面，都需要配置才能使用，否则将无法调用（同一个url仅需调用一次），目前是在utils公共方法文件里配置

    ```javascript
    wx.config({
        debug: false, // 开启调试模式,调用的所有api的返回值会在客户端alert出来，若要查看传入的参数，可以在pc端打开，参数信息会通过log打出，仅在pc端时才会打印。移动端会通过弹窗来提示相关信息。如果分享信息配置不正确的话，可以开了看对应报错信息
        appId: data.data.appid, // 必填，公众号的唯一标识
        timestamp: data.data.timestamp, // 必填，生成签名的时间戳
        nonceStr: data.data.noncestr, // 必填，生成签名的随机串
        signature: data.data.signature, // 必填，签名
        jsApiList: [ // 需要使用的JS接口列表,分享默认这几个，如果有其他的功能比如图片上传之类的，需要添加对应api进来
            'checkJsApi',
            'onMenuShareTimeline',
            'onMenuShareAppMessage',
            'onMenuShareQQ',
            'onMenuShareWeibo'
        ]
    });
    ```
#### 4，配置后只需在js文件里引用这个方法

#### 5，分享接口

    在微信公众平台里的js-SDK说明文档里指出：

    目前我们正在使用的 wx.onMenuShareTimeline、wx.onMenuShareAppMessage、wx.onMenuShareQQ、wx.onMenuShareQZone 接口，即将废弃。请尽快迁移使用客户端6.7.2及JSSDK 1.4.0以上版本支持的 wx.updateAppMessageShareData、updateTimelineShareData 接口。

    目前项目正在用的1.2.0的分享接口，部分即将废弃，
                // 分享给好友
                wx.onMenuShareAppMessage(shareConfig.share);
                // 分享到朋友圈
                wx.onMenuShareTimeline(shareConfig.share);
                // 分享给手机QQ
                wx.onMenuShareQQ(shareConfig.share);
    
    1.4.0
        自定义“分享给朋友”及“分享到QQ”按钮的分享内容（1.4.0）
        wx.updateAppMessageShareData

        自定义“分享到朋友圈”及“分享到QQ空间”按钮的分享内容（1.4.0）
        wx.updateTimelineShareData

#### 补充
    一，
        1，目前h5链接在微信里打开，调用微信分享接口时，会分享类似小卡片格式，点击进入指定链接；
        2，还有一种需求是分享一张图片出去，目前的做法是通过html2canvas插件，将dom传入，通过canvas制图，之前因为有图片存在跨域问题，解决方法是把图片链接转为base64格式再分享，
        3，不过直接分享图片到朋友圈，现在只能在接单易app里做到，H5链接发送一个postmessage给rn，但在微信浏览器打开链接，再分享，都是由小卡片格式分享出去的，目前做不到直接分享图片，能做到的记得教教我哈。
    
    二，
        上面提到了调用分享时，需在公众号配置安全域名，所以目前因为test公众号一次只能绑定固定项目，所以配置正确但test环境分享不成功可能有以下两个原因：
        1，test公众号绑定项目不对；
        2，自己没有关注公司的两个公众号（深圳全速家居服务有限公司）（wxid_y8jk6itf22lc21的接口测试号）
