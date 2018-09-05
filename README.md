# NextJoy 游戏SDK文档说明

### 注意事项

- 对接过程中appid, appkey, appsecret由游戏方向NextJoy游戏平台申请获得
- 游戏方在对接时需要至少提供以下信息
    - 游戏名称
    - 游戏bundleID（包名）
    - 货币名称
    - 兑换比例
    - 支付结果回调地址
    - 游戏标识
    - 付费道具标识和对应金额（iOS申请内购产品ID）
    - 游戏icon （规格：512*512 ）
    - 第三方登录相关配置（微信、QQ平台申请）

- 接入sdk的游戏母包请勿做加固处理
- 游戏进入流程，母包需要保证游戏进入为如下顺序：
    - 健康忠告（包含版号等信息）
    - 登录界面（包含一个登录按钮）
    - 进入游戏

```flow
st=>start: start:>http://www.baidu.com
op1=>operation: 操作1
cond1=>condition: YES or NO?
sub=>subroutine: 子程序
e=>end

st->op1->cond1
cond1(yes)->e
cond1(no)->sub(right)->op1  
```
