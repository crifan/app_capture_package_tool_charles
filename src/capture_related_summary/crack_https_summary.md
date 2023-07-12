# 破解https的ssl pinning心得

## 已root二手小米4安装Xposed

淘宝上买的[二手的已root的小米4，MI 4LTE-CU，Android 4.4.4](https://item.taobao.com/item.htm?id=570638208316)

在无端被`MIUI`自动升级，从`MIUI 5.8.5`升级为`MIUI 7.5.12.17`），导致：

* 丢失了root权限
* 丢失了卖家原先已安装好的Xposed Installer

需要再去想办法：

* 重新获取root权限
  * 用360超级Root去重新root
    * ![](../assets/img/xiaomi_4_360_root_xposed.png)
* 重装可用的Xposed
  * 也是费了番功夫的
    * 重新安装，会报错：`Xposed目前不兼容Android SDK版本19或您的处理器架构armeabi-v7a`
      * 试了N多个版本，都不行
    * 最后是从[这里](https://forum.xda-developers.com/showpost.php?p=64063168&postcount=62)找到了大神`SolarWarez`修改后的`v2.6`的版本的730KB的`Xposed`：
      * `XposedInstaller_v2.6.1_by_SolarWarez_20151129.apk`
      * 或
      * `XposedInstaller_v2.6.1_MIUI_edition_by_SolarWarez_20151129.apk`
    * 才得以正常安装和使用`Xposed`

## 别人的方案

### 雷电模拟器 + (Xposed+justTrustMe) 关闭SSL证书验证

[如何对使用了ssl pinning的APP（如知乎）进行抓包？ - 知乎 (zhihu.com)](https://www.zhihu.com/question/60618756)

> 我想请教一下，我现在在用charles来爬饿了么就出现这种情况，看了你这个帖子，安装上面去配置了
> 
> 但是我启用了SSL代理之后，饿了么就打开不了了

A: 估计是饿了么做了反扒，能检测到你用了代理，所以就不允许使用了。你可以试试：把真机换（比如夜神）模拟器，看看能否避开此问题

Q: 我搞好了，是SSL的问题，后来我用手机模拟器，安装了一个插件，跳过了APP检查CHARLES安全证书，就可以正常拦截到饿了么的数据了

这个事情弄了我一周，找插件也找了好几天才找到可用的，模拟器用夜神也不行，我还换了一台电脑去尝试，后来用雷电模拟器➕(Xposed+justTrustMe) 关闭SSL证书验证，就可以用可以用Cherles获取数据了
