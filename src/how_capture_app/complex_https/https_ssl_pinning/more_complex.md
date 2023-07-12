# 特殊情况

对于其他更加复杂的特殊情况，一般是：

* 特定的app，内部加了额外的反扒方面的技术手段
  * 解决方案：往往要涉及到，app的破解和逆向，找到关键点函数，才能针对性的去绕过处理
    * 注：关于app的逆向破解，详见
      * [Android逆向开发](https://book.crifan.org/books/android_reverse_dev/website/)
      * [iOS逆向开发](https://book.crifan.org/books/ios_reverse_dev/website/)

## 举例

### iOS端

#### 抖音

代码：

```c
#define CHARLES_CERT_FILE @"/Library/PreferenceLoader/Preferences/charles/charles-ssl-proxying-certificate.cer"

%hook TTNetworkManagerChromium

- (NSArray *)ServerCertificate {
    NSArray* serverCertList = %orig();

    NSMutableArray* newCertList = [NSMutableArray arrayWithArray: serverCertList];

    NSString *certResourcePath = CHARLES_CERT_FILE;

    NSFileManager *defaultManager = [NSFileManager defaultManager];
    BOOL isExistedCert = [defaultManager fileExistsAtPath: certResourcePath];
    if (isExistedCert) {
        NSData *certP12Data = [NSData dataWithContentsOfFile: certResourcePath];
        [newCertList addObject: certP12Data];
    }

    NSMutableArray* retNewCertList = [newCertList copy];
    return retNewCertList;
}

%end
```

* 注：
  * 已回复到[这里](https://iosre.com/t/charles%E6%8A%93%E6%8A%96%E9%9F%B3%E7%9A%84%E5%8C%85%E4%B8%BA%E5%95%A5%E9%83%BD%E6%98%AFunknow/20202)
  * 上述代码只是针对相对比较新的某个版本，比如：抖音 `17.8`、`18.9`等版本，才有效
    * 对于其他更新版本，则已无效。
