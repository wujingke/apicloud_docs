/*
Title: weiXin
Description: weiXin
*/

<ul id="tab" class="clearfix">
	<li class="active"><a href="#method-content">分享类接口</a></li>
	<li><a href="#evt-content">支付类接口</a></li>
	<li><a href="#const-content">Constant</a></li>
</ul>
<div id="method-content">

<div class="outline">
[registerApp](#a1)

[sendRequest](#a2)

[auth](#a3)

[cancelAuth](#a4)

[getUserInfo](#a5)

[refreshToken](#a6)
</div>

#**概述**

weiXin封装了微信开放平台的SDK，使用此模块可轻松实现分享消息到微信客户端的功能

**使用此模块之前需先配置config文件的Feature，方法如下**

	名称：weiXin
	参数：urlScheme、apiKey、apiSecret
	描述：配置微信专用的URL Scheme，使得本应用可以启动微信客户端，并与之交换数据，同时可以从微信客户端返回到本应用。urlScheme为必须配置字段，apiKey和apiSecret为选择配置字段
配置示例:
```js
  <feature name="weiXin">
       <param name="urlScheme" value="wxd0d84bbf23b4a0e4"/>
        <param name="apiKey" value="wxd0d84bbf23b4a0e4"/>
        <param name="apiSecret" value="a354f72aa1b4c2b8eaad137ac81434cd"/>
  </feature>
```
字段描述:

		1.param-urlScheme：声明此字段为URL Scheme类型
		2.param-apiKey：通过微信开放平台申请的key，调用分享功能和支付功能使用到的参数
		3.param-apiSecret 通过微信开放平台申请的secret，调用支付功能使用到的参数

#**registerApp**<div id="a1"></div>

注册应用

registerApp({params},callback(ret, err))

##params

key：

- 类型：字符串
- 默认值：无
- 描述：微信开放平台获取的key，可为空，若为空则从当前widget内config文件读取

secret：

- 类型：字符串
- 默认值：无
- 描述：微信开放平台获的secret,可为空,若为空则从当前widget内config文件读取

##callback(ret, err)

ret：

- 类型：JSON对象

内部字段：

```js
{
	status:true           //操作成功状态值
}
```

err：

- 类型：JSON对象

内部字段：

```js
{
    code:0    //错误码
    msg:""    //错误描述
}
```

##示例代码

```js
var weiXin = api.require('weiXin');
weiXin.registerApp(
	function(ret,err){
		if (ret.status) {
			api.alert({msg:''+ret.status});
		} else{
			api.alert({msg:err.msg});
		}
	}
);
```

##补充说明

注册应用

##可用性

iOS系统，Android系统

可提供的1.0.0及更高版本


#**sendRequest**<div id="a2"></div>

发表内容

sendRequest({params}, callback(ret, err))

##params

scene：

- 类型：字符串
- 默认值：timeline
- 描述：场景（详见[场景](!Constant)常量），不能为空

contentType：

- 类型：字符串
- 默认值：无
- 描述：发表内容类型（详见[内容类型](!Constant)常量），不能为空

title：

- 类型：字符串
- 默认值：无
- 描述：标题，不能为空。当contentType为text时，该字段将被忽略

description：

- 类型：字符串
- 默认值：无
- 描述：内容，不能为空

thumbUrl：

- 类型：字符串
- 默认值：无
- 描述：缩略图本地url地址，不能为空

contentUrl：

- 类型：字符串
- 默认值：无
- 描述：内容url地址，只有contentType为text时可以为空

##callback(ret, err)

ret：

- 类型：JSON对象

内部字段：

```js
{
	status:true		//操作成功状态值
}
```

err：

- 类型：JSON对象

内部字段：

```js
{
    code:0       //错误码（详见错误码常量）
    msg: ""      //错误描述
}
```

##示例代码

```js
//内容类型为music或video时，内容地址应为流媒体地址;
var weiXin = api.require('weiXin');
weiXin.sendRequest({
	scene: 'timeline',
	contentType:'music',
	title:'测试用标题',
	description: '测试用内容',
	thumbUrl: 'fs://a.png',
	contentUrl: 'http://117.78.5.26/files/shijian.mp3'
},function(ret,err){
	if(ret.status){
		api.alert({title:'发表微信',msg: '发表成功', buttons: ['确定']});
	}else{
		api.alert({title: '发表失败',msg: err.msg, buttons: ['确定'] 
		});
	}
});
//内容类型为web_page时，内容地址为常规网址;
var weiXin = api.require('weiXin');
weiXin.sendRequest({
	scene:'timeline',
	contentType:'web_page',
	title:'测试用标题',
	description:'测试用内容',
	thumbUrl:'fs://a.png',
	contentUrl: 'http://www.baidu.com/'
},function(ret,err){
	if(ret.status){
		api.alert({title: '发表微信',msg: '发表成功', buttons: ['确定']});
	} else{
		api.alert({title: '发表失败',msg: err.msg,buttons: ['确定']});
	};
});
```

##补充说明

暂不支持视频流地址；title字段在contentType为text时不起作用

##可用性

iOS系统，Android系统

可提供的1.0.0及更高版本

#**auth**<div id="a3"></div>

登录授权获取token

auth(callback(ret, err))

##callback(ret, err)

ret：

- 类型：JSON对象

内部字段：

```js
{  status:true		//操作成功状态值  token：         //登录成功获取的token值，字符串类型，初次登陆有效期2小时，刷新token后有效期为30天}
```

err：

- 类型：JSON对象

内部字段：

```js
{
    msg:""      //错误描述
}
```

##示例代码

```js
var weiXin = api.require('weiXin');weiXin.auth(function(ret,err){ if(ret.status){    api.alert({msg: ret.token}); }else{    api.alert({msg: err.msg}); }});
```

##补充说明

调用此接口前应确保调用过一次registerApp接口，此接口需要访问网络，所以需要一段时间才能callBack得到token

##可用性

iOS系统，Android系统
可提供的1.0.2及更高版本

#**cancelAuth**<div id="a4"></div>

取消登录授权，删除本地记录的token

cancelAuth(callback(ret, err))

##callback(ret, err)

ret：

- 类型：JSON对象

内部字段：

```js
{
    status:    		//操作成功状态值，布尔类型
}
```

err：

- 类型：JSON对象

内部字段：

```js
{
    msg:""      //错误描述
}
```

##示例代码

```js
var weiXin = api.require('weiXin');weiXin.cancelAuth(function(ret,err){ if(ret.status){    api.alert({msg: '退出成功'}); }else{    api.alert({msg: err.msg}); }});
```

##补充说明

无

##可用性

iOS系统，Android系统

可提供的1.0.2及更高版本

#**getUserInfo**<div id="a5"></div>

获取用户信息

getUserInfo(callback(ret, err))

##callback(ret, err)

ret：

- 类型：JSON对象

内部字段：

```js
{ 	 status:      //操作成功状态值，布尔类型     openid:	  //普通用户的标识，对当前开发者帐号唯一     nickname:	  //普通用户昵称     sex:	      //普通用户性别，1为男性，2为女性     province:    //普通用户个人资料填写的省份     city:  	  //普通用户个人资料填写的城市     country:	  //国家，如中国为CN     headimgurl:  //用户头像，最后一个数值代表正方形头像大小（有0、46、64、96、132数值可选，0代表640*640正方形头像），//用户没有头像时该项为空     privilege:   //用户特权信息，json数组，如微信沃卡用户为（chinaunicom）     unionid:  	  //用户统一标识。针对一个微信开放平台帐号下的应用，同一用户的unionid是唯一的。    }
```

err：

- 类型：JSON对象

内部字段：

```js
{
    msg:""      //错误描述
}
```

##示例代码

```js
var weiXin = api.require('weiXin');weiXin.getUserInfo(function(ret,err){ if(ret.status){    api.alert({msg: '获取成功'}); }else{    api.alert({msg: err.msg}); }});
```

##补充说明

调用此接口前确保已经登录

##可用性

iOS系统，Android系统

可提供的1.0.2及更高版本

#**refreshToken**<div id="a6"></div>

token过期时调用此接口刷新token

refreshToken(callback(ret, err))

##callback(ret, err)

ret：

- 类型：JSON对象

内部字段：

```js
{
    status:    		//操作成功状态值，布尔类型    token：         //登录成功获取的token值，字符串类型，有效期30天
}
```

err：

- 类型：JSON对象

内部字段：

```js
{
    msg:""      //错误描述
}
```

##示例代码

```js
var weiXin = api.require('weiXin');weiXin.refreshToken(function(ret,err){ if(ret.status){    api.alert({msg: '刷新成功'}); }else{    api.alert({msg: err.msg}); }});
```

##补充说明

调用此接口前，确保已经登录过

##可用性

iOS系统，Android系统

可提供的1.0.2及更高版本
</div>


<div id="evt-content">

<div class="outline">
[getToken](#b1)

[getOrder](#b2)

[payOrder](#b3)
</div>

#**准备工作**

在使用接口之前请先保证持有向微信开放平台申请得到的 appid、appsecret(长度为32 的字符串,用于获取 access_token)、appkey(长度为 128 的字符串,用于支付过程中生 成 app_signature)及 partnerkey(微信公众平台商户模块生成的商户密钥)

#**getToken**<div id="b1"></div>

获取支付token

getToken({params}, callback(ret, err))

##params

key：

- 类型：字符串
- 默认值：无
- 描述：微信开放平台获取的key，可为空，若为空则使用registerApp接口传入的key，若registerApp接口没有传key参数，则从当前widget内config文件读取

secret：

- 类型：字符串
- 默认值：无
- 描述：微信开放平台获的secret,可为空,若为空则使用registerApp接口传入的secret，若registerApp接口没有传secret参数，则从当前widget内config文件读取

##callback(ret, err)

ret：

- 类型：JSON对象

内部字段：

```js
{
    status: 		//操作成功状态值
	token: 			//返回的token值
	expires: 		//token过期时间
}
```

err：

- 类型：JSON对象

内部字段：

```js
{
    msg:""      //错误描述
}
```

##示例代码

```js
var weiXin = api.require('weiXin');
weiXin.getToken({
	appId: 'wx652070b3a10fcd45',
	secret:'00f373c57777e46ba86d461cbcc2fbe8'
},function(ret,err) {
	if (ret.status) {
		var token = ret.token;
		var expires = ret.expires;
	}else{
		api.alert({msg:err.msg});
	}
});
```

##补充说明

获取支付的token

##可用性

iOS系统，Android系统

可提供的1.0.0及更高版本


#**getOrder**<div id="b2"></div>

获取预支付订单

getOrder({params}, callback(ret, err))

##params

token：

- 类型：字符串
- 默认值：无
- 描述：上个接口获取到的token，不能为空

orderInfo：

- 类型：JSON对象
- 默认值：无
- 描述：订单详情（详见[订单详情生成方法](!Constant)），不能为空

##callback(ret, err)

ret：

- 类型：JSON对象

内部字段：

```js
{
    status: 		//操作成功状态值
	orderId: 		//返回的订单号
}
```

err：

- 类型：JSON对象

内部字段：

```js
{
    msg:""      //错误描述
}
```

##示例代码

```js
var weiXin = api.require('weiXin');
weiXin.getOrder({
	token: '',
	orderInfo:''
},function(ret,err) {
    if (ret.status) {
		var orderId = ret.orderId;
	}else{
		api.alert({msg:err.msg});
	}
});
```

##补充说明

生成支付的订单号

##可用性

iOS系统，Android系统

可提供的1.0.0及更高版本


#**payOrder**<div id="b3"></div>

支付订单

payOrder({params}, callback(ret, err))

##params

orderId：

- 类型：字符串
- 默认值：无
- 描述：上个接口生成的订单号，不能为空

partnerId：

- 类型：字符串
- 默认值：无
- 描述：商家和微信合作的id号，审核通过后微信发送到商家邮箱，不能为空

nonceStr：

- 类型：字符串
- 默认值：无
- 描述：随机串，防重发，不能为空

timeStamp：

- 类型：字符串
- 默认值：无
- 描述：时间戳，防重发，不能为空

package：

- 类型：字符串
- 默认值：无
- 描述：商家根据财付通文档填写的数据和签名（详情见[支付注意事项](!Constant)），不能为空

sign：

- 类型：字符串
- 默认值：无
- 描述：商家根据微信开放平台文档对数据做的签名（详情见[支付注意事项](!Constant)），不能为空

key：

- 类型：字符串
- 默认值：无
- 描述：从微信开放平台获取的key，可为空，若为空则使用registerApp接口或者getToken接口传入的key，若registerApp接口和getToken接口都没有传key参数，则从当前widget内config文件读取

secret：

- 类型：字符串
- 默认值：无
- 描述：商家从微信开放平台申请的secret，可为空，若为空则使用registerApp接口或者getToken接口传入的secret，若registerApp接口和getToken接口都没有传secret参数，则从当前widget内config文件读取

##callback(ret, err)

ret：

- 类型：JSON对象

内部字段：

```js
{
    status: 		//操作成功状态值
	result: 		//返回的支付结果
}
```

err：

- 类型：JSON对象

内部字段：

```js
{
    msg:""       //错误描述
}
```

##示例代码

```js
var weiXin = api.require('weiXin');
weiXin.payOrder({
     orderId: '',
     partnerId:'',
     nonceStr:'',
     timeStamp:'',
     package:'',
     sign:''
},function(ret,err) {
     if (ret.status) {
		
     }else{
		api.alert({msg:err.msg});
     }
});
```

##补充说明

支付订单

##可用性

iOS系统，Android系统

可提供的1.0.0及更高版本
</div>

<div id="const-content">

<div class="outline">
[场景](#1)

[内容类型](#2)

[错误码](#3)

[订单详情生成](#4)

[支付注意事项](#5)
</div>

#**场景**<div id="1"></div>

场景

##取值范围：

- session		//会话
- timeline		//朋友圈
- favorite		//收藏

#**内容类型**<div id="2"></div>

内容类型

##取值范围：

- text			    //文本
- image		    //图片
- music		    //音乐
- video		    //视频
- web_page		//网页

#**错误码**<div id="3"></div>

错误类型

##取值范围：

- 0		//没有错误
- 1		//普通错误
- 2		//用户取消
- 3		//发送失败
- 4		//授权拒绝
- 5		//不支持

#**订单详情生成**<div id="4"></div>
订单详情是json格式的字符串，示例如下：

```js
{

	"appid":"wwwwb4f85f3a797777",

	"traceid":"crestxu",

	"noncestr":"111112222233333", "package":"bank_type=WX&body=XXX&fee_type=1&input_charset=GBK&notify_url=http%3a%2 f%2f www.qq.com&out_trade_no=16642817866003386000&partner=1900000109&spbill_create_ip=1 27.0.0.1&total_fee=1&sign=BEEF37AD19575D92E191C1E4B1474CA9", 
	"timestamp":1381405298, "app_signature":"53cca9d47b883bd4a5c85a9300df3da0cb48565c",

	"sign_method":"sha1"
}
```

其中，各字段含义如下：

<table>
    <tr>
        <th>参数</th>
        <th>是否必须</th>
		<th>说明</th>
    </tr>
    <tr>
        <td>appid</td>
        <td>是</td>
        <td>应用唯一标识，在微信开放平台提交应用审核通过后获得</td>
    </tr>
    <tr>
        <td>traceid</td>
        <td>否</td>
        <td>商家对用户的唯一标识，如果用微信sso，此处建议填写授权用户的openid</td>
    </tr>
    <tr>
        <td>noncestr</td>
        <td>是</td>
        <td>32位内的随机串，防重发</td>
    </tr>
    <tr>
        <td>package</td>
        <td>是</td>
        <td>订单详情（具体方法见后文）</td>
    </tr>
	<tr>
        <td>timestamp</td>
        <td>是</td>
        <td>时间戳，为1970念1月1日00：00到请求发起时间的秒数</td>
    <tr>
        <td>app_signature</td>
        <td>是</td>
        <td>签名（具体生成方法见后文）</td>
    </tr>
    <tr>
        <td>Sign_method</td>
        <td>是</td>
        <td>加密方式，默认为sha1</td>
    </tr>
</table>

**package 生成方法:**
1. 对所有传入参数按照字段名的 ASCII 码从小到大排序(字典序)后,使用 URL 键值对的格 式(即 key1=value1&key2=value2...)拼接成字符串 string1;
1. 在 string1 最后拼接上 key=partnerKey 得到 stringSignTemp 字符串, 并对 stringSignTemp 进行 md5 运算,再将得到的字符串所有字符转换为大写,得到 sign 值 signValue；
1. 对 string1 中的所有键值对中的 value 进行 urlencode 转码,按照 a 步骤重新拼接成字符 串,得到 string2。对于 js 前端程序,一定要使用函数 encodeURIComponent 进行 urlencode 编码(注意!进行 urlencode 时要将空格转化为%20 而不是+)；
1. 将 sign=signValue 拼接到 string1 后面得到最终的 package 字符串；

**app_signature 生成方法:**
1. 参与签名的字段包括:appid、appkey、noncer、package、timestamp 以及 traceid；
1. 对所有待签名参数按照字段名的 ASCII 码从小到大排序(字典序)后,使用 URL 键值对的 格式(即 key1=value1&key2=value2...)拼接成字符串 string1。 注意:所有参数名均为小写字符；
1. 对 string1 作签名算法,字段名和字段值都采用原始值,不进行 URL 转义。具体签名算法 为 SHA1；

#**支付注意事项**<div id="5"></div>

调起微信支付 SDK 时,请求参数中 package 需填写为:Sign=WXPay

注意：不能写在客户端,这个过程应都由服务器端完成
