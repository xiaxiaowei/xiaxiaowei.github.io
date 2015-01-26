---
layout: post
title: CocoaPods使用说明
category: iOS
tags: [iOS, CocoaPods]
keywords: iOS,CocoaPods
description: 
---

> 详细介绍CocoaPods的集成和使用，已经定制Pods。

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

三、添加第三方库
1.需要添加AFNetworking，修改Podfile文件
$vim Podfile
修改内容为：
platform :ios, '7.0'
source 'https://github.com/CocoaPods/Specs.git'
target 'CocoaPodsUseDemo' do
    pod 'AFNetworking', '~> 2.4.0'
end
target 'CocoaPodsUseDemoTests' do
end

2.安装AFNetworking
$pod install
有时可能比较慢，一直在Analyzing dependencies。原因在于当执行install命令的时候会升级CocoaPods的spec仓库，加一个参数可以省略这一步，然后速度就会提升不少。如下：
pod install --verbose --no-repo-update
上面的verbose能够看到详细的安装日志，no-repo-update用来设定不更新spec仓库。
下面是详细的安装日志
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

注意上面的“[!] From now on use `CocoaPodsUseDemo.xcworkspace`”，后面XCode打开CocoaPodsUseDemo.xcworkspace，不使用xcodeproj。
3.打开CocoaPods生成的workspace文件.
 
上面能够看到多了一个Pods工程，里面添加有AFNetworking。
编译工程，在模拟器下链接错误。修改Library Search Paths，添加模拟器lib路径，如下图。
 
编译成功。

四、创建的Pod
将自己的工程模块创建成Pod給其他工程使用。
1.本地创建TestPod目录
$ mkdir TestPod
$ cd TestPod
$ mkdir Classes
2.创建.podspec文件
$ pod spec create TestPod
Specification created at TestPod.podspec
3.提交TestPod到svn库，如并提交到SVN库。
如：https://ip:port/svn/CocoaPods/TestPod。
4.修改TestPod.podspec如下
s.source = { :svn => "https://ip:port/svn/CocoaPods/TestPod"}
s.source_files  = "Classes", "Classes/**/*.{h,m}"
提交SVN
5.打开CocoaPodsUseDemo工程的Podfile，添加
pod 'TestPod', :svn => 'https://ip:port/svn/CocoaPods/TestPod'
6.更新
pod update --verbose --no-repo-update
7.打开CocoaPodsUseDemo.workspace，会看到多了一个Pod，如下图：
 
8.编译工程，Success!
五、创建Repo (Git)
1.仓库的结构
 .
├── Specs
    └── [SPEC_NAME]
        └── [VERSION]
└── [SPEC_NAME].podspec
本地创建如下目录：
 
2.将仓库提交到Git服务器
3.在本地环境添加repo
$ pod repo add REPO_NAME SOURCE_URL
4.查看
$ pod repo list
pod repo list

master
- Type: git (origin)
- URL:  https://github.com/CocoaPods/Specs.git
- Path: /Users/xxw/.cocoapods/repos/master
5. To check if your installation is successful and ready to go:
$ cd ~/.cocoapods/repos/REPO_NAME
$ pod repo lint .
6.Add your Podspec to your repo

Make sure you've tagged and versioned your source, then run:
$ pod repo push REPO_NAME SPEC_NAME.podspec
7. Your private Pod is ready to be used in a Podfile. You can use your spec repository using the source directive as shown in the following example:
source 'URL_TO_REPOSITORY'
8. remove a Private Repo
pod repo remove [name]

六、创建Repo (SVN)
CocoaPods要求Repo为git，但可以使用开源的插件https://github.com/clarkda/cocoapods-repo-svn添加对SVN Repo的支持。
1.安装cocoapods-repo-svn
$ sudo gem install cocoapods-repo-svn
Fetching: cocoapods-repo-svn-0.1.1.gem (100%)
Successfully installed cocoapods-repo-svn-0.1.1
Parsing documentation for cocoapods-repo-svn-0.1.1
Installing ri documentation for cocoapods-repo-svn-0.1.1
1 gem installed
2.添加repo
$pod repo-svn add xxwrepo http://IP:Port/svn/CocoaPods/MasterSpecsRepo
3.添加后查看
$ pod repo list

master
- Type: git (origin)
- URL:  https://github.com/CocoaPods/Specs.git
- Path: /Users/xxw/.cocoapods/repos/master

xxwrepo
- Type: local copy
- Path: /Users/xxw/.cocoapods/repos/xxwrepo

2 repos

4.校验
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

5.查找TestPod
$ pod search TestPod

-> TestPod (0.0.1)
   A short description of TestPod.
   pod 'TestPod', '~> 0.0.1'
   - Homepage: http://EXAMPLE/TestPod
   - Source:   http://IP:Port/svn/CocoaPods/TestPod
   - Versions: 0.0.1 [xxwrepo repo]
6.修改Podfile

最好：把Repo放在git上，而Spec放置在SVN上。

七、FAQ
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











### 什么是 IoC
> 控制反转（Inversion of Control，缩写为IoC），是面向对象编程中的一种设计原则，可以用来减低计算机代码之间的耦合度。其中最常见的方式叫做依赖注入（Dependency Injection，简称DI），还有一种方式叫“依赖查找”（Dependency Lookup）。通过控制反转，对象在被创建的时候，由一个调控系统内所有对象的外界实体，将其所依赖的对象的引用传递给它。 — [维基百科](http://zh.wikipedia.org/wiki/控制反转)

简单说来，就是一个类把自己的的控制权交给另外一个对象，类间的依赖由这个对象去解决。依赖注入属于依赖的显示申明，而依赖查找则是通过查找来解决依赖。

### Laravel 中的使用

注入一个类：

```php
App::bind('foo', function($app)
{
    return new FooBar;
});
```

这个例子的意思是创建一个别名为 `foo` 的类，使用时实际实例化的是 `FooBar`。

使用这个类的方法是：

```php
$value = App::make('foo');
```

`$value` 实际上是 `FooBar` 对象。

如果希望使用单例模式来实例化类，那么使用：

```php
App::singleton('foo', function()
{
    return new FooBar;
});
```

这样的话每次实例化后的都是同一个对象。

注入类的更多例子可以看 [Laravel 官网](http://laravel.com/docs/4.2/ioc)

你可能会疑问上面的代码应该写在哪儿呢？答案是你希望他们在哪儿运行就写在哪儿。0 —— 0 知道写哪儿还用来看这种基础文章么！

## 服务提供器 (Service Providers)
为了让依赖注入的代码不至于写乱，Laravel 搞了一个 **服务提供器（Service Provider）**的东西，它将这些依赖聚集在了一块，统一申明和管理，让依赖变得更加容易维护。

### Laravel 中的使用
定义一个服务提供器：

```php
use Illuminate\Support\ServiceProvider;

class FooServiceProvider extends ServiceProvider {

    public function register()
    {
        $this->app->bind('foo', function()
        {
            return new Foo;
        });
    }

}
```

这个代码也不难理解，就是申明一个服务提供器，这个服务提供器有一个 `register` 的方法。这个方法实现了我们上面讲到的依赖注入。

当我们执行下面代码：

```php
App::register('FooServiceProvider');
```

我们就完成一个注入了。但是这个还是得手动写，所以怎么让 Laravel 自己来做这事儿呢？

我们只要在 `app/config/app.php` 中的 `providers` 数组里面增加一行：

```php
'providers' => [
    …
       ‘FooServiceProvider’,
],
```

这样我们就可以使用 `App::make(‘foo’)` 来实例化一个类了。

你不禁要问了，这么写也太难看了吧？莫慌，有办法。

## 门面模式（Facade）
为了让 Laravel 中的核心类使用起来更加方便，Laravel实现了门面模式。

> 外觀模式（Facade pattern），是軟件工程中常用的一種軟件設計模式，它為子系統中的一組接口提供一個統一的高層接口，使得子系統更容易使用。 — [维基百科](http://zh.wikipedia.org/wiki/外觀模式)

### Laravel 中的使用
我们使用的大部分核心类都是基于门面模式实现的。例如：

```php
$value = Cache::get('key');
```

这些静态调用实际上调用的并不是静态方法，而是通过 PHP 的魔术方法 `__callStatic()` 讲请求转到了相应的方法上。

那么如何讲我们前面写的**服务提供器**也这样使用呢？方法很简单，只要这么写：

```php
use Illuminate\Support\Facades\Facade;

class Foo extends Facade {

    protected static function getFacadeAccessor() { return ‘foo’; }

}
```

这样我们就可以通过 `Foo::test()` 来调用我们之前真正的 `FooBar` 类的方法了。

## 别名（Alias）
有时候我们可能将 `Facade` 放在我们扩展库中，它有比较深的命名空间，如：`\Library\MyClass\Foo`。这样导致使用起来并不方便。Laravel 可以用别名来替换掉这么长的名字。

我们只要在 `app/config/app.php` 中 `aliases` 下增加一行即可：

```php
'aliases' => [
    …
    'Foo' => ‘Library\MyClass\Foo’,
],
```

这样它的使用就由 `\Library\MyClass\Foo::test()` 变成 `Foo::test()` 了。

## 总结
所以有了**控制反转（Inversion of Control）**和**门面模式（Facade）**，实际还有 **服务提供器（Service Providers）**和**别名（Alias）**，我们创建自己的类库和扩展 Laravel 都会方便很多。

这里总结一下创建自己类库的方法：

1. 在 `app/library/MyFoo` 下创建类 `MyFoo.php`
2. 在 `app/library/MyFoo/providers` 下创建 `MyFooServiceProvider.php`
3. 在 `app/library/MyFoo/facades` 下创建 `MyFooFacade.php`
4. 在 `app/config/app.php` 中添加 `providers`  和 `aliases`

