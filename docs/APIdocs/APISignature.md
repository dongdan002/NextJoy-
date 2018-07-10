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
