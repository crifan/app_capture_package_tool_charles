# iOS中安装Charles的ssl证书的典型步骤

iOS中安装Charles的ssl证书的过程，和安卓中基本上是一样的。

此处以iPhone为例去解释具体过程。

在确保iPhone中也已经设置了Wifi的代理为Charles后，用iPhone中的`Safari`去打开：

http://chls.pro/ssl

其内部也会自动跳转到：

http://charlesproxy.com/getssl

弹框提示：

```bash
此网站正尝试打开"设置"以向您显示一个配置描述文件。您要允许吗？
```

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
