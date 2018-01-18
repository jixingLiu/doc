### nodeJs微信公众号开发
#### 一、微信号的分类 ####
1. 企业号：
 不太适合于个人小型开发，是为企业或组织提供移动应用入口，帮助企业建立与员工、上下游供应链及企业应用间的连接。
2. 订阅号：比较适合于个人、小团队，主要是用于信息传播，帮助管理用户以及和用户互动。比如，撰写文章，咨询传播，消息定制等等。
3. 服务号：企业和组织，提供更强大的业务服务与用户管理能力。比如支付，智能接口。
#### 二、各微信号的比较 ####
1. 订阅号：认证与非认证账号的区别就是，认证账号别人可以直接在添加好友里搜索关键词就能找到你。
2. 订阅号和服务号有三点不同
	+ 出现位置不同；
	+ 单月发送消息数量不同，订阅号可以一天一篇，服务号一个月只能发四篇；
	+ 订阅号没有九大接口和支付功能；
3. 服务号的九大接口：
	+ 语音识别：识别你说的话，翻译成文本内容
	+ 客服接口：可以在你发送过消息的24小时内向你回复消息
	+ Oauth2.0网页授权：通过这个可以拿到用户的信息
	+ 生成带参数的二维码：可以获得一系列带不同参数的二维码，在用户扫描关注后，公众号可以根据参数分析各二维码的效果，这些参数可以自己定制，从而实现更多分析结果，比如，用户从哪儿来的
	+ 获取用户地理位置：可以做导航
	+ 获取用户基本信息：可以根据加密后的用户OpenID，通过一系列的参数交互，最终拿到用户基本信息，包括头像、名称、性别、地区
	+ 获取关注者列表：通过这个接口，可以拿到所有关注者的OpenID，就知道有多少人关注你，是谁在关注你
	+ 用户分组接口：通过分组接口，可以在后台为用户移动、创建、修改分组，比如把你们班级你们团队分成男生一组、女生一组
	+ 上传下载多媒体文件：通过这个接口，公众号可以在需要时在微信服务器上传下载多媒体文件
4. 订阅号有的功能：
	+ 会话界面的自定义菜单
	+ 多客服接口，提供贴心快捷的客户服务
	+ 获取用户地址位置，精确提供服务
	+ 高级群发接口，实现更灵活的群发能力
	+ 用户分组接口，方便管理用户
#### 三、 内网穿透  ####
1. 内网穿透
2. Sunny-Ngrock
3. *localtunnel*
#### 四、配置公众账号  ####
1. [https://mp.weixin.qq.com/wiki?t=resource/res_main&id=mp1421137522](https://mp.weixin.qq.com/wiki?t=resource/res_main&id=mp1421137522 "测试号申请")
> 在基本配置/ 填写服务器配置里进行服务器的配置。

> URL就是开发者用来接收微信消息和事件的接口URL，微信会把这些数据或事件push到这个URL中。

> Token你可以任意填写，用作生成一个签名，配置的这个Token会和接口URL中的Token进行比对，以保证安全。

> EncodingAESKey：开发者自己填或随机生成，用作消息的加密密钥

> 消息加解密方式：自己选

#### 五、验证公众号 ####
微信服务器会对我们配置的接口URL推送过来一个set请求，这个get请求中会带上timestamp、nonce、signature、echostr这四个参数。我们的接口收到这个请求后，需要做的事情就是：
1. 第一步：把token、timestamp、nonce这三个经过字典序排序
2. 第二步：将三个参数字符串拼接成一个字符串，进行sha1加密得到r
3. 第三步：将r 与 signature对比，如果二者相同，表示这个请求来源于微信，我们直接原封不动地返回echostr参数内容，接入验证就成功了。
#### 六、实现加密逻辑 ####
#### 为什么是koa，而不是用express？ ####
用koa框架，代码可以更精简、易懂，对于反反复复的异步交互更适合用这个框架实现。

----------
    'use strict'
var Koa = require('koa');
var sha1 = require('sha1');
var config = {
  wechat: {
    appID: 'wxd26c47e9fef261b8',
    appSecret: '7c80e41cebbec2394dd44b374ae5b590',
    token: 'paian'
  }
}

var app = new Koa();
app.use(function *(next){
  console.log(this.query)

  var token = config.wechat.token;

  var signature = this.query.signature;
  var nonce = this.query.nonce;
  var echostr = this.query.echostr;
  var timestamp = this.query.timestamp;

  var str = [token, timestamp, nonce].sort().join('');
  var sha = sha1(str);

  if(sha === signature){
    this.body = echostr + '';
  } else {
    this.body = 'wrong';
  }
})

app.listen(8000);
console.log('listening: 8000');


----------

等到我们通过node admin/app.js把这个服务启动起来，然后点击提交，即可看到配置成功