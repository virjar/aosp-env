# 基本介绍
这是一个AOSP的环境母版本,包括Google官方的AOSP源码，制作时间为2022年04月10日。如果你想编译aosp源码，强烈建议你直接使用下面的资源直接编译，将会给你减少大量下载带宽、环境配置时间

当前时间下，拥有如下环境:
1. 源码跨度为：Android首次发布到完整的android13的所有版本源码
2. 使用Ubuntu 20.4 作为主机环境，并完成基础软件安装。
3. 基础的编译环境：在后续编译过程确认源码可以直接编译通过
4. Repo等源码同步工具:,为了提升速度，以及避免网络导致的错误，我们使用清华源替代了Google的连接，见： https://mirrors.tuna.tsinghua.edu.cn/help/AOSP/
5. Android Studio、sublime、Clion（完成破解）


本环境基于VMware 专业版 12.1.2 (17964953)，你可以在Window或者Mac上安装WMware（请注意需要是12.1.2或者之上的版本）

## 基础配置

- 虚拟机配置: 500G的磁盘（稀疏磁盘），实际占用大多为150G之内
- 虚拟机登录的root账户密码: 0516
- WMware版本： 12.1.2 (17964953)
- 其他： 虚拟机设置了12核、64G内存，但是一般的电脑不会有这么高的配置。所以建议你拿到虚拟机文件之后，根据自己的电脑配置适当调整一下


## ROM列表 [下载地址](https://pan.baidu.com/s/1DGcxPilfmTDb_hJzXJ9uHg?pwd=sj00)
- Base: 主要包括虚拟机文件和基础的repo数据，可以方便切换分支。你可以通过他快速拉去任意想要编译的代码环境，本环境可以帮你省略虚拟机环境搭建、代码下载同步、软件安装等操作
- 已编译：已经完成编译，可以直接增量使用，或者进行刷机
- 未编译：编译成功后删除了out，确保可以一键编译成功。由于删除了out，所以文件会小40%

 **请注意：由于文件很大，下载过程可能容易发生文件传输错误，所以我提供了每个文件的md5值，如果下载后文件无法使用，请检查md5是否正确，并重新下载md5错误的文件即可**

|手机型号|代号|os版本|是否编译|压缩前|大小|文件名称|制作时间|
|---|---|---|---|---|---|---|---|
|aosp-all|包含仓库的代码环境|All|否|180G|150G|aosp_base_220410                     |2022-04-10|
|pixel1  |sailfish       |10 |否| -  |23G |10.0.0_r17_sailfish_uncompiled_220410|2022-04-10|
|pixel1  |sailfish       |10 |是| -  |31G |10.0.0_r17_sailfish_compiled_220409  |2022-04-10|
|pixel4  |flame          |11 |否| -  |27G |11.0.0_r46_flame_uncompiled_220410   |2021-08-04|
|pixel4  |flame          |11 |否| -  |43G |11.0.0_r46_flame_compiled_220410     |2021-08-04|


## 用法

### baseEnv
为了让镜像更加精简，本环境删除了AOSP的捡出代码,所以你打开桌面的AOSP将会看到是一个空文件夹。但是AOSP中其实存在180G的隐藏文件，
这个隐藏文件即为Android全量代码的仓库文件:(AOSP使用git管理，这里是git的提交仓库文件，由于仓库记录了每次代码的变化，所以任何版本的代码都可以通过git命令直接释放出来)

所以请不要删除 ~/Desktop/aosp文件夹下的内容。他是基础数据

1. 切换到你的分支，并且同步一次最新代码：
    1. 这里假定你想编译 android-10.0.0_r17
    2. 打开命令行，并切换到文件夹： cd ~/Desktop/aosp
    3.  repo init -u https://mirrors.tuna.tsinghua.edu.cn/git/AOSP/platform/manifest -b android-10.0.0_r17
    4.  repo sync
    5. 等待执行完成，中途可能报错，报错原因一般是网络问题。重试一两次就可以。如果在2022年4月10日完成了首次全量同步，故今后每次下载的都是增量
    6. 特殊说明，如果您现在的时间距离本镜像制作时间太久（2022年4月10日），出现了当前环境没有的分支，比如android 13。那么先执行一次 repo sync 再切换分支即可
2. 确定分支和下载驱动
    1. 您的手机，需要做在这里查询支持的Android版本，和对应分支: https://source.android.com/setup/start/build-numbers#source-code-tags-and-builds
    2. 下载确定分支之后，需要在这里下载对应的驱动文件: https://developers.google.com/android/drivers#sailfishqp1a.191005.007.a3
    3. 切换完成分支之后，将两个驱动文件解压，然后放置到AOSP目录，执行两个脚本。提取驱动二进制文件
        1. 请注意，需要同意Google的条款:执行脚本,进入条款阅读状态后，输入 `ctr +c `，然后输入: “I ACCEPT”字样，然后提输入“I ACCEPT”，回车。
        2. 之后现实提取了那些文件，代表提取成功
        3. 如果无法成功定位到:“I ACCEPT”的位置，那么可以手动修改对应文件,里面是一个sh，可以直接删除前面的文本，然后修改以下shell的行号偏移内容即可
3. 据根据Google文档，执行编译操作 https://source.android.com/setup/build/building
    1. 打开命令行，切换到： cd ~/Desktop/aosp
    2. 导入基础环境变量: source build/envsetup.sh
    3. 定ROM ：
        1. lunch 查看支持的rom，
        2. lunch aosp_sailfish-eng 启动Pixel一代，具有额外调试工具的开发配置类型rom
    4. 行 执行make命令
        1. make 开始编译
        2. make -j 16 确定编译线程数量
        3. m是make的替代符号，输入m也可以进行编译
4. 提取编译后的产出ROM，或者直接连接ADB进行刷机

### isolateEnv
请根据自己手机找到自己的版本，下载对应虚拟机文件，直接make即可


# 源码阅读
搭建这个环境的目的是因为我的工作需要比较深入的研究Android源码，作为java程序员出身我习惯了IDE提供的代码穿梭功能。Android的在线aosp源码查看网站用起来确实特别垃圾

- 索引紊乱：xref就是根据符号生成的索引，就是一个搜索引擎。关联代码的关联关系不一定是真的。所以当我们搜索一些很常见关键字的时候会遇到大量的搜索结果
- 底层代码无法索引：当遇到一些代码来自Android的高度优化（如插桩、汇编跳板等），大概率找不到实现类
- 只能看，无法增加注释。导致无法回溯无法快速找到之前标记的内容
- 串行阅读，只能有一个窗口，且大文件加载会很慢。没有IDE的快捷键
- 无法修改源码验证

在Android10之后，Google提供的``aidegen``已经非常可靠了，通过他我们可以非常方面的通过ide直接查看、编辑代码。基本上做到和查看普通项目代码相同的流畅度了。
请关注Google文档：[其他工具](https://source.android.com/setup/develop?hl=zh-cn)对于AIDEgen的指导:``对于 Android 10 及更高版本，请使用 IntelliJ with AIDEgen IDE 进行 Android 平台开发``,官方教程：[AIDEGen](https://android.googlesource.com/platform/tools/asuite/+/refs/heads/master/aidegen/README.md)

**请注意，在Android11之后``aidegen``才完整支持clion，所以如果你希望查看c++代码，建议在Android11的源码上面操作**

## 案例：使用clion查看art虚拟机的源码(以``11.0.0_r46_flame_compiled_220410``为例)

1. lunch: cd ~/Desktop/aosp & source build/envsetup.sh && lunch aosp_flame-eng
2. 切换当前目录到art目录： cd art
3. 启动clion : aidegen -i c -p /opt/clion-2020.2/bin/
4. 等待aidegen打开clion即可

**请注意我们的软件默认安装在 ``/opt``目录下，这里的 ``-p``参数指定了clion的目录**
**请注意clion是一个c++的IDE，和Android Studio类似，但是他是收费的，为了降低大家的使用难度和方便环境传播，我们这里使用了破解版本的clion，希望大家在有条件的情况下支持正版**



# VMware 优化原理
我们使用VMware文件产生虚拟机文件，为了使VM在传输的时候达到最小状态，我们做了很多策略。

- 删除仓库文件： 删除``.repo/``和``.git/``文件夹，以此减少一百G的实际存储文件
- 仅保留``.repo/``文件夹：对于base环境来说，除了必备软件以外，对于repo不能释放真实文件，这会导致磁盘文件大小直接double
- 磁盘整理：这个非常重要，为了使得我们的VMware磁盘文件更小，在创建磁盘镜像的时候，我们会选择稀疏磁盘。这样没有真实存储数据的磁盘区块并不会真实占用文件大小。但是当我们在系统上面进行一些操作（产生大文件和新文件）之后。稀疏磁盘将会被撑大。而我们制作镜像的时候，大概率会有这些操作的。此时为了让稀疏磁盘变成最初很小的样子，需要进行磁盘整理
    - 快照分离
    - 安装 vmware-tools
    - 进行磁盘整理: ``sudo vmware-toolbox-cmd disk shrink /``
    - 重启
- 快照克隆：VMware快照功能使得我们可以进行各种快照穿梭，但是也会导致文件变大。当我们确定某个快照可以独立作为特定功能发布的时候，就让他从快照上面分离
- 压缩：就算完成了磁盘整理，也只是保证数据顺序排放而已。数据之间大概率只是会存在冗余。所以再此基础之上还是可以进行压缩。我们选择7z作为压缩工具
    - 跨平台，在windows上面的同学大概率会使用rar，但是rar作为收费协议，在mac上和Linux上面没有免费版本
    - 自动分割文件。考虑传输方便，如果有几十G的大文件，大文件传输容易出事儿。另外使用百度云，即使是vip也不允许上传超过100G的大文件。所以需要支持文件分割
    - 压缩效果强劲，在我们的场景下。降低传播文件大小是第一要务。对比zip，rar，7z。好无疑问7z的压缩效果最牛逼
