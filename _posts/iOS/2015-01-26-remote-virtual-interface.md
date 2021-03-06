---
layout: post
title: 通过远程虚拟接口(RVI)进行设备抓包
category: iOS
tags: [iOS, RVI]
keywords: iOS, RVI
description: 
---

> 在iOS5中新引入了“远程虚拟接口(remote virtual interface ,RVI)“的特性,可以在Mac中建立一个虚拟网络接口来作为iOS设备的网络栈，这样所有经过iOS设备的流量都会经过此虚拟接口，此虚拟接口代替了iOS设备本身的协议栈,但并没有将网络流量中转到Mac本身的网络连接上，这样所有网络连接都是iOS设备本身的，Mac电脑本身连不联网或者连接的网络类型都没有关系，而iOS设备本身可以为任意网络类型,2G/3G/WiFi等。在Mac电脑上使用任意抓包工具抓取RVI接口上的数据包就可以看到iOS设备上的所有网络数据.

## 1.CocoaPods安装

**Step.1** 修改Ruby Resources，采用taboo的镜像

```
$ gem sources --remove https://rubygems.org/
$ gem sources -a http://ruby.taobao.org/
```

通过`-l`参数查看一下设置:

```
$ gem sources -l

*** CURRENT SOURCES ***
  
http://ruby.taobao.org/
```

**Step.2** 打开Xcode6，进入Preferences，点击Locations选项，将Command Line Tools 版本变成Xcode6

**Step.3** 删除旧版本cocoapods

```
$ sudo gem uninstall cocoapods
```

**Step.4** 安装xcodeproj

```
$ sudo gem install xcodeproj
```

安装时控制台输出

```
Fetching: xcodeproj-0.19.4.gem (100%)
Successfully installed xcodeproj-0.19.4
Parsing documentation for xcodeproj-0.19.4
Installing ri documentation for xcodeproj-0.19.4
1 gem installed
```

xcodeproj安装成功

**Step.5** 安装cocoapods

```
$ sudo gem install cocoapods
```

安装时控制台输出：

```
Fetching: nap-0.8.0.gem (100%)
Successfully installed nap-0.8.0
Fetching: cocoapods-core-0.34.4.gem (100%)
Successfully installed cocoapods-core-0.34.4
Fetching: claide-0.7.0.gem (100%)
Successfully installed claide-0.7.0
Fetching: cocoapods-downloader-0.7.2.gem (100%)
Successfully installed cocoapods-downloader-0.7.2
Fetching: cocoapods-plugins-0.3.1.gem (100%)
Successfully installed cocoapods-plugins-0.3.1
Fetching: cocoapods-try-0.4.1.gem (100%)
Successfully installed cocoapods-try-0.4.1
Fetching: netrc-0.7.8.gem (100%)
Successfully installed netrc-0.7.8
Fetching: cocoapods-trunk-0.3.1.gem (100%)
Successfully installed cocoapods-trunk-0.3.1
Fetching: cocoapods-0.34.4.gem (100%)
Successfully installed cocoapods-0.34.4
Parsing documentation for nap-0.8.0
Installing ri documentation for nap-0.8.0
Parsing documentation for cocoapods-core-0.34.4
Installing ri documentation for cocoapods-core-0.34.4
Parsing documentation for claide-0.7.0
Installing ri documentation for claide-0.7.0
Parsing documentation for cocoapods-downloader-0.7.2
Installing ri documentation for cocoapods-downloader-0.7.2
Parsing documentation for cocoapods-plugins-0.3.1
Installing ri documentation for cocoapods-plugins-0.3.1
Parsing documentation for cocoapods-try-0.4.1
Installing ri documentation for cocoapods-try-0.4.1
Parsing documentation for netrc-0.7.8
Installing ri documentation for netrc-0.7.8
Parsing documentation for cocoapods-trunk-0.3.1
Installing ri documentation for cocoapods-trunk-0.3.1
Parsing documentation for cocoapods-0.34.4
Installing ri documentation for cocoapods-0.34.4
9 gems installed
```

cocoapods安装成功

**Step.6** 测试

```
$ pod search AFNetworking

-> AFNetworking (2.4.0)
 A delightful iOS and OS X networking framework.
 pod 'AFNetworking', '~> 2.4.0'
 - Homepage: https://github.com/AFNetworking/AFNetworking
 - Source:   https://github.com/AFNetworking/AFNetworking.git
 - Versions: 2.4.0, 2.3.1, 2.3.0, 2.2.4, 2.2.3, 2.2.2, 2.2.1, 2.2.0, 2.1.0, 2.0.3, 2.0.2, 2.0.1, 2.0.0, 2.0.0-RC3, 2.0.0-RC2, 2.0.0-RC1, 1.3.4, 1.3.3,
 1.3.2, 1.3.1, 1.3.0, 1.2.1, 1.2.0, 1.1.0, 1.0.1, 1.0, 1.0RC3, 1.0RC2, 1.0RC1, 0.10.1, 0.10.0, 0.9.2, 0.9.1, 0.9.0, 0.7.0, 0.5.1 [master repo]
 - Sub specs:   - AFNetworking/Serialization (2.4.0)   - AFNetworking/Security (2.4.0)   - AFNetworking/Reachability (2.4.0)   -
 AFNetworking/NSURLConnection (2.4.0)   - AFNetworking/NSURLSession (2.4.0)   - AFNetworking/UIKit (2.4.0)
```

测试安装成功。

## 2.工程初始化
**Step.1** 新建XCode工程，如：
**Step.2** 控制台进入工程根目录，执行pod init生成Podfile

```
$ cd CocoaPodsUseDemo
$ ll
drwxr-xr-x  10 xxw  staff  340 10 25 11:04 CocoaPodsUseDemo
drwxr-xr-x   5 xxw  staff  170 10 25 11:04 CocoaPodsUseDemo.xcodeproj
drwxr-xr-x   4 xxw  staff  136 10 25 11:04 CocoaPodsUseDemoTests
drwxr-xr-x@  4 xxw  staff  136 10 25 11:04 build
$ pod init
$ ll
drwxr-xr-x  10 xxw  staff  340 10 25 11:04 CocoaPodsUseDemo
drwxr-xr-x   5 xxw  staff  170 10 25 11:04 CocoaPodsUseDemo.xcodeproj
drwxr-xr-x   4 xxw  staff  136 10 25 11:04 CocoaPodsUseDemoTests
-rw-r--r--   1 xxw  staff  215 10 25 11:07 Podfile
drwxr-xr-x@  4 xxw  staff  136 10 25 11:04 build
```

可以看到生成一个Podfile文件，查看该文件内容：

```
$ cat Podfile 
# Uncomment this line to define a global platform for your project
# platform :ios, '6.0'
source 'https://github.com/CocoaPods/Specs.git'
target 'CocoaPodsUseDemo' do
end
target 'CocoaPodsUseDemoTests' do
end
```

## 3.添加第三方库

**Step.1** 需要添加AFNetworking，修改Podfile文件内容为：

```
platform :ios, '7.0'
source 'https://github.com/CocoaPods/Specs.git'
target 'CocoaPodsUseDemo' do
    pod 'AFNetworking', '~> 2.4.0'
end
target 'CocoaPodsUseDemoTests' do
end
```

**Step.2** 安装AFNetworking

```
$pod install
```

有时可能比较慢，一直在Analyzing dependencies。原因在于当执行install命令的时候会升级CocoaPods的spec仓库，加一个参数可以省略这一步，然后速度就会提升不少。如下：

```
pod install --verbose --no-repo-update
```

上面的verbose能够看到详细的安装日志，no-repo-update用来设定不更新spec仓库。
下面是详细的安装日志

```
$ pod install --verbose --no-repo-update
  Preparing

Analyzing dependencies

Inspecting targets to integrate
  Using `ARCHS` setting to build architectures of target `Pods`: (``)
  Using `ARCHS` setting to build architectures of target `Pods-CocoaPodsUseDemo`: (``)
  Using `ARCHS` setting to build architectures of target `Pods-CocoaPodsUseDemoTests`: (``)

Resolving dependencies of `Podfile`
Resolving dependencies for target `Pods' (iOS 7.0)
Resolving dependencies for target `CocoaPodsUseDemo' (iOS 7.0)
  - AFNetworking (~> 2.4.0)
    - AFNetworking/Serialization
    - AFNetworking/Security
    - AFNetworking/Reachability
    - AFNetworking/NSURLConnection
      - AFNetworking/Serialization
      - AFNetworking/Reachability
      - AFNetworking/Security
    - AFNetworking/NSURLSession
      - AFNetworking/Serialization
      - AFNetworking/Reachability
      - AFNetworking/Security
    - AFNetworking/UIKit
      - AFNetworking/NSURLConnection
      - AFNetworking/NSURLSession
Resolving dependencies for target `CocoaPodsUseDemoTests' (iOS 7.0)

Comparing resolved specification to the sandbox manifest
  A AFNetworking

Downloading dependencies

-> Installing AFNetworking (2.4.0)
 > Git download
 > Git download
     $ /usr/bin/git clone https://github.com/AFNetworking/AFNetworking.git
     /Users/xxw/Documents/workspace/CocoaPodsUseDemo/Pods/AFNetworking --single-branch --depth 1 --branch 2.4.0
     Cloning into '/Users/xxw/Documents/workspace/CocoaPodsUseDemo/Pods/AFNetworking'...
     Note: checking out '3babb48b6fe078a96a6a9edf49c577d1e6902853'.
     
     You are in 'detached HEAD' state. You can look around, make experimental
     changes and commit them, and you can discard any commits you make in this
     state without impacting any branches by performing another checkout.
     
     If you want to create a new branch to retain commits you create, you may
     do so (now or later) by using -b with the checkout command again. Example:
     
       git checkout -b new_branch_name
     
   $ /usr/bin/git submodule update --init
  - Running pre install hooks

Generating Pods project
  - Creating Pods project
  - Adding source files to Pods project
  - Adding frameworks to Pods project
  - Adding libraries to Pods project
  - Adding resources to Pods project
  - Linking headers
  - Installing targets
    - Installing target `Pods-CocoaPodsUseDemo-AFNetworking` iOS 7.0
    - Installing target `Pods-CocoaPodsUseDemo` iOS 7.0
  - Running post install hooks
  - Writing Xcode project file to `Pods/Pods.xcodeproj`
  - Writing Lockfile in `Podfile.lock`
  - Writing Manifest in `Pods/Manifest.lock`

Integrating client project

[!] From now on use `CocoaPodsUseDemo.xcworkspace`.

Integrating target `Pods-CocoaPodsUseDemo` (`CocoaPodsUseDemo.xcodeproj` project)
```

注意上面的“[!] From now on use `CocoaPodsUseDemo.xcworkspace`”，后面XCode打开CocoaPodsUseDemo.xcworkspace，不使用xcodeproj。

**Step.3** 打开CocoaPods生成的workspace文件
 
上面能够看到多了一个Pods工程，里面添加有AFNetworking。
编译工程，在模拟器下链接错误。修改Library Search Paths，添加模拟器lib路径，如下图。
编译成功。

## 4.创建的Pod
将自己的工程模块创建成Pod給其他工程使用。
**Step.1** 本地创建TestPod目录

```
$ mkdir TestPod
$ cd TestPod
$ mkdir Classes
```

**Step.2** 创建.podspec文件

```
$ pod spec create TestPod
Specification created at TestPod.podspec
```

**Step.3** 提交TestPod到svn库，如并提交到SVN库。
如：https://ip:port/svn/CocoaPods/TestPod。
**Step.4** 修改TestPod.podspec如下

```
s.source = { :svn => "https://ip:port/svn/CocoaPods/TestPod"}
s.source_files  = "Classes", "Classes/**/*.{h,m}"
```

提交SVN

**Step.5** 打开CocoaPodsUseDemo工程的Podfile，添加

```
pod 'TestPod', :svn => 'https://ip:port/svn/CocoaPods/TestPod'
```

**Step.6** 更新

```
pod update --verbose --no-repo-update
```

**Step.7** 打开CocoaPodsUseDemo.workspace，会看到多了一个Pod，如下图：
 
**Step.8** 编译工程，Success!

## 5.创建Repo (Git)
**Step.1** 仓库的结构
 .
├── Specs
    └── [SPEC_NAME]
        └── [VERSION]
└── [SPEC_NAME].podspec
本地创建如下目录：
 
**Step.2** 将仓库提交到Git服务器
**Step.3** 在本地环境添加repo

```
$ pod repo add REPO_NAME SOURCE_URL
```

**Step.4** 查看

```
$ pod repo list
pod repo list

master
- Type: git (origin)
- URL:  https://github.com/CocoaPods/Specs.git
- Path: /Users/xxw/.cocoapods/repos/master
```

**Step.5** To check if your installation is successful and ready to go:

```
$ cd ~/.cocoapods/repos/REPO_NAME
$ pod repo lint .
```

**Step.6** Add your Podspec to your repo
Make sure you've tagged and versioned your source, then run:

```
$ pod repo push REPO_NAME SPEC_NAME.podspec
```

**Step.7** Your private Pod is ready to be used in a Podfile. You can use your spec repository using the source directive as shown in the following example:

```
source 'URL_TO_REPOSITORY'
```

**Step.8** remove a Private Repo

```
pod repo remove [name]
```

## 6.创建Repo (SVN)
CocoaPods要求Repo为git，但可以使用开源的插件https://github.com/clarkda/cocoapods-repo-svn添加对SVN Repo的支持。
**Step.1** 安装cocoapods-repo-svn

```
$ sudo gem install cocoapods-repo-svn
Fetching: cocoapods-repo-svn-0.1.1.gem (100%)
Successfully installed cocoapods-repo-svn-0.1.1
Parsing documentation for cocoapods-repo-svn-0.1.1
Installing ri documentation for cocoapods-repo-svn-0.1.1
1 gem installed
```

**Step.2** 添加repo

```
$pod repo-svn add xxwrepo http://IP:Port/svn/CocoaPods/MasterSpecsRepo
```

**Step.3** 添加后查看

```
$ pod repo list

master
- Type: git (origin)
- URL:  https://github.com/CocoaPods/Specs.git
- Path: /Users/xxw/.cocoapods/repos/master

xxwrepo
- Type: local copy
- Path: /Users/xxw/.cocoapods/repos/xxwrepo

2 repos
```

**Step.4** 校验

```
$ cd ~/.cocoapods/repos/REPO_NAME
$ pod repo-svn lint .
Linting spec repo `xxwrepo`
.

-> [homepage] The homepage has not been updated from default
  - TestPod (0.0.1)

-> [summary] The summary is not meaningful.
  - TestPod (0.0.1)

Analyzed 1 podspecs files.

All the specs passed validation.
```

**Step.5** 查找TestPod

```
$ pod search TestPod

-> TestPod (0.0.1)
   A short description of TestPod.
   pod 'TestPod', '~> 0.0.1'
   - Homepage: http://EXAMPLE/TestPod
   - Source:   http://IP:Port/svn/CocoaPods/TestPod
   - Versions: 0.0.1 [xxwrepo repo]
```

**Step.6** 修改Podfile

最好：把Repo放在git上，而Spec放置在SVN上。

## 7.FAQ
1. 当我们通过cocopods引入依赖库时，需要显示或隐式注明引用的依赖库版本，具体写法和表示含义如下
pod ‘AFNetworking’      //不显式指定依赖库版本，表示每次都获取最新版本
pod ‘AFNetworking’, ’2.0′     //只使用2.0版本
pod ‘AFNetworking’, ‘> 2.0′     //使用高于2.0的版本
pod ‘AFNetworking’, ‘>= 2.0′     //使用大于或等于2.0的版本
pod ‘AFNetworking’, ‘< 2.0′     //使用小于2.0的版本
pod ‘AFNetworking’, ‘<= 2.0′     //使用小于或等于2.0的版本
pod ‘AFNetworking’, ‘~> 0.1.2′     //使用大于等于0.1.2但小于0.2的版本
pod ‘AFNetworking’, ‘~>0.1′     //使用大于等于0.1但小于1.0的版本
pod ‘AFNetworking’, ‘~>0′     //高于0的版本，写这个限制和什么都不写是一个效果，都表示使用最新版本
