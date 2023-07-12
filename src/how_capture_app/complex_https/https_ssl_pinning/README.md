# 破解https的SSL Pinning

如果有些https，在之前设置了好各种证书和配置后，看到的：

* 要么是`unknown`
* 要么是：加密的乱码
* 要么是：报错无法抓包

而无法看到我们希望的明文数据，则：

最大可能是，对方用了https的`SSL pinning`

## 什么是SSL pinning

* `SSL pinning`=`certificate pinning`=`证书绑定`=`SSL证书绑定`
  * 原理：
    * 内部加了ssl证书的校验，给合法的有效的证书，先计算出`fingerprint`，通过`fingerprint`指纹去匹配对比，才视为有效，否则拒绝访问
  * 效果：对方的app内部，只允许，承认其自己的，特定的证书
    * 导致此处Charles的证书不识别，不允许
    * 导致Charles无法解密看到https的明文数据
  * 详见
    * [SSL证书绑定 · iOS安全与防护 (crifan.org)](https://book.crifan.org/books/ios_security_protect/website/ios_security_protect/anti_crawler/ssl_pinning.html)
