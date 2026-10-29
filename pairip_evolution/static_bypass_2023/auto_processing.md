# 谷歌play保护破解技术方案

>Date    : 2024-6-19
 Author  :  qyzhaojinxi
 
 [TOC]
 
## 一、简要说明

**特征识别：**

* 本方案主要针对那些所对应的原app就被谷歌商店进行保护处理的游戏mod
* 特征是在`lib`文件夹下任意架构文件夹中存在`libpairipcore.so`文件
* 现象：apk反编译后，未做任何修改或者修改部分内容后，重新打包对齐签名，安装后启动app会黑屏闪退，其中报错信息就包含`libpairipcore.so`

**行业现状：**

* apk发布到谷歌后台后，可被选择是否开启apk完整性保护，一般是不开启
* 目前开启play保护的游戏数量在逐渐增加，从采集到的竞品mod中可以分析出，之间增加，但整体占比还比较小
* 目前采集到的带play保护的大部分是已经被进行一定技术处理了，所以只需要进行一次核心技术处理，即可继续兼容
* 目前分析测试从部分平台采集到的mod本身是坏掉的，在后续推进处理时需注意，如gamedva

**play保护大概原理：**

* apk签名检测
* apk完整性检测
* 代码随机加密保护
* apk内文件是否被修改检测

## 二、脚本资源

**资源下载**

* 资源项目库：[点击下载](http://gitlab.everzones.com:6480/appcrackrepo/playprotecthelper)

* python环境：建议3.12.2

* `crc32helper.py`是核心脚本，另一个`new_zipfile.py`是被修改的python官方源码，作为被引用库

* 命令：

```
crc32helper.py copy old_apk new_apk output_apk
```

* 处理成功后会打印log：`New APK file generated and CRC32 set completed.`

## 三、执行流程

**使用场景**

* 当apk被反编译，然后按照需求进行相关内容修改，接下来重新编译打包成apk，此时使用本方案脚本对apk进行处理
* 处理完成后再对apk进行对齐和签名

**脚本使用**

* 核心命令：
```
crc32helper.py copy old_apk new_apk output_apk
```
* 命令解释：`old_apk`是反编译前的apk地址；`new_apk`是修改后重新打包的apk地址；`output_apk`是被该脚本处理成功后输出的apk地址
* 执行命令例子及输出：
```
python3 /Users/xxx/Documents/MyScripts/crc32/crc32helper.py copy '/Users/xxx/Downloads/Guardian-War-v0.2.145-mod-GameDVA.com.apk' '/Users/xxx/Downloads/Guardian-War-v0.2.145-mod-GameDVA.com/dist/Guardian-War-v0.2.145-mod-GameDVA.com.apk' '/Users/xxx/Downloads/Guardian-War-v0.2.145-mod-GameDVA.com/dist/Guardian-War-v0.2.145-mod-GameDVA.com_testcrc.apk'
New APK file generated and CRC32 set completed.
```

**重新对齐和签名**

* 被脚本处理后的output_apk此时无法安装，需要先对该apk进行对齐，然后进行签名
* 处理后游戏即可在安装后正常启动，不会再触发黑屏和闪退

## 四、备注

* 仅针对带play保护的mod，如果是原app，处理流程还需要再添加几个步骤
* 目前可能还存在一些特殊案例，使用了不同的破解或绕过技术，遇到这类案例可能不起作用，如有遇到需要记录反馈出来
* 使用本脚本处理前，需要确保处理前的apk可以安装且不会闪退，否则处理不起作用