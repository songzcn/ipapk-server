自动生成自签名HTTPS服务器，快速安装ipa、apk，基于[ios-ipa-server](https://github.com/bumaociyuan/ios-ipa-server)开发

# 支持
* OS X
* Ubuntu
* 其他平台未测试

# 需要
* [nodejs](https://nodejs.org/)

# 安装
```
$ npm install -g ipapk-server
```

# 用法
```
Usage: ipapk-server [option]

Options:

-h, --help                output usage information
-V, --version             output the version number
-p, --port <port-number>  set port for server (defaults is 1234)
```

## 开启服务
```
$ ipapk-server

# open https://ip:port/download on your iphone
# 推荐使用pm2等进程管理运行服务
```

## API
### 包上传
path:

``` 
POST /upload
```

param: 

```
package:安装包文件
```
response:

```
{
  platform: 'ios',
  build: '1608051045',
  bundleID: 'com.jianshu.Hugo',
  version: '2.11.4',
  name: 'Hugo',
  guid: '46269d71-9fda-76fc-3442-a118d6b08bf1' 
}
```
命令行:`curl 'https://ip:port/upload' -F "package=@文件
路径" --insecure`，不能去掉`@`

### 所有App
path:

```
GET /apps/:platform/:page
```
params:

```
:platform: ios or android
:page: 分页，默认1
```
response:

```
[
	{
		id: 6,
		guid: "46269d71-9fda-76fc-3442-a118d6b08bf1",
		bundleID: "com.jianshu.Hugo",
		version: "2.11.4",
		build: "1608051045",
		icon: "https://10.20.30.233:1234/icon/46269d71-9fda-76fc-3442-a118d6b08bf1.png",
		name: "Hugo",
		uploadTime: "2016-12-01 20:50:05",
		platform: "ios",
		url: "itms-services://?action=download-manifest&url=https://10.20.30.233:1234/plist/46269d71-9fda-76fc-3442-a118d6b08bf1"
	},
	{
		id: 3,
		guid: "baac66f0-0e7b-f72c-40e3-378aab26fd9b",
		bundleID: "com.jianshu.victor",
		version: "1.1.0",
		build: "1611251530",
		icon: "https://10.20.30.233:1234/icon/baac66f0-0e7b-f72c-40e3-378aab26fd9b.png",
		name: "Victor",
		uploadTime: "2016-11-26 20:47:43",
		platform: "ios",
		url: "itms-services://?action=download-manifest&url=https://10.20.30.233:1234/plist/baac66f0-0e7b-f72c-40e3-378aab26fd9b"
	}
]
```
### 某个App的所有版本
path:

```
/apps/:platform/:bundleID/:page
```
params:

```
:platform: ios or android
:bundleID: app bundleID
:page: 分页，默认1
```
response:

```
[
	{
		id: 5,
		guid: "a8573b7a-18bc-1925-f2b4-8842db2153aa",
		bundleID: "com.jianshu.Hugo",
		version: "2.11.4",
		build: "1608051045",
		icon: "https://10.20.30.233:1234/icon/a8573b7a-18bc-1925-f2b4-8842db2153aa.png",
		name: "Hugo",
		uploadTime: "2016-11-26 21:00:51",
		platform: "ios",
		url: "itms-services://?action=download-manifest&url=https://10.20.30.233:1234/plist/a8573b7a-18bc-1925-f2b4-8842db2153aa"
	},
	{
		id: 6,
		guid: "46269d71-9fda-76fc-3442-a118d6b08bf1",
		bundleID: "com.jianshu.Hugo",
		version: "2.11.4",
		build: "1608051045",
		icon: "https://10.20.30.233:1234/icon/46269d71-9fda-76fc-3442-a118d6b08bf1.png",
		name: "Hugo",
		uploadTime: "2016-12-01 20:50:05",
		platform: "ios",
		url: "itms-services://?action=download-manifest&url=https://10.20.30.233:1234/plist/46269d71-9fda-76fc-3442-a118d6b08bf1"
	}
]
```

### 安装app
* 手机使用浏览器(iOS必须使用Safari)打开`https://ip:port/download`页面
* 第一次打开会弹出警告`无法验证服务器`，请点击`详细信息`按钮安装证书，按指示一直点击下一步和完成(**强烈推荐使用静态IP，避免每次重新安装证书**)
* 点击`下载`在线安装`ipa`

# TODO
- 加入ChangeLog
- token验证