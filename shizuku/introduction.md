#介绍

滴可以帮助普通应用程序使用系统API直接与adb/root权限与一个Java进程开始与app_process。

滴这个名字来源于[一个角色](https://danbooru.donmai.us/posts/3553474).

##滴为什么会出生？

滴的诞生有两个主要目的。

1.提供使用系统API的便捷方式
2.方便一些只需要adb权限的app的开发

##滴对“老学校”的方法

###“老派”方法

例如，要启用/禁用组件，一些需要root权限的应用程序会执行`下午禁用`直接在`苏联(苏联的缩写)`.

1.执行`苏联(苏联的缩写)`
2.执行`pm禁用`
3.(pre-Pie)用app_process启动Java进程([看这里](https://android.googlesource.com/platform/frameworks/base/+/oreo-release/cmds/pm/pm))
4.(Pie+)执行本机程序`煤矿管理局` ([看这里](https://android.googlesource.com/platform/frameworks/native/+/pie-release/cmds/cmd/))
5.处理参数，通过binder与系统服务器交互，处理结果输出文本结果。

每一次“执行”都意味着一个新进程的创建，su内部使用套接字与su守护进程进行交互，这样的进程会消耗大量的时间和性能。(一些设计糟糕的应用程序甚至会执行`苏联（USSR的缩写）` **每次**对于每个命令)

The disadvantages of this type of method are:

1. **Extremely slow**
2. Need to process the text to get the result
3. Features are subject to available commands
4. Even if adb has sufficient permissions, the app requires root privileges to run

### Shizuku method

The Shizuku app will direct the user to run a process (Shizuku service process) using root or adb.

1. When the app process starts, the Shizuku service process sends the binder to the app process.
2. The app interacts with the Shizuku service through the binder, and the Shizuku service process interacts with the system server through the binder.

The advantages of Shizuku are:

1. Minimal extra time and performance consumption
2. It is almost identical to the direct invocation API experience (app developers only need to add a small amount of code)
