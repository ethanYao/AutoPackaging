#自动化打包
###命令行打包ipa
xcodebuild archive打包的方法（xcode提供的）

1. clean工程

```
xcodebuild clean -workspace PeopleMoney.xcworkspace -scheme <projectName> -configuration Debug
```
2.生成archive文件，放在自己指定一个位置上

```
xcodebuild archive -workspace XXXX.xcworkspace -scheme XXXX -configuration Debug -archivePath /Users/sunflower/Desktop/ transfer/XXXX.xcarchive
```
3.如果到处的ipa包出现Domain=IDEDistributionErrorDomain Code=14 "No applicable devices found的错误，在当前目录下rvm system，参考[博客](http:// www.jianshu.com/p/4eb789f586b5)

4.导出ipa包，放到自己指定的位置上

```
xcodebuild -exportArchive -archivePath /Users/sunflower/Desktop/transfer/ XXXX.xcarchive -exportOptionsPlist /Users/sunflower/Desktop/ testPlist.plist -exportPath /Users/sunflower/Desktop/transfer/XXXX
```
注意，这个需要配置一个exportOptionsPlist文件，详情查看命令行xcodebuild - h

###上传到appstore
使用altool工具，其实就是application loader的方式上传：

```
/Applications/Xcode.app/Contents/Applications/Application\ Loader.app/Contents/Frameworks/ITunesSoftwareService.framework/Support/ altool
```
也可以先切到这个命令下执行相关操作

1. 验证这个ipa包是否可用有效：

```
altool --validate-app -f /Users/sunflower/Desktop/ transfer/XXXX/XXXX.ipa -u xxxxxx -p xxxxxx -t ios --output- format xml
```
2. upload到appstore：

```
altool --upload-app -f /Users/sunflower/Desktop/ transfer/XXXX/XXXX.ipa -u xxxxxx -p xxxxxx -t ios --output- format xml
```
如果出现报错可以尝试配一下软链，实际上就是快捷方式：

```
ln -s /Applications/Xcode.app/Contents/ Applications/Application\ Loader.app/Contents/itms /usr/local/itms
```

###另外
如果需要添加软链，可以再更目录下建一个bin文件，把相关配置放到里面，可以先看.bash_profile里面是否有profile文件，如果有就可以在根目录下的.profile里面export一个环境变量指向bin

还有安装rvm工具的[地址](http://www.jianshu.com/p/bea091f8448d  _t_t_t=0.4159752886240491)

####远程登录进行打包需要注意的事
ssh登录，如果打包提示证书或者一些不正常的提示报错，可以尝试先对远程的keychain进行解锁，再进行下列操作：

```
security -v unlock-keychain -p "login passWord" ~/Library/Keychains/ login.keychain
```
详情可以参考这个[博客](http://www.jianshu.com/p/172d0ce0b53c)