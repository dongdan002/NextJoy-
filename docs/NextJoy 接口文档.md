
<!-- MarkdownTOC -->

- [NextJoy 接口文档](#nextjoy-%E6%8E%A5%E5%8F%A3%E6%96%87%E6%A1%A3)
    - [文件上传](#%E6%96%87%E4%BB%B6%E4%B8%8A%E4%BC%A0)
        - [获取上传token](#%E8%8E%B7%E5%8F%96%E4%B8%8A%E4%BC%A0token)
            - [描述](#%E6%8F%8F%E8%BF%B0)
            - [接口](#%E6%8E%A5%E5%8F%A3)
            - [参数](#%E5%8F%82%E6%95%B0)
            - [返回值](#%E8%BF%94%E5%9B%9E%E5%80%BC)
        - [批量删除](#%E6%89%B9%E9%87%8F%E5%88%A0%E9%99%A4)
            - [描述](#%E6%8F%8F%E8%BF%B0-1)
            - [接口](#%E6%8E%A5%E5%8F%A3-1)
            - [参数](#%E5%8F%82%E6%95%B0-1)
            - [返回值](#%E8%BF%94%E5%9B%9E%E5%80%BC-1)
        - [获取空间文件列表](#%E8%8E%B7%E5%8F%96%E7%A9%BA%E9%97%B4%E6%96%87%E4%BB%B6%E5%88%97%E8%A1%A8)
            - [描述](#%E6%8F%8F%E8%BF%B0-2)
            - [接口](#%E6%8E%A5%E5%8F%A3-2)
            - [参数](#%E5%8F%82%E6%95%B0-2)
            - [返回值](#%E8%BF%94%E5%9B%9E%E5%80%BC-2)
        - [批量获取空间下文件信息](#%E6%89%B9%E9%87%8F%E8%8E%B7%E5%8F%96%E7%A9%BA%E9%97%B4%E4%B8%8B%E6%96%87%E4%BB%B6%E4%BF%A1%E6%81%AF)
            - [描述](#%E6%8F%8F%E8%BF%B0-3)
            - [接口](#%E6%8E%A5%E5%8F%A3-3)
            - [参数](#%E5%8F%82%E6%95%B0-3)
            - [返回值](#%E8%BF%94%E5%9B%9E%E5%80%BC-3)
        - [刷新存储空间文件](#%E5%88%B7%E6%96%B0%E5%AD%98%E5%82%A8%E7%A9%BA%E9%97%B4%E6%96%87%E4%BB%B6)
            - [描述](#%E6%8F%8F%E8%BF%B0-4)
            - [接口](#%E6%8E%A5%E5%8F%A3-4)
            - [参数](#%E5%8F%82%E6%95%B0-4)
            - [返回值](#%E8%BF%94%E5%9B%9E%E5%80%BC-4)
        - [刷新存储空间文件目录](#%E5%88%B7%E6%96%B0%E5%AD%98%E5%82%A8%E7%A9%BA%E9%97%B4%E6%96%87%E4%BB%B6%E7%9B%AE%E5%BD%95)
            - [描述](#%E6%8F%8F%E8%BF%B0-5)
            - [接口](#%E6%8E%A5%E5%8F%A3-5)
            - [参数](#%E5%8F%82%E6%95%B0-5)
            - [返回值](#%E8%BF%94%E5%9B%9E%E5%80%BC-5)
        - [接口签名](#%E6%8E%A5%E5%8F%A3%E7%AD%BE%E5%90%8D)
            - [签名方法说明](#%E7%AD%BE%E5%90%8D%E6%96%B9%E6%B3%95%E8%AF%B4%E6%98%8E)
            - [签名生成步骤](#%E7%AD%BE%E5%90%8D%E7%94%9F%E6%88%90%E6%AD%A5%E9%AA%A4)
            - [签名示例](#%E7%AD%BE%E5%90%8D%E7%A4%BA%E4%BE%8B)
            - [接口错误码](#%E6%8E%A5%E5%8F%A3%E9%94%99%E8%AF%AF%E7%A0%81)

<!-- /MarkdownTOC -->

# NextJoy 接口文档
## 文件上传

### 获取上传token

#### 描述
由客户端向NextJoySdk请求文件上传token
此接口必须签名，参照“接口签名”标题下的签名方法

#### 接口
http://sdktest.game.nextjoy.com/upload_token/get

请求方式：http post

#### 参数

| 参数名 | type | 是否必须 | 默认值 | 描述 |
| :--- | :--- | :--- | :--- | :--- |
| appid | int | true | 0 | App唯一ID |
| child_id | int | true | 0 | 马甲包ID |
| timestamp | int | true | 0 | 时间戳 |
| sign | string | true | NULL | 签名串 |

#### 返回值

```
{
    "status": 200,
    "message": "操作成功",
    "data": {
        "upload_token": "ynphD3vWSUAcF-87M89UqrRbC_w4N0DyswEdIu8R:1wiOb4GJmP1BAV8gcc284Jpgwo8=:eyJzY29wZSI6ImdhbWUtaGRkeno6LyIsImRlYWRsaW5lIjoxNTI1NzYwNDgzfQ==",
        "bucket_name": "game-hddzz"
    }
}
```


### 批量删除

#### 描述
由客户端向NextJoySdk请求文件批量删除操作
此接口必须签名，参照“接口签名”标题下的签名方法

#### 接口
http://sdktest.game.nextjoy.com/upload_token/batch_del_file

请求方式：http post

#### 参数

| 参数名 | type | 是否必须 | 默认值 | 描述 |
| :--- | :--- | :--- | :--- | :--- |
| appid | int | true | 0 | App唯一ID |
| child_id | int | true | 0 | 马甲包ID |
| keystr | string | true | NULL | key列表,文件上限为500条 |
| timestamp | int | true | 0 | 时间戳 |
| sign | string | true | NULL | 签名串 |

#### 返回值

```
{
    "status": 200,
    "message": "操作成功",
    "data": {
    }
}
```
### 获取空间文件列表

#### 描述
由客户端向NextJoySdk请求获取空间文件列表
此接口必须签名，参照“接口签名”标题下的签名方法

#### 接口
http://sdktest.game.nextjoy.com/upload_token/get_file_list

请求方式：http post

#### 参数

| 参数名 | type | 是否必须 | 默认值 | 描述 |
| :--- | :--- | :--- | :--- | :--- |
| appid | int | true | 0 | App唯一ID |
| child_id | int | true | 0 | 马甲包ID |
| marker | string | true | NULL | 用于分页,第一次调用时为"", 下次调用时使用上次返回的marker|
| prefix | string | true | NULL | 前缀 |
| timestamp | int | true | 0 | 时间戳 |
| sign | string | true | NULL | 签名串 |

#### 返回值

```
{
    "status": 200,
    "message": "操作成功",
    "data": {
        "marker": 用于分页,下次调用时作为参数
        "data": {返回的文件列表数据，上限为1000条}
    }
}
```
### 批量获取空间下文件信息

#### 描述
由客户端向NextJoySdk请求批量获取空间文件列表
此接口必须签名，参照“接口签名”标题下的签名方法

#### 接口
http://sdktest.game.nextjoy.com/upload_token/batch_get_file_info

请求方式：http post

#### 参数

| 参数名 | type | 是否必须 | 默认值 | 描述 |
| :--- | :--- | :--- | :--- | :--- |
| appid | int | true | 0 | App唯一ID |
| child_id | int | true | 0 | 马甲包ID |
| keystr | string | true | NULL |要查询的文件名，多个文件用","隔开|
| timestamp | int | true | 0 | 时间戳 |
| sign | string | true | NULL | 签名串 |

#### 返回值

```
{
    "status": 200,
    "message": "操作成功",
    "data": {
        "data": {返回文件获取结果}
    }
}
```

### 刷新存储空间文件

#### 描述
由客户端向NextJoySdk请求刷新存储空间文件
此接口必须签名，参照“接口签名”标题下的签名方法

#### 接口
http://sdktest.game.nextjoy.com/upload_token/batch_refresh_file

请求方式：http post

#### 参数

| 参数名 | type | 是否必须 | 默认值 | 描述 |
| :--- | :--- | :--- | :--- | :--- |
| appid | int | true | 0 | App唯一ID |
| child_id | int | true | 0 | 马甲包ID |
| urls | string | true | NULL |要刷新的文件路径，多个文件用","隔开，上限100条|
| timestamp | int | true | 0 | 时间戳 |
| sign | string | true | NULL | 签名串 |

#### 返回值

```
{
    "status": 200,
    "message": "操作成功"
}
```

### 刷新存储空间文件目录

#### 描述
由客户端向NextJoySdk请求刷新存储空间文件目录
此接口必须签名，参照“接口签名”标题下的签名方法

#### 接口
http://sdktest.game.nextjoy.com/upload_token/batch_refresh_dir

请求方式：http post

#### 参数

| 参数名 | type | 是否必须 | 默认值 | 描述 |
| :--- | :--- | :--- | :--- | :--- |
| appid | int | true | 0 | App唯一ID |
| child_id | int | true | 0 | 马甲包ID |
| dirs | string | true | NULL |要刷新的文件目录，多个目录用","隔开，上限10条|
| timestamp | int | true | 0 | 时间戳 |
| sign | string | true | NULL | 签名串 |

#### 返回值

```
{
    "status": 200,
    "message": "操作成功"
}
```

### 接口签名

#### 签名方法说明
客户端SDK与服务器交互的每个接口都需要进行数据签名验证，这里对签名方法做如下约定：
1. 签名方法为MD5

2. 待签名数据为接口请求中所带的data数据项的所有子项


#### 签名生成步骤

* 1.filterdata为data中数据剔除sign字段后的所有数据项。
* 2.将filterdata中的所有数据按照键值升序排列，结果记为sortdata。
* 3.将sortdata中的数据按照key+'|'+value+'#'的格式拼接，记为preStr
* 4.取标准MD5(preStr+appSecret)，其中appSecret为平台得到的app秘钥。


#### 签名示例

appSecret：'23094b343e52485b4fbf9d94a8bc55a5'

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
    't': 1525756884,
    'timestamp': 1525756884,
    'sdk_ver': '1.0',
    'device_name': 'malei_android',
    'device_os_ver': '123',
    'actoken': 'cuax2yEdX75/jDNdsDaxTSE8=jia=fZNqOD5AUu0Z2y0J9v2GaJjag8Mp/4M5PTeDeO1'
}
```

将data按照key进行排序，生成sortdata

将sortdata中的数据按照key+'|'+value+'#'的格式拼接,得到preStr

```
preStr = acid|1818#actoken|cuax2yEdX75/jDNdsDaxTSE8=jia=fZNqOD5AUu0Z2y0J9v2GaJjag8Mp/4M5PTeDeO1#api_ver|1.0#app_ver|1.0#app_ver_code|12.0#appid|1001#channel_id|1#child_id|1000#device_name|malei_android#device_os_ver|123#imei|fghjkl;#os|1#package_id|1#sdk_ver|1.0#t|1525756884#timestamp|1525756884#23094b343e52485b4fbf9d94a8bc55a5
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
    't': 1525756884,
    'timestamp': 1525756884,
    'sdk_ver': '1.0',
    'device_name': 'malei_android',
    'device_os_ver': '123',
    'actoken': 'cuax2yEdX75/jDNdsDaxTSE8=jia=fZNqOD5AUu0Z2y0J9v2GaJjag8Mp/4M5PTeDeO1',
    'sign': '7E6AA323D6A95DCF1499875AB8CA537E'
}
```


#### 接口错误码

```
(200, "操作成功")
(18000, "token校验失败");
(18005, "获取uploadToken失败"),
(18006, "获取uploadToken请求超时"),
(18007, "获取uploadToken请求校验失败"),
(18008, "批量删除文件失败"),
(18009, "批量删除的文件过多"),
```

