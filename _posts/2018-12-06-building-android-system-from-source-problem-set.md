---
layout: post
title:  "Android 编译问题&常见错误集"
date:   2018-12-06
desc: ""
keywords: "android,rom,jackserver,ubuntu"
categories: [Android]
tags: [android,rom,ubuntu]
icon: icon-mobile-device
---
# 搭建配置环境+下载android源码
[配置环境](https://source.android.com/setup/build/requirements)

注意事项，源码文件大概60个G，保持虚拟机有足够的磁盘空间，如果磁盘空间不够会报错，需要手动增加虚拟机内存，以VM上运行Ubuntu 为例，手动关闭虚拟机后，进入Settings-Hard Disk(SCSI) 调整虚拟机硬盘尺寸，调整完后开机并没有显示磁盘空间变大，是因为这些磁盘空间还未被分配,通过安装一个gparted 工具进行手动分配磁盘空间
```
sudo apt-get install gparted
```




# 常见错误集

### [错误一 jack server问题](https://forum.xda-developers.com/android/software/aosp-cm-los-how-to-fix-jack-server-t3575179)

```
[  0% 3/59149] Ensuring Jack server is installed and started
Jack server already installed in "/home/vanshu/.jack-server"
Launching Jack server java -XX:MaxJavaStackTraceDepth=-1 -Djava.io.tmpdir=/tmp -Dfile.encoding=UTF-8 -XX:+TieredCompilation -Xmx2G -cp /home/vanshu/.jack-server/launcher.jar com.android.jack.launcher.ServerLauncher
[  0% 4/59149] //art/tools/cpp-define-generator:cpp-define-generator-data clang++ main.cc [linux]
ninja: build stopped: subcommand failed.
18:06:07 ninja failed with: exit status 1
```

### 错误二
#### 解决方法为在make -j4 命令之前执行

```sh
export LC_ALL=C
make -j4
```


```
[ 11% 7676/66826] //external/selinux/checkpolicy:checkpolicy lex policy_scan.l [linux]
FAILED: out/soong/.intermediates/external/selinux/checkpolicy/checkpolicy/linux_x86_64/gen/lex/external/selinux/checkpolicy/policy_scan.c 
prebuilts/misc/linux-x86/flex/flex-2.5.39 -oout/soong/.intermediates/external/selinux/checkpolicy/checkpolicy/linux_x86_64/gen/lex/external/selinux/checkpolicy/policy_scan.c external/selinux/checkpolicy/policy_scan.l
flex-2.5.39: loadlocale.c:130: _nl_intern_locale_data: Assertion `cnt < (sizeof (_nl_value_type_LC_TIME) / sizeof (_nl_value_type_LC_TIME[0]))' failed.
Aborted (core dumped)
[ 11% 7679/66826] //external/selinux/checkpolicy:checkpolicy clang policy_define.c [linux]
ninja: build stopped: subcommand failed.
03:50:56 ninja failed with: exit status 1

```


### [错误三 jackserver 内存不足到问题](https://stackoverflow.com/questions/35579646/android-source-code-compile-error-try-increasing-heap-size-with-java-option)

报错信息：
```
Android source code compile error: “Try increasing heap size with java option '-Xmx<size>'”
```

解决方法：
```
export JACK_SERVER_VM_ARGUMENTS="-Dfile.encoding=UTF-8 -XX:+TieredCompilation -Xmx4g"
./prebuilts/sdk/tools/jack-admin kill-server
./prebuilts/sdk/tools/jack-admin start-server
```