
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

