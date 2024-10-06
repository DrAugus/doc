# 1 连接远程桌面

按文档账号密码进行连接（不会有人不会吧

## 2 游戏配置

### 文件导入

建议在D盘根目录新建以游戏名为名的文件夹，在该文件夹内新建 `release` `script` `install files` 三个子文件夹

* `release`放置 服务端代码发布组件 release unicode（如游戏无法运行需要校验是否漏放dll）同时放入`GMtools` `自动启动`脚本 及`License.ini`

* `script`放置 数据库脚本组件（网站类应用需要多放入一个`网站脚本`）

* `install files`放置 三个必备软件/Plugins `msodbcsqlx86` `nppInstaller32` `vc_redist.x86` (均建议32位)

## 3 数据库处理

### 3-1 启动

打开`SQL Server Management Studio`之后，服务器名称填入本地地址 `localhost`/`127.0.0.1` 即可，登录名为本地数据库登录名`sa`，密码由产品提供。点击连接。

### 3-2 表操作

`RYPlatformDB.dbo.GameGameItem`

* 示例: 40120,梭哈,3,127.0.0.1,RYTreasureDB,16777216,16777216,ShowHand.DLL,NO

`RYPlatformDB.dbo.GameKindItem`

* 示例: 40120,40120,1,10,0,梭哈,no,1,1,1,3,0

`RYPlatformDB.dbo.DataBaseInfo`

|字段|值|
|  :----:  | :----:  |
|`DBAddr`  |127.0.0.1 |
|`DBPort`  |1433 |
|`DBUser`  |sa用`GMtools` `XOR加密` |
|`DBPassword`  |‘密码’用`GMtools` `XOR加密` |
|`MachineID` | 用`GMtools`下方的机器标识 |
|首尾字段|auto update 无须填入|

## 4 iis php搭建

### part php

将本地`phpStudy`文件夹放到c盘根目录下，同时将项目php文件放入路径`C:\phpStudy\PHPTutorial\WWW`

使用`Notepad++`编辑`sqlsrv.php`

```php
$uid = "sa"; //数据库用户名
$pwd = "123456"; //数据库密码
$WXAppID = "wxd2f8910096ac966e"; 
$WXSecret = "5e67dac50e449fa57d75f957939773c0"; 
```

`$WXAppID` `$WXSecret`的修改主要是为了网页/h5游戏的访问，apk项目修改与否无影响

### part iis

* 打开Internet信息服务(`IIS`)管理器，点击右上角`添加网站`，网站名称键入`php`，物理路径为`C:\phpStudy\PHPTutorial\WWW`，apk端口为`8080`，其他为`80`，点击确定。

* 在该php网站IIS部分找到`处理程序映射`，点击`添加模块映射`，请求路径键入`*.php`，模块选择`FastCgiModule`
  ，可执行文件选择`C:\phpStudy\PHPTutorial\php\php-7.2.1-nts\php-cgi.exe`，名称随意（建议填php），点击确定。

全部操作完成在浏览器键入 <http://localhost:8080/index.php> 如为 `sql srv succ!` 即配置成功
(对于非apk项目需要导入默认index.php页才能看到成功标记)

> **如遇热更新失败需要检查IIS MIME配置**

```java
<staticContent>
    <mimeMap fileExtension=".plist" mimeType="application/octet-stream" />
    <mimeMap fileExtension=".jsc" mimeType="application/octet-stream" />
    <mimeMap fileExtension=".ExportJson" mimeType="application/octet-stream" />
    <mimeMap fileExtension=".apk" mimeType="application/octet-stream" />
    <mimeMap fileExtension=".json" mimeType="application/x-javascript" />
</staticContent>
```

## 5 游戏启动

打开`游戏服务器`进行房间配置。后续即可使用一键启动脚本。

## Cocos Creator 配置

点击左上角`Cocos Creator` -> `偏好设置` -> `原生开发环境`

分别配置

* `NDK路径` 为`yourandroidsdk\ndk-bundle`
* `Android SDK路径` 为`yourandroidsdk`
* `ANT路径` 为`yourant\bin`

## TIPS

可能遇到的问题

* 无法构建 compile failed

  可能由于项目路径过长
