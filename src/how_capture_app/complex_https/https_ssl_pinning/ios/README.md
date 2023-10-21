# iOS端

* 前提
  * iOS：已越狱
    * 确保后续能安装`Cydia`/`Sileo`等包管理器
* 绕过SSL pinning的工具
  * 插件=tweak=越狱插件
    * 最好用的是：`NyaMisty/ssl-kill-switch3`
      * 下载地址
        * https://github.com/NyaMisty/ssl-kill-switch3
          * https://github.com/NyaMisty/ssl-kill-switch3/releases/tag/v1.0
            * https://github.com/NyaMisty/ssl-kill-switch3/releases/download/v1.0/com.nablac0d3.sslkillswitch2_1.0_iphoneos-arm.deb
      * 插件详情
        * ![ios_bypass_pinning_nyamisty_ssl_kill_switch3](../../../../assets/img/ios_bypass_pinning_nyamisty_ssl_kill_switch3.png)
      * 效果
        * WhatsApp的https请求可以看到明文
          * https://v.whatsapp.net
            * ![ssl_kill_switch3_charles_whatsapp](../../../../assets/img/ssl_kill_switch3_charles_whatsapp.png)
        * Apple的相关请求也可以看到明文
          * https://gateway.icloud.com.cn
            * ![ssl_kill_switch3_charles_icloud](../../../../assets/img/ssl_kill_switch3_charles_icloud.png)
    * 其次是：`evilpenguin/SSLBypass`
      * 下载地址
        * https://github.com/evilpenguin/SSLBypass
          * https://github.com/evilpenguin/SSLBypass/blob/main/packages/com.evilpenguin.sslbypass_1.0-5%2Bdebug_iphoneos-arm.deb
      * 插件详情
        * ![ios_bypass_pinning_evilpenguin_sslbypass](../../../../assets/img/ios_bypass_pinning_evilpenguin_sslbypass.png)
    * 再次是：`nabla-c0d3/ssl-kill-switch2`
      * 下载地址
        * https://github.com/nabla-c0d3/ssl-kill-switch2
          * https://github.com/nabla-c0d3/ssl-kill-switch2/releases
            * https://github.com/nabla-c0d3/ssl-kill-switch2/releases/download/0.14/com.nablac0d3.sslkillswitch2_0.14.deb
      * 插件详情
        * ![ios_bypass_pinning_nabla_c0d3_ssl_kill_switch2](../../../../assets/img/ios_bypass_pinning_nabla_c0d3_ssl_kill_switch2.png)
      * 注：
        * 旧版本：[iOS SSL Kill Switch](https://github.com/iSECPartners/ios-ssl-kill-switch)
    * 其他
      * [SSL Kill Switch 2 (iOS 13)](https://julioverne.github.io/)
  * `Frida`的js
    * 绕过证书校验
      * [Frida CodeShare Project: iOS SSL Bypass](https://codeshare.frida.re/@lichao890427/ios-ssl-bypass/)

## 抓包举例

### SSL Kill Switch 2 (iOS 13)

#### Apple账号

经测试，Apple账号登录过程中的https请求：

* 除了特殊的，特定的：
  * https://gsa.apple.com
* 之外，其他普通的（包括带账号绑定的https请求），是可以抓包的，能看到明文的
  * 比如：
    * https://setup.icloud.com
    * https://bag.itunes.apple.com

具体步骤：

* 手机
  * iPhone8: `iOS 15.1`、`palera1n`的`rootful`越狱
* Sileo中安装插件
  * `julioverne`的`SSL Kill Switch 2 (iOS 13)`
    * 源地址：https://julioverne.github.io
      * ![julioverne_sileo_repo_src](../../../../assets/img/julioverne_sileo_repo_src.png)
    * 插件安装后效果
      * `com.julioverne.sslkillswitch2`(0.14c)
        * ![sileo_installed_tweak_sslkillswitch2](../../../../assets/img/sileo_installed_tweak_sslkillswitch2.png)
          * 已安装文件
            * ![sslkillswitch2_installed_files](../../../../assets/img/sslkillswitch2_installed_files.png)

https抓包效果：

* 能抓包明文的
  * https://bag.itunes.apple.com/bag.xml
    * ![charles_https_ok_bag_itunes](../../../../assets/img/charles_https_ok_bag_itunes.png)
  * https://setup.icloud.com/setup/qulify/session
    * ![charles_https_ok_icloud_setup](../../../../assets/img/charles_https_ok_icloud_setup.png)
* 无法抓包的
  * https://gsa.apple.com
    * ![charles_gsa_apple_CONNECT](../../../../assets/img/charles_gsa_apple_CONNECT.png)
      ```bash
      https://gsa.apple.com 
      200 
      CONNECT 
      gsa.apple.com 

      Tue Jul 04 15:24:43 CST 2023 
      1031 
      20568 
      Complete
      ```
