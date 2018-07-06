## 服务器

### 校验登陆合法性

#### 描述
由游戏服务器向NextJoySdk申请验证

此接口必须签名，参照“接口签名”标题下的签名方法

#### 流程

![avatar](NextJoyGameSdk_files/img/login.png)

#### 接口
http://sdktest.game.nextjoy.com/cp_auth/actoken

请求方式：http post

#### 参数

| 参数名 | type | 是否必须 | 默认值 | 描述 |
| :--- | :--- | :--- | :--- | :--- |
| appid | int | true | 0 | App唯一ID |
| actoken | string | true | NULL | actoken |
| child_id | int | true | 0 | 马甲包ID |
| channel_id | int | true | 0  | 渠道商ID |
| acid | int  | true | 0 | 子渠道ID |
| package_id | int | true | 0 | 包版本号 |
| imei | string | true | NULL | 设备唯一标示IMEI |
| os | int | true | 0 | 客户端 1：Android  2：ios |
| timestamp | int | true | 0 | 时间戳 |
| sign | string | true | NULL | 签名串 |


#### 返回值

```
{
    "status": 200,
    "message": "操作成功",
    "data": {
        "ext": "",
        "uid": "15321521",
    }
}
```

### 创建订单接口

#### 描述
由游戏服务器向NextJoySdk申请订单
此接口必须签名，参照“接口签名”标题下的签名方法

#### 流程

![avatar](NextJoyGameSdk_files/img/pay_order.png)

#### 接口
http://sdktest.game.nextjoy.com/cp_pay/create_order

请求方式：http post

#### 参数

| 参数名 | type | 是否必须 | 默认值 | 描述 |
| :--- | :--- | :--- | :--- | :--- |
| appid | int | true | 0 | App唯一ID |
| actoken | string | true | NULL | token |
| child_id | int | true | 0 | 马甲包ID |
| channel_id | int | true | 0  | 渠道商ID |
| acid | int  | true | 0 | 子渠道ID |
| package_id | int | true | 0 | 包版本号 |
| server_id | string | true | NULL | 区服务ID|
| sdk_ver | string | true | NULL | sdk插件版本号 |
| api_ver | string | false | NULL | 核心库版本号 |
| app_ver | string | false | NULL | app版本号|
| device_name | string | false | NULL | 设备名称 |
| device_os_ver | string | false | NULL | 设备系统版本号 |
| imei | string | true | NULL | 设备唯一标示IMEI |
| os | int | true | 0 | 客户端 1：Android  2：ios |
| product_id | string | true | NULL | 商品ID |
| order_no | string | true | NULL | NextJoy订单号 |
| cp_order_no | string | true | NULL | CP方订单号,长度限制为50 |
| amount | int | true | NULL | 充值金额，单位为分 |
| currency | string | true | NULL | 充值币种，目前只支持"CNY" |
| timestamp | int | true | 0 | 时间戳 |
| sign | string | true | NULL | 签名串 |
| optional | string | false | NULL | 创建订单时的预留参数,需要做urlencode |

#### 返回值

```
{
    "status": 200,
    "message": "操作成功",
    "data": {
        "ext": "",
        "order_no": "P986559359666491392",
        "param": {
            "amount": 1,
            "pay_url": "http://192.168.0.206:8081/pay_order/type"，
            "order_no":"P986559359666491392" ,
            "ch_product_id":"com.nextjoygames.xxx.coins1",      //订单类型为"IOS内购"时返回
            "ios":"1",                                          //订单类型为"IOS内购"时返回
            "payment_type":"10"
        }
    }
}
```


### 支付结果通知接口（商品发放通知）

#### 描述

异步通知，支付成功后SDK将回调该接口，必须验证签名。（参照“接口签名”标题下的签名方法）


#### 接口地址

由游戏开发方决定。

#### 通知方式

HTTP GET

#### 请求参数


| 参数名 | type | 是否必须 | 默认值 | 描述 |
| :--- | :--- | :--- | :--- | :--- |
| appid | int | true | 0 | App唯一ID |
| uid | int | true | 0  | 用户唯一ID |
| server_id | string | true | NULL | 区服务ID|
| order_no | string | true | NULL | NextJoy订单号 |
| cp_order_no | string | true | NULL | CP方订单号,长度限制为50 |
| amount | int | true | NULL | 充值金额，单位为分 |
| currency | string | true | NULL | 充值币种，目前只支持"CNY" |
| product_id | string | true | NULL | 商品ID |
| timestamp | int | true | 0 | 时间戳 |
| sign | string | true | NULL | 签名 |
| optional | string | false | NULL | 创建订单时的预留参数 |


#### 返回值

返回值类型为**string**

> "success" 表示成功，SDK服务器将不再发送通知。
> 
> "failed" 或其他表示失败，SDK服务器将再次发送。

### 接口签名

#### 签名方法说明
客户端SDK与服务器交互的每个接口都需要进行数据签名验证，同时，支付相关回调的接口中，也需要游戏服务器进行验签，这里对签名方法做如下约定：
1. 签名方法为MD5

2. 待签名数据为接口请求中所带的data数据项的所有子项


#### 签名生成步骤

* 1.filterdata为data中数据剔除sign、actoken字段后的所有数据项。
* 2.将filterdata中的所有数据按照键值升序排列，结果记为sortdata。
* 3.将sortdata中的数据按照key+'='+value+'&'的格式拼接，记为preStr
* 4.preStr去掉末尾"&"字符
* 5.取标准MD5(preStr+appSecret)，其中appSecret为平台得到的app加密字符串。

#### 签名示例

appSecret：'b6bc0677a06b493ff6ee797c75334721'

签名前参数列表(Map类型)data：

```
{
    'appid': '1001',
    'child_id': 1000,
    'channel_id': 1,
    'package_id': 1,
    'acid': '1818',
    'imei': 'fghjkl;',
    'os': 1,
    'api_ver': '1.0',
    'app_ver': '1.0',
    'app_ver_code': '12.0',
    't': 1524636970,
    'sdk_ver': '1.0',
    'device_name': 'malei_android',
    'device_os_ver': '123',
    'actoken': 'nAcE5gcRJpsDYypvMq3c0YXDkbpJxqwdzZeSYnLFaaatvFAcX=jia=n4XW28jRJyTHAs',
    'cp_order_no': '1524627000485',
    'amount': '100',
    'currency': 'CNY',
    'payment_type': '100',
    'product_id': 'ios_rech2',
    'server_id': '1.0'
}
```

去掉Map表里的"actoken"字段：del data['actoken']

将data按照key进行排序，生成sortdata

将sortdata中的数据按照key+'='+value+'&'的格式拼接,并去掉最后的"&"，得到preStr

```
preStr = 'acid=1818&amount=100&api_ver=1.0&app_ver=1.0&app_ver_code=12.0&appid=1001&channel_id=1&child_id=1000&cp_order_no=1524627000485&currency=CNY&device_name=malei_android&device_os_ver=123&imei=fghjkl;&os=1&package_id=1&payment_type=100&product_id=ios_rech2&sdk_ver=1.0&server_id=1.0&t=1524636970'
```

取标准MD5得到sign，并添加到参数列表中,data['sign'] = (MD5(preStr+appSecret)).upper()

最终请求的参数列表

```
{
    'appid': '1001',
    'child_id': 1000,
    'channel_id': 1,
    'package_id': 1,
    'acid': '1818',
    'imei': 'fghjkl;',
    'os': 1,
    'api_ver': '1.0',
    'app_ver': '1.0',
    'app_ver_code': '12.0',
    't': 1524636970,
    'sdk_ver': '1.0',
    'device_name': 'malei_android',
    'device_os_ver': '123',
    'actoken': 'nAcE5gcRJpsDYypvMq3c0YXDkbpJxqwdzZeSYnLFaaatvFAcX=jia=n4XW28jRJyTHAs',
    'cp_order_no': '1524627000485',
    'amount': '100',
    'currency': 'CNY',
    'payment_type': '100',
    'product_id': 'ios_rech2',
    'server_id': '1.0',
    'sign': 'D1A0ECA5334525ED2C6BD6EA251A1EEE'
}
```


#### SDK错误码

```
(200, "操作成功")
(17000, "创建订单失败"),
(17001, "创建订单签名验证失败"),
(17002, "当前购买的商品已下架，请购买其他商品"),
(17003, "当前购买的商品价格不对，请购买其他商品"),
(17004, "当前购买的商品配置信息丢失，请购买其他商品"),
(17005, "ios充值处理失败"),
(17006, "支付失败"),
(17007, "订单不存在"),
(17008, "订单已经支付"),
(17009, "订单已经超时，请重新下单支付"),
(17010, "游客不能进行充值"),
(17011, "订单号重复"),
(17012, "支付成功"),
(18000, "token校验失败");
(18005, "获取uploadToken失败"),
(18006, "获取uploadToken请求超时"),
(18007, "获取uploadToken请求校验失败"),
```
