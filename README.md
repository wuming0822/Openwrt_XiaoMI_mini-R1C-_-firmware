# 本地编译_非Github云编译
如果SSH的.bin文件并没有成功激活重启（红灯常亮），且USB接口没有问题，则大概率是你的激活U盘：

##### 1.格式没有格式成FAT32  2.U盘的分区要为MBP格式，如果激活错误常亮红灯则大概率是因为自己的U盘曾经改成过GPT格式，小米的激活过程不识别GPT分区格式的U盘，一定需要用分区软件改成MBP格式！

2.格式没有格式成FAT32  
## 1.对于开始编译系统之前的碎碎念
对于校园网检测路由器的方法目前已知有：

1.基于 HTTP 数据包请求头内的 User-Agent 字段的检测

2.基于 IPv4 数据包包头内的 TTL 字段的检测

3.基于 HTTP 数据包请求头内的 User-Agent 字段的检测

4.DPI (Deep Packet Inspection) 深度包检测技术

5.基于 IPv4 数据包包头内的 Identification 字段的检测

6.基于网络协议栈时钟偏移的检测技术

7.Flash Cookie 检测技术

以上检测方法为目前已知的检测方法，有的可以直接更改防火墙规则而有的则需要修改路由器的运行环境以及增添模块

本人使用的是Xiaomi_mini(R1C),带USB版本，所以整篇都是基于Xiaomi_mini(R1C)所围绕的教程

运行编译环境需要用到科学上网环境以及ubuntu系统，如果自身现处环境不支持编译系统，可以使用本人已编译好的固件，包含的插件为`airplay2`,`shadowsocksr plus+`,以及防校园网终端检测的`ua2f`,`RKP-IPID`模块，以防路由器性能不够，就没有安装过多插件.

[本人编译的固件(百度网盘)](https://pan.baidu.com/s/1e2gDDQES3LLKILT16cibYg?pwd=xin2)

如果想要在此基础上增添自己想要的插件，可以跟着教程自己搭建本地编译环境从而添加自己需要的固件

以下为在本地编译固件环境所需要的准备的：

1.`Vmware虚拟机`[VMware 17 下载安装及永久激活使用教程](https://www.cnblogs.com/hellogmy/p/17253041.html)

2.`Ubuntu_22.04_lts固件`（固件编译所需环境）[ubuntu固件阿里云镜像站](https://mirrors.aliyun.com/ubuntu-releases/22.04/ubuntu-22.04.3-desktop-amd64.iso?spm=a2c6h.25603864.0.0.2b7e45f8ROrZfC)

3.`米家的账号`（用于开启小米路由器的ssh功能）[MWIFI开发者官网](http://www.miwifi.com/miwifi_open.html)

4.`小米路由器mini开发者固件`[MWIFI开发者固件下载](http://www.miwifi.com/miwifi_download.html)

5.`breed固件(最大程度保护路由器在刷机过程中不变砖)`[breed下载站](https://breed.hackpascal.net/breed-mt7620-xiaomi-mini.bin)

6.`以及一个良好的心情:)`

## 2.开始进行ubuntu系统的搭建

在虚拟机内创建一个ubuntu系统（用前面所下载的iso镜像）

如果自身电脑内存16G可以将4G的内存分配给虚拟机的ubuntu系统，如果16G以下可以少分配一点，最好不要小于2G内存，内存的数量与你最终编译openwrt的速度成正比

处理器可以1核心2线程，或者2核心4线程（也是取决你的电脑配置）

分配50G存储给ubuntu系统

进入系统后选择上海时间地区

进入系统之后，为了接下来的步骤顺利进行，可进行换源操作 [换源教程](https://blog.csdn.net/frighting_ing/article/details/122688413)

## 3.在ubuntu系统中配置编译openwrt固件所需要的环境

打开ubuntu终端输入
```
sudo apt-get update
```
等待，然后输入,安装编译环境所需要的软件
```
sudo apt-get -y install build-essential asciidoc binutils bzip2 gawk gettext git libncurses5-dev libz-dev patch python3 python2.7 unzip zlib1g-dev  libc6-dev-i386 subversion flex uglifyjs git-core gcc-multilib p7zip p7zip-full msmtp libssl-dev texinfo libglib2.0-dev xmlto qemu-utils upx libelf-dev autoconf automake libtool autopoint device-tree-compiler g++-multilib antlr3 gperf wget curl swig rsync
```
拉取源码
```
git clone https://github.com/coolsnowwolf/lede
```
拉取校园网所需要的UA2F模块和RKP-IPID模块
```
git clone https://github.com/EOYOHOO/UA2F.git package/UA2F
git clone https://github.com/EOYOHOO/rkp-ipid.git package/rkp-ipid
```
打开主目录，然后找到lede文件并双击打开feeds.conf.default文件，打开后在最后一行添加：
```
src-git kenzo https://github.com/kenzok8/openwrt-packages
src-git small https://github.com/kenzok8/small
```
回到终端，更新feed模块
```
./scripts/feeds update -a
./scripts/feeds install -a
```
## 4.开始本地编译.config
打开编译菜单，在终端输入
```
make menuconfig
```
##### 在前三项为配置与个人使用相符的硬件的相关的设置（用于确定固件的运行环境）

本人以小米路由mini为例设置前三项

`Target System选择-> MediaTek Ralink MIPS`

`Subtarget选择-> MT7620 based boards`

`Target Profile选择-> Xiaomi MiWiFi Mini`

其他硬件或软路由同理，在网上找一找就能找到相应的选项

##### 后面的选项依据教程勾选相应的插件，开启模块，以及主题的选择(在相应的模块前按 Y 即可勾选，或 双击空格 来确定勾选，如果勾选成功则相应的选项最前面的（）里就会出现*，如（*）)

勾选上ipid

`kernel-modules`->`Other modules`->`kmod-rkp-ipid`

勾选相应模块

`kernel modules`->`Netfilter Extensions`->`kmod-ipt-u32`

`kernel modules`->`Netfilter Extensions`->`kmod-ipt-ipopt`

勾选上ua2f模块

`network`->`Routing and Redirection`->`ua2f`

勾选模块

`network`->`firewall`->`iptables-mod-filter`

`network`->`firewall`->`iptables-mod-ipopt`

`network`->`firewall`->`iptables-mod-u32`

`network`->`firewall`->`iptables-mod-conntrack-extra`

在`LUCI`->`application`中勾选自己需要的插件，因为无注释，可搭配[注释网站](https://vvmi.net/archives/openwrtlist/)来勾选自己需要的插件

在`LUCI`->`Theme`中选择第三项主题（看个人喜好）

按方向键移动到Exit，按回车键即可退出编译设置界面

## 本地编译openwrt固件（非常看个人网络，要有科学上网环境）

1.在终端输入指令下载 dl 库（等待时间有点长，请耐心等待）
```
make download -j8
```
2.编译固件，终端输入
```
make V=s -j1
```
##### ubuntu环境开始进行固件编译，在2G内存1核心2线程的情况下编译时间为：2-3小时

编译好的固件在`leda/bin/targets`里

## 5.固件刷入（运用到breed固件以及路由器ssh激活固件）[小米路由器mini通过breed刷入openwrt系统](https://www.bilibili.com/video/BV1qV411j7P8/?spm_id_from=333.337.search-card.all.click&vd_source=374488c08b0f741f142ffcb2138296be)

## 6.配置openwrt系统（模块开启以及防火墙的自定义规则）

进入openwrt，点击系统然后勾选`Enable NTP client（启用 NTP 客户端`，`Provide NTP server（作为 NTP 服务器提供服务）`

在后面的`NTP server candidates（候选 NTP 服务器）`分别填写`ntp1.aliyun.com`,`time1.cloud.tencent.com`,`stdtime.gov.hk`,`pool.ntp.org`

在`网络`->`防火墙规则`->`自定义规则`里添加：

```
iptables -t nat -A PREROUTING -p udp --dport 53 -j REDIRECT --to-ports 53

iptables -t nat -A PREROUTING -p tcp --dport 53 -j REDIRECT --to-ports 53

# 通过 rkp-ipid 设置 IPID

# 若没有加入rkp-ipid模块，此部分不需要加入

iptables -t mangle -N IPID_MOD

iptables -t mangle -A FORWARD -j IPID_MOD

iptables -t mangle -A OUTPUT -j IPID_MOD

iptables -t mangle -A IPID_MOD -d 0.0.0.0/8 -j RETURN

iptables -t mangle -A IPID_MOD -d 127.0.0.0/8 -j RETURN

iptables -t mangle -A IPID_MOD -d 10.0.0.0/8 -j RETURN

iptables -t mangle -A IPID_MOD -d 172.16.0.0/12 -j RETURN

iptables -t mangle -A IPID_MOD -d 192.168.0.0/16 -j RETURN

iptables -t mangle -A IPID_MOD -d 255.0.0.0/8 -j RETURN

iptables -t mangle -A IPID_MOD -j MARK --set-xmark 0x10/0x10

# 防时钟偏移检测

iptables -t nat -N ntp_force_local

iptables -t nat -I PREROUTING -p udp --dport 123 -j ntp_force_local

iptables -t nat -A ntp_force_local -d 0.0.0.0/8 -j RETURN

iptables -t nat -A ntp_force_local -d 127.0.0.0/8 -j RETURN

iptables -t nat -A ntp_force_local -d 192.168.0.0/16 -j RETURN

iptables -t nat -A ntp_force_local -s 192.168.0.0/16 -j DNAT --to-destination 192.168.1.1

# 通过 iptables 修改 TTL 值 数字为需要的修改的ttl值

iptables -t mangle -A POSTROUTING -j TTL --ttl-set 64

# iptables 拒绝 AC 进行 Flash 检测

iptables -I FORWARD -p tcp --sport 80 --tcp-flags ACK ACK -m string --algo bm --string " src=\"http://1.1.1." -j DROP
```

添加完自定义规则后，用windows的终端或用Xshell，powershell等远程控制软件连接到小米路由器后台`IP:192.168.1.1`的终端上

如果为windows终端或powershell则直接在终端输入`ssh root@192.168.1.1`

login：输入`root`

password: 输入`password`(输入密码时不显示，输入完毕后回车即可)

即可进入管理后台

在终端输入以下指令用于管理ua2f模块的开启以及配置
```
# 开机自启
uci set ua2f.enabled.enabled=1
# 自动配置防火墙（默认开启）（建议开启）
uci set ua2f.firewall.handle_fw=1
uci set ua2f.firewall.handle_tls=1
uci set ua2f.firewall.handle_mmtls=1
uci set ua2f.firewall.handle_intranet=1
# 保存配置
uci commit ua2f

操作：# 开机自启
service ua2f enable
# 启动ua2f
service ua2f start

# 手动关闭ua2f
service ua2f stop
```

即可完成XiaoMI_mini_openwrt所有配置，现在连上你的宿舍网线，尽情畅游因特网吧！

## 参考文献
[openwrt编译校园网防检测固件（例r2s）https://y5hwpw3fsi.feishu.cn/docx/UO0wdHReDoUdewxwlpGckSjin3d](https://y5hwpw3fsi.feishu.cn/docx/UO0wdHReDoUdewxwlpGckSjin3d)

[关于某大学校园网共享上网检测机制的研究与解决方案https://www.sunbk201.site/posts/crack-campus-network](https://www.sunbk201.site/posts/crack-campus-network)

[校园网检测/极路由3(HC5861)openwrt固件，过校园网多设备检测（非破解）https://www.right.com.cn/forum/thread-8240757-1-1.html](https://www.right.com.cn/forum/thread-8240757-1-1.html)

[ubuntu 22.04配置国内镜像源: 阿里云/清华大学/中科大https://blog.csdn.net/a772304419/article/details/132470753](https://blog.csdn.net/a772304419/article/details/132470753)

[ubuntu换镜像源（ubuntu换源）https://blog.csdn.net/frighting_ing/article/details/122688413](https://blog.csdn.net/frighting_ing/article/details/122688413)

[OpenWrt插件安装对照表https://vvmi.net/archives/openwrtlist/](https://vvmi.net/archives/openwrtlist/)

[Breed 更新https://blog.hackpascal.net/2022/07/2022-07-24-breed-update/](https://blog.hackpascal.net/2022/07/2022-07-24-breed-update/)

[breed下载站https://breed.hackpascal.net/](https://breed.hackpascal.net/)

[[MINI] 小米路由器mini（MiR1C）最新root方法，刷breed和openwrthttps://www.right.com.cn/forum/thread-5662043-1-1.html](https://www.right.com.cn/forum/thread-5662043-1-1.html)






















