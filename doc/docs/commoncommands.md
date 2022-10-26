# 常用命令
adb
---
```
adb pull /storage/emulated/0/Android/data/com.duowan.mobile/files/yymobile/logs/.  C:\Users\YY\Downloads\log11  下载手机日志
adb shell "dumpsys window | grep mCurrentFocus"   查看正在前台运行的activity  查看当前获取焦点的window实例

a db shell am start -a android.intent.action.VIEW + 路由地址

adb shell pm clear com.baidu.searchbox    清数据  adb shell pm clear + 包名

adb logcat -v(日志级别) （日志过滤） >> 输出文件路径
    eg:adb logcat -v thread >> 111.text
    adb logcat >> 111.text


adb bugreport C:\Users\YY\Downloads\log11     导出错误日志

无线连接
第一步：Android设备开启USB调试，并且通过USB线连接到电脑。
第二步：在终端执行以下命令”adb tcpip 5555“。
第三步：在终端执行以下命令”adb connect 192.168.1.110“(192.168.1.110为Android设备的IP地址)。此时拔出USB线，应该就可以adb通过wifi调试Android设备。
```

git
---
``` kotlin
git撤销提交（commit）
git reset --soft HEAD~1 撤销commit 保留修改
git reset --hart HEAD^ 撤销commit 不保留修改
git 撤销push
git log   找到你想回滚版本唯一的commit标识代码,
git revert 标识码 。 （标识码对应的提交会被回滚）
git revert是用一次新的commit来回滚之前的commit，git reset是直接删除指定的commit

合并分支
git merge pluginmain-android_8.12.0_activityopt_yydev_feature
git reset --merge
git merge --abort
git其他用法
git stash  暂存修改
git stash show  查看刚才暂存的修改
git stash pop  取出修改
git stash save <message>
git stash list //查看暂存区的所有暂存修改
git stash pop //取出最近一次暂存并删除记录列表中对应记录
git stash apply stash@{X} //取出相应的暂存
git stash drop stash@{X} //将记录列表中取出的对应暂存记录删除
git stash clear 清空stash仓库
git status 查看状态
git 分支同步
git cherry-pick <commit id>
v1暂时先这样，以后用到在修改
1. 如果顺利，就会正常提交
2. 如果在cherry-pick 的过程中出现了冲突
    2.1 $ git status # 看哪些文件出现冲突，手动解决冲突
    2.2 git cherry-pick --continue  #继续提交


账号
个人账号xiaodandingzp   邮箱 2440788861@qq.com  ssh密码 ZP123zhang
公司账号 zhangping2  邮箱zhangping2@yy.com
 
Git命令
Git config user.name [name]
Git config user.email [email]
Git config –global user.name [name]
Git config –global user.email [email]
```
参考链接： [git参考链接](https://x-front-team.github.io/2017/03/27/%E5%85%AC%E5%8F%B8%E5%92%8C%E4%B8%AA%E4%BA%BAgit%E8%B4%A6%E5%8F%B7%E5%A6%82%E4%BD%95%E5%90%8C%E6%97%B6%E5%9C%A8%E4%B8%80%E5%8F%B0%E7%94%B5%E8%84%91%E4%BD%BF%E7%94%A8/)

如何反编译系统App﻿
---
```
1、获取需要获取系统App的包名
这个有很多方式，可以使用adb命令获取当前Activity名称，可以看到包名

2、获取App安装路径
adb shell pm path 包名
例如：adb shell pm path com.huawei.android.launcher
返回：﻿package:/system/app/HwLauncher6/HwLauncher6.apk﻿

3、拉取App文件
abd pull App文件路径
例如：adb pull /system/app/HwLauncher6/
这样可以拉到到一个文件夹，里面文件是这样：
HwLauncher6.apk
oat
arm64
HwLauncher6.odex
HwLauncher6.vdex
vdex：Android 8.0在odex的基础上又引入了vdex机制，目的是为了降低dex2oat时间
这个是优化后的dex，无法反编译查看代码

4、vdex to dex
需要借助这个工具，这个工具在8.0以及以下是可以直接生成dex，8.0以上会生成cdex，所以需要第五步﻿https://github.com/anestisb/vdexExtractor﻿教程：﻿https://blog.csdn.net/Alexwym/article/details/107730906﻿

5、cdex to dex
通过compact_dex_converter转换cdex为dex﻿https://github.com/anestisb/vdexExtractor/files/2351602/compact_dex_converter_android_arm64-v8a.zip﻿﻿https://github.com/anestisb/vdexExtractor/files/2351603/compact_dex_converter_android_armeabi-v7a.zip﻿
教程：﻿https://www.bilibili.com/read/cv10063504﻿
示例：
adb push .\compact_dex_converter /data/local/tmp
adb shell chmod 777 /data/local/tmp/compact_dex_converter
adb push .\HwLauncher6_classes.cdex /data/local/tmp
adb shell "/data/local/tmp/compact_dex_converter /data/local/tmp/HwLauncher6_classes.cdex"
最后拉取结果文件夹
adb pull /data/local/tmp
```