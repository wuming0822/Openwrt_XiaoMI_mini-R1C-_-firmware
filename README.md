# Openwrt_XiaoMI_mini_(R1C)_firmware（本地编译_非Github云编译）
#对于校园网检测路由器的方法目前已知有：

#1_基于 HTTP 数据包请求头内的 User-Agent 字段的检测

#2_基于 IPv4 数据包包头内的 TTL 字段的检测......等
#以上检测方法为目前已知的检测方法，有的可以直接更改防火墙规则而有的则需要修改路由器的运行环境以及增添模块

#本人使用的是Xiaomi_mini(R1CM),带USB版本，所以整篇都是基于Xiaomi_mini(R1CM)所围绕的教程

#运行编译环境需要用到科学上网环境以及ubuntu系统，如果自身现处环境不支持编译系统，可以使用本人以编译好的固件，包含的插件为`airplay2`,`shadowsocksr plus+`,以及防校园网终端检测的`ua2f`模块以防路由器性能不够，就没有安装过多插件.

[本人编译的固件(百度网盘)](https://pan.baidu.com/s/1e2gDDQES3LLKILT16cibYg?pwd=xin2)

#如果想要在此基础上增添自己想要的插件，可以跟着教程自己搭建本地编译环境从而添加自己需要的固件

#以下为在本地编译固件环境所需要的准备的：

##########1_`Vmware虚拟机`[Vmware]()

2_`Ubuntu_22.04_lts固件`（固件编译所需环境）[ubuntu固件阿里云镜像站](https://mirrors.aliyun.com/ubuntu-releases/22.04/ubuntu-22.04.3-desktop-amd64.iso?spm=a2c6h.25603864.0.0.2b7e45f8ROrZfC)

3_`米家的账号`（用于开启小米路由器的ssh功能）[MWIFI开发者官网](http://www.miwifi.com/miwifi_open.html)

4_`小米路由器mini开发者固件`[MWIFI开发者固件下载](http://www.miwifi.com/miwifi_download.html)

5_`以及一个良好的心情:)`

#以上所有需要的固件我都会打包放置百度网盘

###########[所需的固件包(百度网盘)]()



