---
title: "Galaxy Watch 折腾记"
author: "2419 沈泽厚"
author_display: "2419 沈泽厚"
create_date: "2025-12-XX"
status: "已见刊"
note: "AI参与指数2"
---

### 零、概述

鉴于高中学生的电子产品现状，我一直梦想着入手一块智能手表，在监测健康功能的同时为「平淡」的生活增添一些乐趣，但又苦于「穷困潦倒」的境地，没能选择Apple Watch Series 10（当时44 mm表壳＋蜂窝网络在官方起步也要3999元）。最终只好选择了「性价比」款式——Samsung Galaxy Watch 7（44 mm + LTE），最终到手价约为1600元（时间为2025年寒假）。

当然，买之前我也是做了不少功课的，我将在下面罗列一下其初始参数配置（也是前面提到其「性价比」的缘由）：

- SoC: Exynos W1000
    - CPU: 4× Cortex-A55 @ 1.5 GHz, 1× Cortex-A78 @ 1.6 GHz
    - GPU: Mali-G68 MP2
    - Architecture: ARMv8-A (Model: 32 bit)
    - Process: Samsung 3 nm (SF3)
- RAM: 2 GB LPDDR5
- Storage: 32 GB eMMC 5.1
- Screen: Super AMOLED
    - Size: 1.5 inch
    - Resolution: 480 × 480 (327 ppi)
- Connect
    - Wi-Fi: 802.11.a/b/g/n (2.4 GHz + 5 GHz)
    - Bluetooth v5.3
    - Cellular Network: 4G LTE (eSIM)
    - GNSS: GPS (L1 + L5), GLONASS, BeiDou, Galileo
    - NFC
- Sensors
    - Samsung BioActive Sensor
    - Blood Pressure Monitor
    - SpO₂
    - Skin Temperature Sensor
    - Accelerometer
    - Gyroscope
    - Barometer
    - Geomagnetic Sensor
    - Ambient Light sensor
- Battery
    - Capacity: 425 mAh
    - Type: Li-ion
    - Charging: Qi 15 W
- Size & Weight
    - Dimensions: 44.4 × 44.4 × 9.7 mm
    - Weight: 33.8 g
- OS: Wear OS 5 (Android 14)

我改造这块手表的初衷很简单，那就是希望其在保持基本流畅运行度的前提下，尽量增强其的功能，起到一定程度上的代替手机的作用（同时享受折腾的乐趣）。为了行文与读者参考方便，我就在此先行列出最终折腾完成后实现的功能及其依赖的软件：

1. 不受限制的浏览网页（*Chrome*与*V2RayNG*）；
2. 即时通讯（*Telegram*、*TGWear*与*Element X*）；
3. 阅读文章（*Moon+ Reader*）；
4. 聆听音乐与播客（*NetEase Music*、*Apple Music*与*Pocket Casts*）；
5. 欣赏视频（《哔哩终端》）；
6. 一定的记录能力（*Material Notes*）；
7. 与LLM对话（*RikkaHub*、ChatGPT Web）；
8. 一定的独立配置能力（*APKMirror Installer*、*Shizuku*、*aShell*、*App Ops*、《抬腕文件》与*LocalSend*）；
9. 其他优化（*Gboard*、*AIDA64*）。

不过既然本文名为「折腾记」，那么我还是会大致讲一些我走过的弯路，不会完全按照教程的模式来写。不感兴趣的读者直接跳过并获取对自己有价值的参考信息即可。

### 一、开启开发者模式并进行初始配置

既然想要实现上述的功能，那么很明显，自由地安装App就是第一步了。为此，我们需要在一台Android设备上安装好《Wear OS 工具箱》（`wearosbox.com/zh`），并进行初始化配置。其他开关一路保持默认即可，在「向导」界面请打开「使用配对码连接设备」，「设备」界面请选择「Wear OS 4 及以上」。

这里解释一下「配对码」的来历：从Wear OS 4开始，Google不再使用旧版那种连上5555端口就能直接ADB的Wi-Fi调试方式，而是改成带TLS的无线调试流程，因此首次连接需要配对码授权。

随后需要在手表上开启「开发者模式」，具体请先进入「设置＞关于手表＞软件信息」界面，并连续点击「软件版本」数次，直到提示「已开启【开发者模式】」。在开启成功后，请到「设置＞开发者选项」中启用「ADB 调试」开关，随后进入「无线调试」菜单，并开启同名开关（此时请将手表与手机连接至同一个Wi-Fi，包括但不限于热点）。

接下来请点击「配对新设备」，在记录下「WLAN 配对码」「IP 地址和端口」后重新拿起手机，在主页的第一个文本框中按照提示输入设备地址与配对码。根据个人经验，此时需要重新将手表上的新「IP 地址和端口」重新输入文本框并连接，在重新提示「连接成功」后，初次连接就算成功了。请注意：这个连接十分不稳定，时不时会断联，断联后重连大概率不需要重输配对码，否则就需要看看自己人品是否有问题了😀（如果文本框内的设备地址还保留的话则直接再次点击「连接」即可重连）。

### 二、下载并安装浏览器

在下载*Chrome*之前，我们需要下载*APKMirror Installer*，这是因为其最新安装包已采用APKM格式分发，无法直接通过ADB安装。请在手机上访问`apkmirror.com/apk/apkmirror/apkmirror-installer-official`（需要科学的网络环境，例如通过旅行eSIM），向下滑动页面直到看见「All versions」后选中其中一个最新的不带beta字样的版本。跳转至新页面后请点击「SCROLL TO AVAILABLE DOWNLOADS」，随后请选择灰色的带有「APK」标识的选项。跳转到下一个页面后请点击「DOWNLOAD APK」，随后安装包便会下载到手机中。

另外，我们还需要*Shizuku*的支援：请访问`github.com/RikkaApps/Shizuku/releases`，在「Assets」中选择一个最新版本的`.apk`文件下载。随后回到《WearOS 工具箱》，在确保已连接手表的前提下，进入「功能」菜单，随后按顺序点击「安装 APK」「选择 APK」。在找到其中*APKMirror Installer*的安装包并选中后，点击「安装」即可，当「安装成功」的字样出现后，你便会在手表上看见*APKMirror Installer*了。随后请再重复一遍安装步骤，把*Shizuku*也一并装到手表上。以后的通过ADB的安装过程将不再赘述。

随后请访问`apkmirror.com/apk/google-inc/chrome`，在「All versions」部分选择一个不带beta字样的最新的版本，随后点击「SCROLL TO AVAILABLE DOWNLOADS」。在确保「Architecture」一栏中含有`armeabi-v7a`（本手表虽然SoC为64位架构，但实际上系统只支持32位，以后下载时不再赘述）后选择一个「Minimum Version」最新的选项（在撰文时其为`Android 12L+`）。页面跳转后你会发现一个「This APK requires a library」的提示，后面跟着一个*Trichrome Library*的超链接，点进去后再点击「DOWNLOAD APK」即可。随后回到*Chrome*下载的页面，点击「DOWNLOAD APK BUNDLE」，随后请耐心等待两个App的下载进程。在下载完成后，请回到《WearOS 工具箱》，在「功能」页面中选择「文件管理」。在此，你将会进入手表系统的文件目录。请找到「Download」文件夹并打开，随后点击右下角的三个点的按钮进入「更多操作」选单，在其中选择「上传文件」，随后把两个App的安装包上传进手表即可。

在此之后，我们回到手表上并打开*Shizuku*。请先下滑页面，在「通过无线调试启动」处找到「配对」按钮并点击，随后继续下滑，点击「开发者选项」按钮。在点击后，系统将自动跳转过去，随后你需要再次点开「无线调试」页面，选择「配对新设备」后记住配对码。接下来是比较关键的操作，请先不要叉掉页面，直接按下电源键回到主屏幕。随后左滑进入通知界面，在这里你会看到一条来自*Shizuku*的通知，点进去后在「信息」文本框中输入配对码并发送。随后回到*Shizuku*，点击「启动」按钮，如果闪过一串代码后回到主界面并显示「Shizuku 正在运行」后，其便配置完成了。在此之后，Shizuku服务将在无人工干预的情况下继续运行下去，直到手表重新启动，以后将不再赘述开启过程。

然后我们需要回到*APKMirror Installer*，在授予其必要权限后点击「Browse file」，随后请在「Download」目录中找到两个安装包。由于Android的设计特性（*Chrome*、WebView与Chromium内核相分离），你需要先安装*Trichrome Library*，随后再安装*Chrome*本体。值得注意的是：由于Wear OS的限制，我们无法安装*Android System WebView*，也就无法使用*Via*等不依赖WebView的超轻量浏览器（本来它还挺适合手表的）。

在这里我想解释一下选择*Chrome*的原因：首先就是*Chrome*实际上是对Wear OS进行了一定适配的，你在它的设置里尝试登录Google账号的话就会看到提示（当然，国行手表由于缺失Google服务是无法登录的）；其次，*Edge*等其他浏览器更加臃肿（不论是内核功能还是UI层面），这导致其在手表上的显示与运行时较为困难（当然一定要用的话也可以，不过很难用就是了）；随后就是前面讲过的，*Via*根本无法在手表上使用，*Firefox*也因为不明原因刚点开就会闪退；最后，一些所谓专门的手表浏览器（如《三星浏览器》个人认为根本不可用，因为其连基本的下载功能都不具备）。

### 三、浏览器周边配套

这一段涉及网络配置与系统权限。我的建议是：如果你的目标只是手表能「正常」上网、打开网页，那就尽量最小化改动；只有当你自己确定一定需要代理时继续往下折腾。

#### 1．DNS：最低成本、最小副作用

据我的实际经验，*Chrome*的默认DNS在中国大陆似乎有一些问题（如SNI阻断），如果你遇到了任何网页都无法打开的问题，请到其的「设置＞隐私与安全＞使用安全 DNS」中把「使用安全 DNS」的开关关掉。或者也可以手动选择国内还不错的DNS，在点击「另选一个提供商」后选择「自定义」，随后在「提供商网址」文本框中输入`114.114.114.114`即可）。当然，这类设置实际上相对有玄学成分，如果行不通请自行摸索。

#### 2．*aShell*：调整UI分辨率及其他妙用

*aShell*是一个使用*Shizuku*驱动的安卓设备的本地ADB终端，这样我们就可以在手表上自己修改分辨率而不依赖手机调试。至于为什么要修改分辨率，因为手表屏幕是圆形的，但Android系统告诉App的逻辑分辨率仍然为480 × 480。因此，如果App没有主动适配圆屏，一大部分的界面都会无法看见与点击。而如果将分辨率调整为480 × 900（适合竖屏程序，如*Apple Music*）或600 × 600（适合只是单纯字太小的界面，如*RikkaHub*），虽然会使得逻辑分辨率与物理分辨率不匹配或出现黑边的问题，但你至少能点击到全部的按钮了😃。

截至撰文时，*aShell*的最新版本是v0.23，但是据我实测，这个版本似乎有些比较影响（在手表上的）使用体验的Bug。因此，如果你看到这篇文章的时候在`f-droid.org/zh_hans/packages/in.sunilpaulmathew.ashell`上没有看到更加新的版本，请优先使用v0.17版本。在将其下载并安装到手表上后，请将其打开，在「aShell 指令框」中输入`wm size 480x900` 后按下文本框内的「☆」收藏这条命令，随后确认，你就会发现手表只剩中间一部分能显示内容了。想要恢复，输入（也建议收藏）`wm size reset`或`wm size 480x480`即可。当然，想要其他分辨率也一样，你可以自己多试试适合你自己App的最佳分辨率，不过乱改手表可能会变砖😄，实在不放心你也可以提前接一个蓝牙键鼠。在修改适合的分辨率后，理论上看不见的UI也就能看见了，如果看不见，那就再改一次。

#### 3．Proxy配置（手表上较麻烦）

3.1．Wear OS 5

在更新Wear OS 6之前，我使用的是*Clash Meta for Android*，它UI简洁、运行流畅，让人心情愉悦。如果你还没升级的话强烈建议你选择它（而且手机上也能用），下载地址为`github.com/MetaCubeX/ClashMetaForAndroid/releases`，随后请效仿上面的步骤将App下载并安装到手表上。

很多人第一次在Wear OS上跑此类App，都会遇到「点了启动后闪退」的问题。背后的原因是：系统对VPN的关键能力做了App-Ops（应用操作）层面的控制，其中包括：

- `ACTIVATE_VPN`：无用户交互激活VPN（默认模式是`MODE_IGNORED`，也就是「忽略」）；
- `ESTABLISH_VPN_SERVICE`：通过`VpnService`建立VPN连接。

这就解释了我为什么要引入*App Ops*：它能配合*Shizuku*，把这些默认不给第三方App的权限临时放开。下载链接：`github.com/RikkaApps/App-Ops-issue-tracker/releases`。

在将其安装至手表上后，请保证Shizuku服务正在运行，随后点击右上角的「🔍」，搜索「Clash Meta」即可找到结果。在进入*Clash Meta for Android*的页面之后，请将「ESTABLISH_VPN_SERVICE」`ESTABLISH_VPN_SERVICE`与「激活 VPN」`ACTIVATE_VPN`都设为「允许」。

设置好*App Ops*后你需要复制好服务商提供的配置URL（可以利用*Chrome*。当然，你想手动输入也可以），随后将*Clash*打开，并点击「配置＞􀅼＞URL」，然后请将剪贴板中的URL粘贴到对应的文本框中。至于「名称」，你可以看心情自己选择，不填当然也可以；「自动更新」的话，这个相对更有用些，开启后会自动更新你的代理配置（可以同步剩余流量），你想开的话建议设成1440分钟（下同）。在设置好之后，请点击「💾」保存配置。回到首页后点击「已停止」即可成功启动代理，如果想关闭，点击「运行中」就行了。至于更改代理节点选择，请在「代理」页面中自行探索。

3.2．Wear OS 6

但在更新Wear OS 6以后，以往的依赖`VpnService`的方案因各种原因失效了（个人认为最大可能是App Ops太久未更新，已不太兼容Android 16了）。于是，我们只好用*v2RayNG*（下载链接：`github.com/2dust/v2rayNG/releases`）走「仅代理」模式。具体操作方法如下（记得先拷贝订阅链接）：

在进入*v2RayNG*之后请点击左上角的三条横线图标，随后在弹出的菜单中选择「设置」选项。往下滑动菜单，请勾选「允许来自局域网的连接」「订阅分组设置＞自动更新订阅」，并将模式改为「仅代理」。回到主菜单后请按「􀅼＞从剪贴板导入」，随后点击多出来的「import sub」按钮，你就可以选择一个合适的代理节点了。

此外，这条路线依赖ADB（*aShell*）写入系统代理。在默认端口（目前为10808，具体请以实际为准）下的启用代码如下（建议收藏，下同）：

```shell
adb shell settings put global http_proxy 127.0.0.1:10808
```

关闭代码如下（请按顺序执行）：

```shell
adb shell settings put global http_proxy :0
adb shell settings delete global http_proxy
```

补充说明：如果忘记关掉系统代理，*Chrome*等App的网络都会因本地代理服务器消失而无法工作；另外，该方案对于不遵循系统代理的App不生效，但是对于本文目标网页浏览来说已经足够。如果你已习惯手机或老版本系统上的傻瓜体验，那么该路线天然存在上限。当然，最好的结果当然是你手上的或将要购买的手表是老版本系统。

### 四、单App功能

在最重要的浏览器配置完后，我们的手表的可用性就已经很高了。在这一部分，我将叙述一些简单的单App即可完成配置的功能。

#### 《哔哩终端》

它是一款专为智能手表与低配置设备优化的第三方哔哩哔哩客户端，官网提供下载入口，并且有对应开源仓库可查。其功能众多并且UI完全适配手表。官网：`biliterminal.cn/#download-section`。

#### *Telegram*

- 路线A：*Telegram for Android*

    优点是功能全、更新快；缺点是UI明显为手机设计，经常需要用*aShell*调整分辨率。注意，其在Android上提供的官方版本分成两个，一个是Google Play版，另一个是官网版本。前者的通知推送依赖FCM，应用内购依赖GMS，虽然更加省电但是显然国行手表上用不了；后者则是官网版本，不依赖任何其他组件，我建议使用这个版本。其他第三方客户端如果各位有需要可自行尝试。

    下载链接：`telegram.org/dl/android/apk`；

    本人使用的中文语言包：`t.me/setlanguage/zh-hans-beta`。

- 路线B：*TGWear*

    *TGWear*是专门为Wear OS设计的版本，UI更加适配手表，但缺点就是功能缺失、更新速度慢。本人建议可以把A、B都装在手表上（32 GB正常应该绰绰有余）。

    下载链接：`github.com/TGwear/TGwear/releases`。

- 路线C：Telegram Web

    全平台通用，但个人没在手表上用过，因为手表上打开网页操作较为麻烦，而且个人该App的使用频率蛮高的。如果你的手表容量告急或者不怎么用*Telegram*的话建议选择该路线。

    网址：`web.telegram.org`。

#### *Element X*

*Element X*是一个以Matrix协议为基础的去中心化的开源即时通讯软件。其支持端到端加密，甚至不需要用手机号注册。如果你被*Telegram*的SMS Fee困住了，或许你可以试一下它，二者基本社交功能十分相近。我个人在上面加入了一些技术群组，如*Breezy Weather*。

下载链接：`f-droid.org/packages/io.element.android.x`。

#### *Moon+ Reader*

在纸质书昂贵且有被没收风险的情况下，能在腕上阅读电子书就显得尤为可贵了。*Moon+ Reader*（静读天下）是一款功能强大且高度可自定义的非常好用的Android电子书阅读器，支持EPUB、MOBI、PDF、TXT等多种格式。它以极高的自由度著称，让使用者能精细调整字型、间距、背景、翻页特效及手势控制。建议搭配后文的*LocalSend*，把各种文件丢进手表，然后让它直接从 `Download` 等目录打开即可。

但其实这款软件最大的问题就是它不是开源软件，甚至不是免费软件。这意味着安装包要到APKMirror下载，免费版不是全功能的，但最大的问题还是其十分依赖GMS（如广告、付费），这意味着我们必须忍受它经常的闪退。虽说至少正常阅读的时候不会闪退，且重进一下就能解决问题，但总归让人有些恼怒。或许你也可以使用开源的*KOReader*（下载链接：`github.com/koreader/koreader/releases`）来起到同样的用途，但是据我个人体验，它的UI比较简陋，可能不太适合在手表上使用。

下载链接：`apkmirror.com/apk/moon/moon-reader`。

#### *Material Notes*

这是一款基于Material Design（Android原生）风格打造的一款轻量的笔记软件，我之所以选择它的理由是它不仅支持标准的纯文本写作，还支持Markdown格式与（某种够用的）富文本格式，这使它的可迁移性与生态协作能力更上一层楼。虽然它不支持直接同步，但是这种软件一般都不太契合手表的操作与网络条件。

#### *RikkaHub*

这是一款同样基于Material Design的LLM聊天客户端，功能强大，支持切换不同的供应商与LLM进行聊天。为了使用该App，你需要一个提供商的API Key。有条件的话，你当然可以直接去OpenAI或Google Cloud拿Key；而我用的是OpenRouter，它甚至支持用支付宝充值余额；Google AI Studio也有免费的可用，但是截止写作时，它提供的最好的模型还是Gemini 3 Flash，且RPM只有5，TPM只有250K，RPD只给了20。小提示，*RikkaHub*支持独立配置系统代理，建议你照上面的方法配置。

下载链接：`github.com/rikkahub/rikkahub/releases`；

官方文档：`docs.rikka-ai.com/zh/docs/basic/get-started.html`；

OpenRouter：`openrouter.ai`；

Google AI Studio：`aistudio.google.com`。

#### *LocalSend*

这是一款开源的跨平台文件共享软件，无需连接互联网。这是我主要用来在手机与手表上传输文件的方式（如录音机录制的M4A文件），当然，我也经常用它在我的Android手机与iPhone之间传输文件。

有趣的是，因为Wear OS阉割`DocumentsUI`模块（*Files*）的缘故，许多调用它的App（如*LocalSend*、*Telegram*）将无法发送本地文件。所以，目前我使用的方案是可以通过*Moon+ Reader*自带的文件浏览功能来「转发到」这些App中。

下载链接（官网）：`localsend.org/zh-cn/download`；

下载链接（GitHub）：`github.com/localsend/localsend/releases`。

#### 《洋葱商店》

这是一款手表上的App Store，主要用来下载一些手表版本的国产软件。

下载链接：`onionapk.com/store`。

#### 《网易云音乐》

罕见的，《网易云音乐》竟然在这一「小众」平台上提供了十分完善的功能支持，如每日推荐、搜索、收藏夹、听歌识曲等，就是播客听不了。你可以在《洋葱商店》里获取它。

#### 《抬腕文件》

这是一款手表上使用的较为简陋的文件管理App。说它简陋，是因为它连「分享」功能都没有，导致我还得用上文提到过的*Moon+ Reader*，但有时候还是能派上用场的。你可以在《洋葱商店》里获取它。值得注意的是，Google官方的*Files*（`apkmirror.com/apk/google-inc/files-google`）经过我的尝试，无法在手表上使用。

#### *Gboard (Wear OS)*

由于手表自带的《三星输入法》只支持九键，且很多中文标点符号给的是半角的缘故，我会再选择安装一个*Gboard*，根据使用场景不同来灵活切换。在安装后你可以在《设置》里将它设定为默认法输入法。值得注意的是，它在分辨率修改的工况下比《三星输入法》更敏感、更容易失效，这个时候就只能通过重启来解决了（反正我是没找到更方便快捷的解决方案）。

下载链接：`apkmirror.com/apk/google-inc/gboard-the-google-keyboard-android-wear`。

#### *Apple Music*

*Apple Music*自从我初一开始使用iPod touch (7th gen)就成为了我的主力音乐软件，因此，我自然想把它的Android版装到手表里。据我所知，它有国内应用商店与Google Play两个发行渠道，二者App本体相同，就是更新节奏与策略不太一样——后者更快，而且提供Beta版本——读者们可以自行选择。

下载链接（Google Play）：`apkmirror.com/apk/apple/apple-music`；

下载链接（应用宝）：`sj.qq.com/appdetail/com.apple.android.music`；

Apple Support：`support.apple.com/zh-cn/109340`。

#### *Pocket Casts*

因为一些尚且不明的原因，以*AntennaPod*为代表的一些简洁、开源的播客App会在手表上莫名地闪退，我只好退而求其之，选择一个相对臃肿的商业软件。有趣的是，其实它提供了Wear OS版本，但是需要额外付费订阅。

下载链接（标准版）：`apkmirror.com/apk/automattic-inc/pocket-casts`；

下载链接（Wear OS版）：`apkmirror.com/apk/automattic-inc/pocket-casts-podcast-player-wear-os`。

#### *AIDA64*

之所以把它放在最后面，是因为这玩意其实没什么用，就是我手表刚买来用来看配置的。看完后最大的用途可能就是在没网络的时候可以看看疯狂跳动的传感器数值。提示：它有Wear OS版，但据说功能有阉割，请各位酌情选择。

下载链接（官网）：`aida64.com/aida64-android`；

下载链接（APKMirror）：`apkmirror.com/apk/finalwire-ltd/aida64`；

下载链接（APKMirror的Wear OS版）：`apkmirror.com/apk/finalwire-ltd/aida64-wear-os`。

### 结语

折腾到这里，这块Galaxy Watch 7当然还远远称不上是一台真正意义上的「腕上手机」——受限于屏幕、输入方式、系统权限与软件适配，它终究无法彻底取代手机，更不可能在所有场景下都提供良好的体验。很多时候，它仍然只是一个需要不断妥协、不断试错、不断寻找替代方案的小设备。

但换个角度看，折腾的意义本来也不在于「完美替代」，而在于尽可能逼近自己想要的状态：让一块原本主要用于通知、计步与健康监测的智能手表，逐渐获得浏览网页、即时通讯、阅读文本、听音乐、记笔记，乃至一定程度上独立配置自身的能力。它未必强大，却已经不再只是一个被动接受生态安排的消费品，而开始成为一件真正可以被个人意志塑造的工具。

更重要的是，这种折腾本身也带来了一种相当具体的乐趣。你会在一次次安装失败、权限冲突、界面错位与连接断开的过程中，被迫理解Android、Wear OS与各种应用生态那些原本隐藏在「正常使用」背后的结构；也会在某个原本以为不可能实现的功能终于跑起来的时候，获得一种并不算宏大、却十分真实的满足感。对我而言，这种满足感并不亚于功能本身。

所以，本文与其说是一篇教程，不如说是一份阶段性的记录：记录我如何在一块手表有限的边界之内，一点一点把它改造成更符合自己需求的样子。它未必适合所有人，其中许多方案也未必具有长期稳定性；但至少在写下这些文字的时候，它已经确实为我的日常生活增添了不少便利，也增添了不少乐趣。

如果这篇文章能为后来者提供哪怕一点参考，让你少走几个弯路，或者干脆因此生出一些继续折腾自己设备的兴趣，那么它的目的也就算达到了。