# 移动端安装Charles的ssl证书

此处整理，如何到移动端手机中安装Charles的ssl证书。

## 安卓中安装Charles的ssl证书的典型步骤

### 通过浏览器下载Charles的ssl证书

在给安卓中[设置了Wifi代理为Charles](http://book.crifan.com/books/app_capture_package_tool_charles/website/how_capture_app/normal_http_request.html)之后，再去安卓端浏览器打开：

http://chls.pro/ssl

其会自动跳转到

http://charlesproxy.com/getssl

然后会自动弹框去下载证书文件

注意：**不要用微信**去打开，改用手机中单独的浏览器，比如`QQ浏览器`去打开

![](../../../assets/img/xiaomi_9_charles_ssl_download_pem.png)

### 安装Charles的ssl证书

找到下载好的证书文件：

![](../../../assets/img/found_downloaded_charles_ssl_pem_file.png)

点击去安装，正常情况下，可以弹出用安装证书所用工具。

比如：

* 从微信等方式发送到手机端后点击证书显示的`证书安装工具`
  * ![](../../../assets/img/click_pem_install_by_cert_tool.png)
* 小米4中用浏览器下载到`getssl.crt`后点击弹框选择`证书安装工具`
  * ![](../../../assets/img/getssl_crt_click_choose_install_tool.png)

然后后续就是正常的安装证书的过程了。

另外，很多设备真正安装证书之前，需要进入设置PIN码或解锁图案的设置界面，比如：

* 小米9
  * ![](../../../assets/img/set_lock_before_install_cert.png)
* 小米4
  * ![](../../../assets/img/xiaomi_4_set_lock_before_cert.png)
  * ![](../../../assets/img/xiaomi_4_lock_type_choice.png)
  * ![](../../../assets/img/xiaomi_4_set_lock_pattern.png)

正常的证书安装过程是：

进入`为证书命名`界面，输入证书名：

![](../../../assets/img/input_cert_name_charles_m1l.png)

此处是：

* 证书名称：`Charles M1L`
  * 注：
    * 此处可以随意命名
    * 一般命名中包含Charles，更易于后期识别
* 凭据类型：`VPN和应用`
  * 注意：
    * 有两个选项：
    * ![](../../../assets/img/credential_type_not_choose_vpn.png)
    * **应该**选`VPN和应用`
    * **不要**选`WLAN`
      * 我之前错误理解为：此处Charles代理是用于Wifi，所以要选WLAN

然后就会显示`toast`提示：`已安装 xxx`：

![](../../../assets/img/installed_charles_cert.png)

### 确认Charles证书已正确安装

接下来再去确认Charles证书已正常安装：

`受信任的凭据 -> 用户` 中可以看到已安装的证书：

```bash
XK72 Ltd
Charles Proxy CA
```

![](../../../assets/img/trusted_user_show_xk72_charles.png)

点击后可以看到Charles证书的详情：

![](../../../assets/img/charles_ca_cert_detail.png)

另外，小米9中，还可以通过`用户凭据`中看到已安装的证书：

![](../../../assets/img/xiaomi_9_installed_use_credential_see_charles.png)

## iOS中安装Charles的ssl证书的典型步骤

iOS中安装Charles的ssl证书的过程，和安卓中基本上是一样的。

此处以iPhone为例去解释具体过程。

在确保iPhone中也已经设置了Wifi的代理为Charles后，用iPhone中的`Safari`去打开：

http://chls.pro/ssl

其内部也会自动跳转到：

http://charlesproxy.com/getssl

弹框提示：

`此网站正尝试打开"设置"以向您显示一个配置描述文件。您要允许吗？`

![](../../../assets/img/iphone_safari_pop_config_file_allow.png)

点击`允许`后，进入 安装描述文件 页：

![](../../../assets/img/iphone_install_charles_proxy_ca.png)

点击安装后，继续点击安装，弹出菜单后选择安装：

![](../../../assets/img/warning_unmanaged_root_charles_ca.png)

稍等片刻即可安装成功：

签名者 会显示绿色的 已验证✔️

![](../../../assets/img/verified_charles_proxy_ca.png)

即可。

点击可进入证书详情页：

![](../../../assets/img/charles_ca_file_detail.png)

![](../../../assets/img/charles_ca_file_detail_more.png)

### iOS 10.3+ 还需要信任根证书

对于`iOS 10.3`之后的系统，还需要再去信任根证书才可以：

`设置 → 通用 → 关于本机 → 证书信任设置`

![](../../../assets/img/iphone_settins_general.png)

![](../../../assets/img/iphone_about_ca_trust_setting.png)

去点击勾选：`Charles Proxy CA`

![](../../../assets/img/default_not_select_charles_proxy_ca.png)

![](../../../assets/img/select_charles_proxy_ca_continue.png)

![](../../../assets/img/trusted_charles_proxy_ca.png)
