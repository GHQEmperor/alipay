AliPay SDK for Golang


## 帮助

在集成的过程中有遇到问题，欢迎加 QQ 群 564704807 讨论。


## 其它支付

微信支付 [https://github.com/smartwalle/wxpay](https://github.com/smartwalle/wxpay)

PayPal [https://github.com/smartwalle/paypal](https://github.com/smartwalle/paypal)


## 关于各分支（版本)

* v1 - 最老的版本，实现了完整的支付功能，目前已停止更新维护；
* v2 - 在 v1 的基础上进行了一些优化和规范调整，目前已停止更新维护；
* v3 - 支持公钥证书签名和验签，详情可以参考 [https://docs.open.alipay.com/291/105974/](https://docs.open.alipay.com/291/105974/) 和 [https://docs.open.alipay.com/291/105971/](https://docs.open.alipay.com/291/105971/)，为目前主要维护分支；
* master - 和主要维护分支同步；

## v3 版本如何初始化

截止本次更新(2019-08-28)，支付宝的沙箱环境并未支持公钥证书，考虑到大家开发测试方便，所以本分支保留了沙箱环境下对普通公钥的支持，如果是生产环境则只支持公钥证书，不支持普通公钥。

**需要特别注意以下内容**

#### 沙箱环境

初始化函数传递的参数信息和老版本一样 

```
var client, err = alipay.New(appID, aliPublicKey, privateKey, false)
```


#### 生产环境

初始化函数传递的参数信息不再需要 aliPublicKey, 传了也无所谓，会自动过滤掉。

**下面用到的 privateKey 需要特别注意一下，如果是通过“支付宝开发平台开发助手”创建的CSR文件，在 CSR 文件所在的目录下会生成相应的私钥文件，我们需要使用该私钥进行签名，需要注意该私钥是 pkcs8 格式，可以使用“支付宝开发平台开发助手”将其转换为 pkcs1 格式。**

```
var client, err = alipay.New(appID, "", privateKey, true)
```

另外，需要调用以下几个方法加载证书信息，所有证书都是从支付宝创建的应用处下载，参考 [https://docs.open.alipay.com/291/105971/](https://docs.open.alipay.com/291/105971/) 和 [https://docs.open.alipay.com/291/105972/](https://docs.open.alipay.com/291/105972/)

```
client.LoadAppPublicCertFromFile("appCertPublicKey_2017011104995404.crt")
client.LoadAliPayRootCertFromFile("alipayRootCert.crt")
client.LoadAliPayPublicCertFromFile("alipayCertPublicKey_RSA2.crt")
```

## 已实现接口

#### 手机网站支付API

```
中线(-)后面的名称是该接口在 AliPay 结构体中对应的方法名。
```

* **手机网站支付接口**
	
	alipay.trade.wap.pay - **TradeWapPay()**
	
* **电脑网站支付**

	alipay.trade.page.pay - **TradePagePay()**

* **统一收单线下交易查询**
	
	alipay.trade.query - **TradeQuery()**
	
* **统一收单交易支付接口**
	
	alipay.trade.pay - **TradePay()**
	
* **统一收单交易创建接口**

	alipay.trade.create - **TradeCreate()**
	
* **统一收单线下交易预创建**

	alipay.trade.precreate - **TradePreCreate()**
	
* **统一收单交易撤销接口**

	alipay.trade.cancel - **TradeCancel()**
	
* **统一收单交易关闭接口**

	alipay.trade.close - **TradeClose()**

* **统一收单交易退款接口**

	alipay.trade.refund - **TradeRefund()**
	
* **App支付接口**

	alipay.trade.app.pay - **TradeAppPay()**

* **统一收单交易退款查询**

	alipay.trade.fastpay.refund.query - **TradeFastpayRefundQuery()**
	
* **支付宝订单信息同步接口**
	
	alipay.trade.orderinfo.sync - **TradeOrderInfoSync()**

* **单笔转账到支付宝账户接口**

	alipay.fund.trans.toaccount.transfer - **FundTransToAccountTransfer()**
	
* **查询转账订单接口**

	alipay.fund.trans.order.query - **FundTransOrderQuery()**
	
* **资金授权发码接口**

	alipay.fund.auth.order.voucher.create - **FundAuthOrderVoucherCreate()**

* **资金授权操作查询接口**

	alipay.fund.auth.operation.detail.query - **FundAuthOperationDetailQuery()**

* **资金授权撤销接口**
	
	alipay.fund.auth.operation.cancel - **FundAuthOperationCancel()**
	
* **资金授权解冻接口**

	alipay.fund.auth.order.unfreeze - **FundAuthOrderUnfreeze()**

* **资金授权冻结接口**

	alipay.fund.auth.order.freeze - **FundAuthOrderFreeze()**
		
* **线上资金授权冻结接口**
	
	alipay.fund.auth.order.app.freeze - **FundAuthOrderAppFreeze()**
	
* **查询对账单下载地址**
	
	alipay.data.dataservice.bill.downloadurl.query - **BillDownloadURLQuery()**
	
* **身份认证初始化服务**

    alipay.user.certify.open.initialize - **UserCertifyOpenInitialize()**
    
* **身份认证开始认证**

    alipay.user.certify.open.certify - **UserCertifyOpenCertify()**
    
* **身份认证记录查询**

    alipay.user.certify.open.query - **UserCertifyOpenQuery()**
    
* **用户信息授权(网站支付宝登录快速接入)**
    
    生成授权链接 - **PublicAppAuthorize()**
    
* **换取授权访问令牌**

    alipay.system.oauth.token - **SystemOauthToken()**
    
* **支付宝会员授权信息查询接口**
    
    alipay.user.info.share - **UserInfoShare()** 


#### 通知
	
* **通知内容转换及签名验证**
	
	将支付宝的通知内容转换为 Golang 的结构体，并且验证其合法性。
	
## 集成流程

从[支付宝开放平台](https://open.alipay.com/)申请创建相关的应用，使用自己的支付宝账号登录即可。

#### 沙箱环境

支付宝开放平台为每一个应用提供了沙箱环境，供开发人员开发测试使用。

沙箱环境是独立的，每一个应用都会有一个商家账号和买家账号。

#### 应用信息配置

参考[官网文档](https://docs.open.alipay.com/200/105894) 进行应用的配置。

本 SDK 中的签名方法默认为 **RSA2**，采用支付宝提供的 [RSA签名&验签工具](https://docs.open.alipay.com/291/105971) 生成秘钥时，秘钥的格式必须为 **PKCS1**，秘钥长度推荐 **2048**。所以在支付宝管理后台请注意配置 **RSA2(SHA256)密钥**。

生成秘钥对之后，将公钥提供给支付宝（通过支付宝后台上传）对我们请求的数据进行签名验证，我们的代码中将使用私钥对请求数据签名。

请参考 [如何生成 RSA 密钥](https://docs.open.alipay.com/291/105971)。

#### 创建 Wap 支付

``` Golang
var aliPublicKey = "" // 可选，支付宝提供给我们用于签名验证的公钥，通过支付宝管理后台获取
var privateKey = "xxx" // 必须，上一步中使用 RSA签名验签工具 生成的私钥
var client, err = alipay.New(appId, aliPublicKey, privateKey, false)

// 将 key 的验证调整到初始化阶段
if err != nil {
	fmt.Println(err)
	return
}

var p = alipay.TradeWapPay{}
p.NotifyURL = "http://xxx"
p.ReturnURL = "http://xxx"
p.Subject = "标题"
p.OutTradeNo = "传递一个唯一单号"
p.TotalAmount = "10.00"
p.ProductCode = "QUICK_WAP_WAY"

var url, err = client.TradeWapPay(p)
if err != nil {
	fmt.Println(err)
}

var payURL = url.String()
fmt.Println(payURL)
// 这个 payURL 即是用于支付的 URL，可将输出的内容复制，到浏览器中访问该 URL 即可打开支付页面。
```

#### 同步返回验签

支持自动对支付宝返回的数据进行签名验证，详细信息请参考[自行实现验签](https://doc.open.alipay.com/docs/doc.htm?docType=1&articleId=106120).

如果需要开启自动验签，只需要在初始化 AliPay 对象的时候提供 **aliPublicKey** 参数，该参数的值为支付宝管理后台获取到的支付宝公钥，如下：

``` Golang
var client, err = alipay.New(appId, aliPublicKey, privateKey, false)
```

#### Return URL

发起支付的时候，当我们有提供 Return URL 参数，那么支付成功之后，支付宝将会重定向到该 URL，并附带上相关的参数。

```Golang
var p = alipay.TradeWapPay{}
p.ReturnURL = "http://xxx/return"
```

这时候我们需要对支付宝提供的参数进行签名验证，当然，前提是我们在 alipay.New(...) 初始化方法中有正确提供 **支付宝公钥**：


```Golang
var client, err = alipay.New(appId, aliPublicKey, privateKey, false)

http.HandleFunc("/return", func(rep http.ResponseWriter, req *http.Request) {
	req.ParseForm()
	ok, err := client.VerifySign(req.Form)
	fmt.Println(ok, err)
}
```

#### 异步验证支付结果

有支付或者其它动作发生后，支付宝服务器会调用我们提供的 Notify URL，并向其传递会相关的信息。参考[手机网站支付结果异步通知](https://doc.open.alipay.com/docs/doc.htm?spm=a219a.7629140.0.0.XM5C4a&treeId=203&articleId=105286&docType=1)。

我们需要在提供的 Notify URL 服务中获取相关的参数并进行验证:

```Golang

var client, err = alipay.New(appId, aliPublicKey, privateKey, false)
 
http.HandleFunc("/alipay", func(rep http.ResponseWriter, req *http.Request) {
	var noti, _ = client.GetTradeNotification(req)
	if noti != nil {
		fmt.Println("支付成功")
	} else {
		fmt.Println("支付失败")
	}
	alipay.AckNotification(rep) // 确认收到通知消息
})
```

此验证方法适用于支付宝所有情况下发送的 Notify，不管是手机 App 支付还是 Wap 支付。

#### 关于应用私钥 (privateKey)

应用私钥是我们通过工具生成的私钥，调用支付宝接口的时候，我们需要使用该私钥对参数进行签名。

#### 关于应用公钥 (publicKey)

应用公钥是我们通过工具生成的公钥，需要通过支付宝后台上传该公钥，支付宝收到我们请求的时候，会使用该公钥对参数进行签名验证，以确保数据的有效性。

#### 关于支付宝公钥 (aliPublicKey)

支付宝公钥是从支付宝管理后台获取 **(不是我们通过工具生成的公钥)**，该公钥是支付宝提供给我们用于验证支付宝接口返回数据的有效性 (我们需要使用该公钥对支付宝返回的数据进行签名验证)。

#### 关于 alipay.New() 函数中的最后一个参数 isProduction

支付宝提供了用于开发时测试的 sandbox 环境，对接的时候需要注意相关的 app id 和密钥是 sandbox 环境还是 production 环境的。如果是 sandbox 环境，本参数应该传 false，否则为 true。

#### 支持 RSA 签名及验证
默认采用的是 RSA2 签名，不再提供 RSA 的支持

## License
This project is licensed under the MIT License.
