## 安卓
### 注意事项
*    appId ,appKey由明日世界平台申请获得
*   
*    android:minSdkVersion值大于等于11
*    获取资源方式请勿使用R.xx.xx ,使用西游SDK提供的NextJoyResourceUtil进行资源获取
*    需要在AndroidManifest.xml中application节点配置com.nextjoy.sdk.NextJoyApplication，如有自定义application，需要继承至com.nextjoy.sdk.NextJoyApplication
*    标记*必接的接口请务必全部接入
*    接入的SDK的游戏母包不能做加固处理
*    提供游戏母包时，请附带提供一张512*512的游戏icon
*    apk的编译请使用android21
*    游戏进入流程，母包必须保证进入游戏的顺序是 健康忠告（包含版号等信息）-----登录界面（必须包含一个登录按钮）-------进入游戏
*    必须保证游戏主activity在AndroidManifest.xml中配置android:configChanges="orientation|keyboardHidden|screenSize"，其中值至少包含"orientation|keyboardHidden|screenSize" 以避免横竖屏切换游戏activity被重启
### Android SDK 接入步骤
1. 导入NextJoy_Kernel.aar和NextJoySDK-TOOL.aar两个文件到libs目录
2. 在build.gradle文件中添加如下引用
```
compile(name: 'NextJoy_Kernel', ext: 'aar')
compile(name: 'NextJoySDK-TOOL', ext: 'aar')
compile 'com.android.support:support-v4:21.0.0'
compile 'com.android.support:multidex:1.0.2'
compile 'com.squareup.okhttp3:okhttp:3.6.0'
compile 'com.google.code.gson:gson:2.8.0'
compile 'com.tencent.mm.opensdk:wechat-sdk-android-without-mta:+'
```
3. 引入sdk配置

### 引入SDK配置

#### AndroidManifest.xml配置

##### 权限配置
```
<uses-permission android:name="android.permission.INTERNET" />
<uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE" />
<uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />
<uses-permission android:name="android.permission.ACCESS_WIFI_STATE" />
```
##### 组件配置
```
<!--请勿修改-->
<meta-data android:name=“NEXTJOY_SDK_VERSION"
android:value=“1.0.0” />

<activity
android:name="com.nextjoy.sdk.p.view.activity.NextJoyMainActivity"
android:configChanges="screenSize|keyboardHidden|orientation"
android:launchMode="singleTask"
android:windowIsTranslucent="false"
android:windowDisablePreview="true"
android:theme="@style/nj_transparent" >
</activity>

<service android:name="com.nextjoy.sdk.p.view.widget.FloatService"/>

<activity
android:name="com.tencent.tauth.AuthActivity"
android:noHistory="true"
android:launchMode="singleTask" >
<intent-filter>
<action android:name="android.intent.action.VIEW" />
<category android:name="android.intent.category.DEFAULT" />
<category android:name="android.intent.category.BROWSABLE" />
<data android:scheme="tencent+appID” /><!-- 此APPID为应用在腾讯平台申请，由明日世界负责申请提供—>
</intent-filter>
</activity>
<activity
android:name="com.tencent.connect.common.AssistActivity"
android:configChanges="orientation|keyboardHidden"
android:screenOrientation="behind"
android:theme="@android:style/Theme.Translucent.NoTitleBar" />
<activity android:name="包名.wxapi.WXEntryActivity"
android:exported="true"
android:label="@string/app_name"
android:launchMode="singleTop"
android:theme="@android:style/Theme.Translucent" />
```

##### 微信回调页面WXEntryActivity配置
```
import android.os.Bundle;
import com.nextjoy.sdk.INextJoySDKListener;
import com.nextjoy.sdk.NextJoyGameSDK;
import com.nextjoy.sdk.p.DefaultSDKHandler;
import com.nextjoy.sdk.p.view.activity.NextJoyWXCallBackActivity;

public class WXEntryActivity extends NextJoyWXCallBackActivity implements INextJoySDKListener {
@Override
public void onCreate(Bundle savedInstanceState) {
try {
super.onCreate(savedInstanceState);
DefaultSDKHandler.getInstance().getWxApi().handleIntent(getIntent(), this);
NextJoyGameSDK.getInstance().addSDKListener(this);
} catch (Exception e){
}
}

@Override
public void onResult(int code, String msg) {
finish();
}

@Override
protected void onDestroy() {
NextJoyGameSDK.getInstance().removeListener(INextJoySDKListener.class,this);
super.onDestroy();
}
}
```
注：此处不需要修改，直接copy就行

### 生命周期接入
```
@Override
protected void onStart() {
super.onStart();
NextJoyGameSDK.getInstance().onStart();
}

@Override
protected void onRestart() {
super.onRestart();
NextJoyGameSDK.getInstance().onRestart();
}

@Override
protected void onResume() {
super.onResume();
NextJoyGameSDK.getInstance().onResume();
}

@Override
protected void onStop() {
super.onStop();
NextJoyGameSDK.getInstance().onStop();
}

@Override
protected void onDestroy() {
super.onDestroy();
NextJoyGameSDK.getInstance().onDestroy();
}

@Override
protected void onPause() {
super.onPause();
NextJoyGameSDK.getInstance().onPause();
}

@Override
protected void onActivityResult(int requestCode, int resultCode, Intent data) {
super.onActivityResult(requestCode, resultCode, data);
NextJoyGameSDK.getInstance().onActivityResult(requestCode, resultCode, data);
}


@Override
protected void onNewIntent(Intent intent) {
super.onNewIntent(intent);
NextJoyGameSDK.getInstance().onNewIntent(intent);
} 
```
### 业务功能接入

#### 初始化
NextJoyGameSDK.getInstance().init(Activity mActivity, NextJoyGameConfig config , INextJoySDKCallBack sdkListener)

| 参数名 | 说明 | 可否为空 |
| :--- | :--- | :--- |
| mActivity | 游戏主Activity上下文，请勿传递闪屏等Activity对象 | 否 |
| config | setDebugState debug 模式 setOrientation 屏幕方向 setAppId 明日世界提供appId setAppKey  明日世界提供appKey | 否 |
| sdkListener | SDK全局回调对象 | 否 |

注：必接，注意在未初始化成功时请勿进入游戏或调用SDK其它接口

#### 登录

NextJoyGameSDK.getInstance().login(boolean isClosable)

| 参数名 | 说明 | 类型 | 可否为空 |
| :--- | :--- | :--- | :--- |
| isClosable |登录界面是否可关闭| boolean | 否 |
注：必接，登录

#### 注销

NextJoyGameSDK.getInstance().logout()

注：必接，注销

#### 支付

NextJoyGameSDK.getInstance().pay(PayParams payParams)

| 参数名 | 说明 | 类型 | 可否为空 |
| :--- | :--- | :--- | :--- |
| payParams.setStatus |订单状态码| int | 否 |
| payParams.setOrder_no | 支付订单号| String | 否 |
| payParams.setAmount | 支付订单金额(整数 单位为分)| String | 否 |
| payParams.setPay_url | 支付的返回的支付链接 | String | 否 |

注：必接，支付，请严格按照规范传递参数，status为下订单的状态码，下订单时SDK服务器返回的状态码

#### 支付流程
cp客户端向cp游戏服务端发起下单请求（需要从SDK中获取一下配置参数），cp服务器生成订单后将订单信息返回给cp客户端，cp客户端拿到订单信息后封装成PayParams对象，调用SDK的pay(PayParams payParams)方法，调起支付界面。
cp客户端下单请求参数
```
//基础参数
params.put("appid", NextJoyGameSDK.getInstance().getAppId());//应用id
params.put("child_id", NextJoyGameSDK.getInstance().getChildId());//应用小包id
params.put("channel_id", NextJoyGameSDK.getInstance().getChannelId());//渠道id
params.put("acid", NextJoyGameSDK.getInstance().getAcid());//子渠道id
params.put("package_id", NextJoyGameSDK.getInstance().getPackageId());//package ID
params.put("imei", trimParam(NextJoyDeviceInfoHelper.getInstance().sDeviceInfo.getDeviceId()));//设备唯一标示IMEI
params.put("timestamp", DataFormatUtil.formatString(System.currentTimeMillis() / 1000));//时间戳
params.put("os",1);//客户端 1：Android  2：ios
//业务参数
actoken ：平台分配给cp的token
```
#### 退出游戏

NextJoyGameSDK.getInstance().exit()

注：必接，退出游戏

#### 分享功能
*   NextJoyGameSDK.getInstance().shareWeiXin（NJShareInfo njShareInfo）;//微信分享接口（只适用与应用内只有微信分享功能的场景）

| 参数名 | 说明 | 类型 | 可否为空 |
| :--- | :--- | :--- | :--- |
| njShareInfo.shareInfoType |分享内容样式| 枚举 | 否 |
| njShareInfo.sharePattern | 分享途径| 枚举 | 否 |
| njShareInfo.title | 分享标题| String | 否 |
| njShareInfo.content | 分享内容| String | 否 |
| njShareInfo.shareUrl | 分享的链接地址 | String | 否 |
| njShareInfo.imagePath | 分享图片的本地路径| String | 否 |

```
shareWeiXin() API接口只适用于应用内只有微信分享的场景。如需求其他分享方式请使用share().调用shareWeiXin() API接口方法如下：
//例1
NJShareInfo njShareInfo = new NJShareInfo();
njShareInfo.setSharePattern(NJSharePattern.WEIXIN_CIRCLE);//设置分享途径：朋友圈
njShareInfo.setShareInfoType(NJShareInfoType.IMAGE);//设置分享内容格式：纯图片
njShareInfo.setImagePath(Environment.getExternalStorageDirectory()+"/DCIM/Screenshots/Screenshot_2018-05-28-08-55-10-87.png");//设置分享图片的本地路径
NextJoyGameSDK.getInstance().shareWeiXin(njShareInfo);
//例2
NJShareInfo shareInfo = new NJShareInfo();
shareInfo.setTitle("测试————标题");
shareInfo.setContent("测试--------内容");
shareInfo.setShareUrl("http://www.baidu.com");//设置分享链接
shareInfo.setShareInfoType(NJShareInfoType.IMAGE_TEXT);//设置分享内容格式：图文
shareInfo.setSharePattern(NJSharePattern.WEIXIN);//设置分享途径：微信好友
shareInfo.setImagePath(Environment.getExternalStorageDirectory()+"/DCIM/Camera/1496808702960.jpg");
NextJoyGameSDK.getInstance().shareWeiXin(shareInfo);
```

*   NextJoyGameSDK.getInstance().share（NJShareInfo njShareInfo）;//调起SDK分享界面

| 参数名 | 说明 | 类型 | 可否为空 |
| :--- | :--- | :--- | :--- |
| njShareInfo.shareInfoType |分享内容样式| 枚举 | 否 |
| njShareInfo.sharePattern | 分享途径| 枚举 | 否 |
| njShareInfo.title | 分享标题| String | 否 |
| njShareInfo.content | 分享内容| String | 否 |
| njShareInfo.shareUrl | 分享的链接地址 | String | 否 |
| njShareInfo.imagePath | 分享图片的本地路径| String | 否 |

```
share() API接口直接调起SDK内部的分享界面，暂时只提供微信 QQ平台的分享方式
NJShareInfo shareInfo = new NJShareInfo();
shareInfo.setTitle("测试————标题");
shareInfo.setContent("测试--------内容");
shareInfo.setShareUrl("http://www.baidu.com");
shareInfo.setShareInfoType(NJShareInfoType.IMAGE);
List<NJSharePattern> patternList = new ArrayList<>();
patternList.add(NJSharePattern.WEIXIN);
patternList.add(NJSharePattern.WEIXIN_CIRCLE);
shareInfo.setPattenConfig(patternList);//设置分享界面显示哪些分享方式，可不设置，默认显示：微信 朋友圈  qq  qq空间四种方式
shareInfo.setImagePath(Environment.getExternalStorageDirectory()+"/DCIM/Camera/1496808702960.jpg");
NextJoyGameSDK.getInstance().share(shareInfo);

```
注：
1.当使用shareWeiXin()API时，njShareInfo.setSharePattern参数必须设置为NJSharePattern.WEIXIN或者NJSharePattern.WEIXIN_CIRCLE，设置其他无效;shareInfo.setPattenConfigke 不需设置
2.当使用share() API时，njShareInfo.setSharePattern不需要设置,shareInfo.setPattenConfig可不设置，默认显示：微信 朋友圈  qq  qq空间四种方式。
3.所分享的图片大小应小于1.5M;否则分享有可能失败。因此需要客户端在保存分享图片的时候对图片做压缩处理。

*   分享参数说明
```
//分享内容样式
public enum NJShareInfoType {
IMAGE,//纯图片格式
IMAGE_TEXT,/图文格式
}
//分享途径
public enum NJSharePattern {
WEIXIN_CIRCLE,
WEIXIN,
QQ,
QQZONE,
}

```

### 全局回调监听对象
com.nextjoy.sdk.
INextJoySDKCallBack sdkListener

详细字段说明
回调接口名
*    onInitResult 初始化回调
```
参数：int responseCode 状态码 NextJoyCode.CODE_INIT_SUCCESS 初始化成功；NextJoyCode.CODE_INIT_FAIL 初始化失败
String responseMessage 状态描述
```
*    onLoginResult 登录回调
```
参数：int responseCode 状态码 NextJoyCode.CODE_LOGIN_SUCCESS 登录成功；NextJoyCode.CODE_LOGIN_FAIL 登录失败
String responseMessage 状态描述
String sAuthToken cp方验证token。登录失败时为空字符串
boolean bSwitchAccount  是否切换账号
```
*    onLogoutResult 注销回调
```
参数：int responseCode 状态码 NextJoyCode.CODE_LOGINOUT
String responseMessage 状态描述
```
*    onExitResult 退出回调
```
参数：int responseCode 状态码 NextJoyCode.CODE_EXIT
String responseMessage 状态描述
```
*    onPayResult 支付回调（注：H5的支付方式没有此回调，此时需要cp服务器接收到sdk服务器支付成功的通知时，自行通知cp客户端）
```
参数：int responseCode 状态码 NextJoyCode.CODE_PAY_SUCCESS 支付成功；NextJoyCode.CODE_PAY_FAIL 支付失败；NextJoyCode.CODE_PAY_WAIT
支付等待；NextJoyCode.CODE_PAY_CANCEL 支付取消
String responseMessage 状态描述
```
*    onAntiAddictionResult 身份认证查询接口回调
```
参数：int responseCode 状态码 NextJoyCode.CODE_QUERY_ANTIADDICTION_SUCCESS 查询成功； NextJoyCode.CODE_QUERY_ANTIADDICTION_FAIL 查询失败
String responseMessage 状态描述
int userCurrentStatus 身份认证状态 NextJoyCode.CODE_ANTIADDICTION_UNKNOWN 未进行身份认证；NextJoyCode.CODE_ANTIADDICTION_JUVENILES 未成年；NextJoyCode.CODE_ANTIADDICTION_ADULT 成年
```
*   onShareResult 分享回调
```
参数：int shareStatus 状态码 NextJoyCode.CODE_SHARE_SUCCESS 分享成功； NextJoyCode.CODE_SHARE_FAIL 分享失败；NextJoyCode.CODE_SHARE_CANCEL 分享取消
String responseMessage 状态描述
```
### 获取用户信息 NextJoyUserModel

LocalUserBuffer.getUserInfo()；
```
String account;//账号
String actoken;//cp游戏验证token
int user_type;//1：游客  0：正常用户
String phone;//手机号
String token;//登录token
int safe_level;//账号安全级别
int fcm;// -1：未进行防沉迷验证  0：未通过防沉迷验证  1：通过防沉迷验证
```
### 获取配置参数信息
```
NextJoyGameSDK.getInstance().getAppId();//应用id
NextJoyGameSDK.getInstance().getChildId();//应用小包id
NextJoyGameSDK.getInstance().getAcid();//子渠道id
NextJoyGameSDK.getInstance().getPackageId();//package ID
NextJoyDeviceInfoHelper.getInstance().sDeviceInfo.getDeviceId();//设备唯一标示IMEI 注：与明日世界平台相关的业务使用IMEI时，需使用此方法获取。
NextJoyGameConfig.API_VERSION;//核心库版本号
NextJoyGameConfig.SDK_VERSION;//sdk插件版本号
```
### 桌面精灵
```
DefaultSDKHandler.getInstance().showFloatMenu();//显示桌面精灵在默认位置或者上次显示的位置
NextJoyGameSDK.getInstance().showFloatMenu(int posX,int posY);//在指定位置显示桌面精灵  posX：指定位置所在屏幕的X轴坐标；posY：指定位置所在屏幕的Y轴坐标 posX >=0  posY >=0
DefaultSDKHandler.getInstance().hideFloatMenu();//隐藏桌面精灵

```
注：如需显示桌面精灵，只需在登录回调成功时调用显示桌面精灵的api即可。

### 遇到问题
* 调起sdk登录界面，手机锁屏再开屏时，游戏界面黑屏

参考：https://blog.csdn.net/huangkun125/article/details/80432121

* 支付没有回调

SDK现在接入h5的支付方式， SDK是没办法接收h5支付界面支付的回调。因此需要游戏方自行处理优化支付的界面逻辑