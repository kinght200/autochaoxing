# First Of All

想sk的小伙伴请先考虑该脚本是否**适合**你，**请认真看README中的功能支持项。**

脚本的初衷是致力于解决一些浪费大家时间的网课（chaoxing的禁止倍速和鼠标限制真滴很难受），所以**并不建议大家刷专业方向的课程，特别是因为今年的特殊形势而生的课程。**

关于sk，本项目其实给出了一份方案：**无界面脚本+查题程序**<br/>
（*查题程序查题范围不限于选择判断题，也不限于超星，查询结果依赖于多个查题接口*）<br/>
当然，**用户可以根据情况进行选择、和其他项目搭配使用。**

<br/>

## 提issue

- 执行遇到问题请先查看FAQ的相应项

- 如果仍未解决，提issue时建议附图：工作目录、运行界面、报错信息

- 尽量准确、详细地描述错误信息

<br/>

# FAQ

## How to run (v2.0)-怎样运行

- **模式选择**：`-m(--mode)`  

  -  默认single模式
  - `single`:      单课程自动模式——选择课程,自动完成该课程(默认启动参数，可不填写)
  - `fullauto`:  全自动模式——自动遍历全部课程,无需输入
  - `control`:    单课程控制模式——选择课程并选择控制章节,自动完成选定章节前的任务点

- **视频倍速**：`-r(--rate)`   

  - 默认1倍速
  - [0.625,16]​   全局倍速设置——在选定模式的全局范围内开启该倍速

- **答题设置**：`-n(--num)`  

  - 默认值：5                              可选值：0，1，2 ......
  - 自动答题时,如果 *未找到答案的题目数量* 达到`num`值,则**暂时保存答案,不进行自动提交**

- 实例：

  ![image-20200403014324816](https://github.com/Luoofan/Images/tree/master/AutoCX/image-20200403014324816.png)
  
- PS：Linux环境下：请用python3执行<br/>
              Windows环境下：请把python3环境配置为系统默认python，或者更改代码执行

<br/>



## About Query-Questions-关于查题

- 脚本使用多个`API`接口查题，正确率取决于接口题库

- **查题逻辑**：找到第一个脚本认为的合理答案即停止，不再访问其他查题接口

- 用户可自定义：
  - 自动提交限制：参见运行参数 **-n(--num)**
    - eg：`-n 2`：表示如果有**两个题目未找到答案**，该份章节测试将选择**暂时保存答案**而不是自动提交
  - `API`查询优先级：基于查题逻辑，用户可自定义`API`查询顺序
    - 方法：打开`src`目录下`queryans.py`文件，找到`16-22`行的`api_priority字典`按注释提示**修改对应接口的取值**即可
  
- ####  我想只刷视频不答题怎么办？

  请编辑`src`目录下`singlecourse.py`文件中第57行：`self._que_server_flag=1`，将1改为0即可不进行答题

<br/>



## About chrome&chromedriver-该如何选择版本

- 要把`chromedriver.exe`放在 **外层目录(与autocx.py同级目录)** 下

- 必须是**可执行**的chromedriver，如果双击exe不可执行，说明环境没搭好

- 需要注意的是版本要对应：chromedriverV2.9之前的版本可以进notes.txt查看对应chrome版本，之后的70及以上到80都是直接和chrome对应的

- **版本号前三个数应一致，第四个可以更换着尝试**，尽量使用chrome最新版

- Linux用户请自行安装chromium或者chrome以及匹配的chromedriver，当然**更推荐直接使用`docker`**

  

<br/>



## About login info-登录信息怎样正确填写

- 在`logindata_phone.txt或logindata.txt`中按提示填写登录信息，并把提示信息删除

  - `logindata.txt:`——[其实就是这里的登录信息](https://passport2.chaoxing.com/login?refer=http://i.mooc.chaoxing.com)
    - 第一行填写机构**全称**
    - 第二行填写手机号或学号
    - 第三行填写登录密码
  - `logindata_phone.txt:`——[这个的登录信息在这里](https://passport2.chaoxing.com/wlogin)
    - 第一行填写手机号
    - 第二行填写登录密码
  - 可填写多个账号，也可一个账号写多个（自己注意）

- **编码问题**：

  - 怎样判断是编码的问题？

    - 程序运行时会输出`check XXXXinfo: `，这个时候检查输出的信息是否带有特殊符号

  - txt需要是**utf-8编码**，若不是，可另存为->选择编码->覆盖原文件

  - 有的windows默认的utf-8编码其实是`utf-8 BOM`编码，如果是这种情况，可按如下方式修改`autocx.py`:

    ![image-20200403015518283](https://github.com/Luoofan/Images/tree/master/AutoCX/image-20200403015518283.png)

    **怎样判断是带BOM的编码**：如果你的第一个check的信息最前面是个小方块，那很有可能是utf-8 BOM

<br/>



## About Files after run-脚本运行后会生成什么文件

程序在运行一次后，可能会在当前目录下生成以下文件：

1. `login_vercode.png`：登录时需要输入的验证码图片，会自动弹出，记住验证码后关闭，在执行窗口填写即可(docker下直接显示在终端)**(手机登录方式不需要验证码)**
3. `ans_vercode.png`：答章节测试题时需要确认提交的验证码 **(几乎不会弹出)**
3. docker下会生成`AccountInfo`目录，目录下有针对账号生成的刷课记录文件

<br/>



## About Exit-关于中止程序运行

- **请以正常方式退出程序  或者 `ctrl+C`方式中断程序运行**
- windows环境下尽量不要直接关闭命令行窗口(`cmd`)，会导致chrome后台进程残留<br/>
  如果直接关闭了cmd，并且想终止残留进程，可以：
  
  - 命令行窗口执行：`taskkill /F /IM chrome.exe`  来关闭**所有的chrome进程**（注意是所有的）

  - 任务管理器找到后台chrome进程关闭
  
    （当然，如果对几十M内存没特殊要求的话， 完全可以忽略）

 <br/>



## About Docker-关于Docker版

- 具体使用教程请[移步项目地址](https://hub.docker.com/r/kimjungwha/autocx)
- docker版本下通过**进程之间的通信**实现了在**无界面单终端**下的**多开刷课**，用户需按序填写每个账号的信息 (`fullauto`模式不必输入) ，然后每个账号的`sk`将在后台进行
- 这样的模式导致的**交互性**远远不如有界面下的执行<br/>
  用户需要在当前目录的`AccountInfo`目录下找到相应的账号文件查看`sk`过程及结果<br/>
  查看进度及结果时：推荐**使用`cat`命令查看**
- 当然，选用这种模式不仅成功实现了单终端无界面下的多开，更棒的是**不必担心ssh不稳定断开所带来的程序中断**，换句话说，只要您的服务器(`or your pc`)没有出现异常，`sk`将持续进行下去直到完成它的任务。

<br/>



## About requests-关于访问请求

- 在完成章节的测试的时候，会发送课程和题目信息到题库服务器
- 如果程序运行出现异常，会发送报错(traceback……error……)到服务器，以便更快地debug，来给大家提供更好的体验~~
- 可以检查`query_ans(type, question)`和`send_err(err_info)`函数来调整您发送的信息

<br/>



## How to develop？-怎样开发

- 如果想知道代码做了些什么，可以在`source_code\login_courses.py`中第44行（查找`headless`所在行）前加`#`注释，再次运行会展示浏览器窗口
- 代码经过简单重构后已经 **将功能封装**，可以参照注释直接使用or开发
- 大家有新的想法可以提`issue`，改进或者完成的新功能欢迎`pull request`

<br/>



# UPDATE DETAILS

- 2020-5-30:

  - 修复了docker下多开任务只能完成第一个的bug

- 2020-5-15:

  - 修复了部分未查到题目空选并提交的bug

- 2020-5-13：

  - 修复了部分win10不支持chrome启动选项(`single-process`)的bug

- 2020-5-7:

  - 提高了`GreasyFork组`查题接口的准确率
  
- 2020-5-6:

  - 提高了原`SearchAns`查题接口的准确率
  - 取消一个失效的查题接口，修复一个更改的查题接口
  - 更新`SearchAns.exe`

- 2020-5-4：

  - 修复了docker版本下chromium进程残留的bug
  - 修复了查题程序结果显示不完整的bug,更新了查题程序使用的查题接口
  - 发布查题程序(`exe`)  [通道](https://github.com/Luoofan/autochaoxing/releases)

- 2020-4-30：

  - 修复了`Ctrl-C`方式中断程序导致的chrome后台进程残留的bug

- 2020-4-29：

  - 优化 获取未完成任务点 模块
  - 支持**ppt、音频任务点**
  
- 2020-4-28:

  - **更改项目目录结构**
    - 外层：登录信息文本(txt)、主执行文件(autocx.py)、chrome驱动(chromedriver)
    - 其他代码细节放在src源代码文件夹中
  - 合并docker与win版本，文件上不再区分
  - 减少不答题情况下的等待时间（用户可自行设定是否进行答题）

- 2020-4-24：

  - 新增**全局答题设置**选项`-n(--num)`  默认值为5
    - 可选值：0，1，2 ......
    - 自动答题时,如果 未找到答案的题目数量 达到num值,则暂时保存答案,不进行自动提交
  - 查题接口+1
  - 支持**自定义查题API优先级** 
  - 修复了遇到【非选择判断类】题目空提交的bug，改为跳过这类题目继续执行

- 2020-4-23：

  - 脚本答题功能恢复，请使用最新脚本（exe暂时仍无法使用）
  - **封装答题功能**，原来**单题库变为多题库**，答题正确率依赖于题库。

- 2020-4-18：

  - 题库服务器停止维护并暂时关闭，脚本目前将不再进行自动答题

    **注意：未更新exe，使用exe会导致答题错误，勿使用**

- 2020-4-7：

  - 上传2.0版win10x64打包程序,[通道](https://github.com/Luoofan/autochaoxing/releases)

- 2020-4-6：

  - **发布了Docker2.0版本**（有docker的小伙伴可以直接在docker里多开sk啦）

- **2020-4-2**：:star:

  - **发布了2.0版本**
  - 新增**模式选择**：`-m(--mode)`
    - `single`:      单课程自动模式——选择课程,自动完成该课程(默认启动参数，可不填写)
    - `fullauto`:  全自动模式——自动遍历全部课程,无需输入
    - `control`:    单课程控制模式——选择课程并选择控制章节,自动完成选定章节前的任务点
  - 新增**视频倍速**：`-r(--rate)`  默认1倍速
    - [0.625,16]   全局倍速设置——在选定模式的全局范围内开启该倍速
  - 代码简单**重构**，执行**优化**：将原有功能封装，想*亲自写脚本*的童鞋可以关注这点哦 :point_left:
  - 提高**容错率**(遇到未完成的任务点会暂时跳过,登录异常采用备用登录方案)
  - 更改原播放视频部分的模拟操作为js操作，提高程序运行稳定性
  - 可以通过 `-h(--help)`选项查看帮助信息，`-v(--version)`选项查看版本信息
  - 运行异常提交服务器—以便尽快debug
  - 分支合并到`master`，**执行文件更改为`autocx.py`** (以后只会增加参数，不会变更主执行文件)

  -------------------------------------------------------------------------------------------------------------------------------------

- 2020-3-22:

  - **multi_autocx**分支下新增了**手机号登录**模式，无需输入验证码即可登录，推荐使用该方式
  - 整理了项目文件结构，工作目录调整到**source_codes**
  - 修复了同页面内多项章节测试无法完成的bug、修复了输出信息颜色显示不稳定的bug

- 2020-3-21：

  - 添加了分支**multi_autocx**，可以方便地**多开**刷课（同ip）
    - 在`logindata.txt`中每三行填写一份账户信息
    - 运行`python multi_autocx.py`按提示操作即可

  - 更改了登录和获取课程的模式，**减少了等待时间**，原来的模式保留作为备用方案
  - 修复了其他任务点影响视频任务点无法执行的bug，修复了部分视频无法获取的bug

- 2020-3-16:

  - 由[KimJungWha](https://github.com/KimJungWha)制作了**Docker版本**，并发布到了[DockerHub](https://hub.docker.com/r/kimjungwha/autocx)

- 2020-3-15：

  - 增加了短时间内多次答题的时间限制，**减少答题验证码的弹出**
  - 修复了部分未完成任务点无法获取的bug
  - 新增了在**无图形界面的linux终端**下运行的脚本，需要工作目录下有`viu`，[viu:终端显示图片](https://github.com/atanunq/viu)
  - 发布了win10x64下的打包程序1.2

- 2020-3-13：

  - 新增了**查题程序**，使用的服务器与脚本自动答题所使用的不同，可以在题目输入不完整时搜索答案，但不能保证服务器始终有效
  
- 2020-3-11：
  - **规范了查题接口的使用**
  - 删去了查题程序，如果有查题需要可以移步*题库与考试下的链接*
  - 修复了程序在linux编码错误和执行路径错误的bug
  - 发布了win10x64下的打包程序1.1
  
- 2020-3-10：

  - 修复了部分页面无法获取课程的bug、修复了普通章节下的子章节无法获取的bug
  
- 2020-3-9：

  - 修复了部分视频检测错误的bug、修复了有些页面无法打开视频页面和章节测试的bug
  - 新增了查题程序，分命令行执行和窗口执行两种，配套刷课脚本用来考试查询
  - 发布了win10x64下的打包程序1.0，可直接运行exe开始刷课
  
- 2020-3-5：
  
  - 发布1.0版本
