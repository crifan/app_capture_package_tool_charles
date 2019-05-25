# 破解https的SSL Pinning

如果有些https，在之前设置了好各种证书和配置后，看到的：

* 要么是`unknown`
* 要么是：加密的乱码
* 要么是：报错无法抓包

而无法看到我们希望的明文数据，则：

最大可能是，对方用了https的`SSL pinning`

## 什么是SSL pinning

`SSL pinning`=`证书绑定`=`SSL证书绑定`

对方的app内部，只允许，承认其自己的，特定的证书

导致此处Charles的证书不识别，不允许

导致Charles无法解密看到https的明文数据

尤其是：

### Android 7.0之后系统如何破解https的ssl pinning

对于Android 7.0 (API 24) 之后，做了些改动，使得系统安全性增加了，导致：

* APP 默认不信任用户域的证书
  * -》之前把Charles的ssl证书，安装到 `受信任的凭据 -> 用户` 就没用了，因为不受信任了
  * 只信任（安装到）系统域的证书

导致无法抓包https，抓出来的https的请求，都是加了密的，无法看到原文了。

对此，总结出相关解决思路和方案：

* （努力想办法）让系统信任Charles的ssl证书
  * 作为app的开发者自己：改自己的app的配置，允许https抓包
    * 重要提醒：前提是得到或本身有app的源码
  * 把证书放到受系统信任的系统证书中去
    * 重要提示：前提是手机已root
* 绕开https不去校验
  * 借助于其他（JustTrustMe等）工具绕开https的校验
    * 重要提示：需要借助其他XPosed等框架配合才可以

下面详细介绍每一种方案和具体如何操作：

### 自己修改app去增加配置，允许https抓包

通过修改app的配置，使得允许https抓包

而修改app的配置，又分两种：

* 自己有app源码
  * 可以通过修改源码的方式去添加允许https抓包的配置
* 自己没源码，只有apk
  * 借助第三方工具修改apk，增加配置，允许https抓包

下面详细介绍如何操作：

#### 通过修改app源码去增加配置允许htts抓包

前提：

* 要么你自己是该app的开发者
  * 本身就有源码，就是app的拥有者
* 要么是你想要破解app的人
  * 本身就不可能有源码
    * 但是
      * 如果，有技术，有能力，有运气，破解得到app源码
        * 那理论上也可以使用此办法
      * 注意：实际情况下，**往往没机会破解出app源码**
        * 所以此办法不适用

如果具备修改app的源码，则具体操作过程是：

修改`AndroidManifest.xml`，增加如下配置：

```xml
<?xml version="1.0" encoding="utf-8"?>
  <manifest ... >
    <application android:networkSecurityConfig="@xml/network_security_config"
                  ... >
    ...
    </application>
  </manifest>
```

在`res`目录下新建一个`xml`文件夹，再新建文件：

`res/xml/network_security_config.xml`

内容为：

##### 手机中已正确安装Charles证书

```xml
<?xml version="1.0" encoding="utf-8"?>
  <network-security-config>
    <domain-config>
      <domain includeSubdomains="true">你要抓取的域名</domain>
      <trust-anchors>
        <certificates src="user"/>
      </trust-anchors>
  </domain-config>
</network-security-config>
```

其中：

* `<certificates src="user"/>`：信任用户自己安装的证书

##### 手机中没安装Charles证书，但是已有Charles证书文件

```xml
<?xml version="1.0" encoding="utf-8"?>
    <network-security-config>
        <domain-config>
        <domain includeSubdomains="true">你要抓取的域名</domain>
        <trust-anchors>
        <certificates src="@raw/证书文件名"/>
        </trust-anchors>
    </domain-config>
</network-security-config>
```

然后再去：

* `res`目录下新建一个`raw`文件夹
  * 将手机上安装的证书文件放入`res/raw/`目录下
    * 支持的证书格式：`pem`，`ca`

#### 用工具修改apk增加配置允许https抓包

比如借助第三方工具：

[levyitay/AddSecurityExceptionAndroid](https://github.com/levyitay/AddSecurityExceptionAndroid)

去下载源码，再去：

```bash
cd AddSecurityExceptionAndroid
./addSecurityExceptions.sh ../xxx.apk
```

即可给apk增加允许https抓包的配置了，然后就可以继续用Charles抓包https了。

### 把证书放到系统信任区

* 背景和思路：既然只信任系统区的证书，那么可以想办法把Charles证书放到系统区，就可以被信任了，就可以https抓包了
  * 而系统信任的地方
    * 对应安卓的设置中的：`受信任的凭据 -> 系统`
    * 对应安卓系统目录：`/system/etc/security/cacerts/`
    * 对应的系统证书的名字有特定规则
      * 需要找到工具根据规则计算出名字后
      * 才能再去把证书放到系统区中
* 前提：手机已root
* 详细步骤
  * 计算证书名
    * `openssl x509 -subject_hash_old -in charles-ssl-proxying-certificate_saved.pem`
    * 算出数值，比如`3a1074b3`
  * 证书文件改名
    * 然后把原Charles证书`charles-ssl-proxying-certificate_saved.pem`改名为`3a1074b3.0`
  * 放到系统分区
    * 放到`/system/etc/security/cacerts/`
* 注意
  * 但是呢，现在多数手机都很难root了
    * 包括我之前的锤子M1L和很多常见品牌，比如小米、华为等，的最新手机
  * 如果真的可以root，那倒是容易此办法去解决ssl pinning的问题

### 用其他工具绕开https校验实现https抓包

* 确保了手机已root或越狱
  * Android：已root
    * 确保后续可以安装Xposed等工具
  * iOS：已越狱
    * 确保后续能安装`Cydia`等工具
* 再去用可以绕开/禁止`SSL pinning`的插件
  * `Android`
    * 基于`Xposed`的[JustTrustMe](https://github.com/Fuzion24/JustTrustMe)
      * 限制：
        * 只能支持`Android 7.0`之前的安卓
          * 超过`Android 7.0`就不工作了
    * 基于`Cydia`的[Android-SSL-TrustKiller](https://github.com/iSECPartners/Android-SSL-TrustKiller)
  * `iOS`
    * 基于Cydia的[SSL Kill Switch 2](https://github.com/nabla-c0d3/ssl-kill-switch2)
      * 旧版本：基于Cydia的[iOS SSL Kill Switch](https://github.com/iSECPartners/ios-ssl-kill-switch)

即可绕开ssl的验证，抓包到https被解密变成明文的数据。

下面详细解释，如何在已root的安卓中，借助XPosed和JustTrustMe去实现，绕开https证书校验，实现抓包https得到明文数据。