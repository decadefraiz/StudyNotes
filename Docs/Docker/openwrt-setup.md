# ![docker logo](./svg/docker.svg ':size=60') Docker安装软路由系统

## 一、查看网卡信息，确认ipv4地址和网关地址，以及网卡混合模式是否开启
```shell
sudo route -n
# 查看自己的路由信息，主要是看路由网关

ifconfig
# 查看自己的网卡信息，如果显示查看不了，就按照提示用命令行安装netwotk-tool

ifconfig eth0
# 不同系统的网卡名称不同，可能不是eth0，是具体情况而定
# 查看自己的网卡是否打开了混杂模式，flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500这里面没显示PROMISC就是没有打开

sudo ip link set eth0 promisc on 或者 sudo ifconfig eth0 promisc
# 打开网卡的混杂模式

ifconfig eth0
# 查看混杂模式是否开启，flags=4419<UP,BROADCAST,RUNNING,PROMISC,MULTICAST>  mtu 1500，这里面显示有PROMISC就是已经开启了
```
<br>

## 二、创建Docker的macvlan网卡
```shell
docker network create -d macvlan --subnet=192.168.71.0/24 --gateway=192.168.71.1 -o parent=eth0 macnet
docker network create -d macvlan --subnet=192.168.71.0/24 --gateway=192.168.71.1 -o parent=eth0 -o macvlan_mode=bridge macnet
# 创建docker网卡，下面的代码区别在于网卡的模式是桥模式，可以将容器的ip置于和宿主机同层

docker network list
# 查看macvlan网卡是否建成
```
<br>

## 三、拉取Docker镜像文件
```shell
curl https://downloads.immortalwrt.org/releases/23.05.2/targets/x86/64/immortalwrt-23.05.2-x86-64-rootfs.tar.gz
# 通过该网址拉取docker版本能用的immortalwrt镜像文件

cd /home/auther/下载
# 进入文件夹路径，auther为用户名，找到.tar.gz文件

gunzip xxx.tar.gz
# 解压文件，发现文件后缀变为.tar表示解压成功

docker import xxx.tar immortalwrt
# 将解压后的xxx.tar文放进docker镜像库里，并命名为immortalwrt

docker pull sulinggg/openwrt
# 从其他镜像仓库获取镜像文件。sulinggg/openwrt这一段是需要拉取的镜像名称

docker images
# 查看镜像是否存在
```
<br>

## 四、运行Docker容器，并进行ip地址的修改
```shell
docker run --restart always --name openwrt -d --network macnet --privileged sulinggg/openwrt:x86_64 /sbin/init
docker run --restart always --name openwrt -d --network macnet --ip=192.168.71.55 --privileged sulinggg/openwrt:x86_64 /sbin/init
# 启动容器，restrat always指的是当容器挂掉或者主机重启就会自动重启，name是容器的名字，network是用的网卡，sulinggg/openwrt:x86_64是镜像的名字
# 上下代码的不同点是，若第四步的macvlan选择桥模式，则此处可选择第二行自定义容器的IP地址

docker ps
# 查看容器运行进程

docker exec -it 996484d93afa bash
docker exec -it immortalwrt /bin/sh
# 上下两种都可以进入容器内部，后面的一串字符就是前一步创建成功的容器id，第二种适用于images的名称

passwd
# 上方若选择 /bin/sh进入，则可能需要先修改登录密码

vim /etc/config/network
# 修改IP段，进入之后上下左右键选到'lan'那一栏的ipaddr，按键盘上的Insert，进入修改模式，修改为自己路由同一网段的即可，gateway改为自己的网关，dns也改为网关

esc
# 退出修改模式

:wq
# 保存修改

/etc/init.d/network restart
service network restart
# 进行重启，两种都可以
```
<br>

## 五、登录软路由
通过修改的ip地址，在网页登录创建的openwrt，如果没有改过密码，则默认密码是password

<br>

## 六、软件包相关
如果一直显示软件包列表更新不了，可以尝试替换软件包源
```shell
# openwrt软件国内中科大源
src/gz immortalwrt_core https://mirrors.ustc.edu.cn/openwrt/releases/21.02.7/targets/x86/64/packages
src/gz immortalwrt_base https://mirrors.ustc.edu.cn/openwrt/releases/21.02.7/packages/x86_64/base
src/gz immortalwrt_luci https://mirrors.ustc.edu.cn/openwrt/releases/21.02.7/packages/x86_64/luci
src/gz immortalwrt_packages https://mirrors.ustc.edu.cn/openwrt/releases/21.02.7/packages/x86_64/packages
src/gz immortalwrt_routing https://mirrors.ustc.edu.cn/openwrt/releases/21.02.7/packages/x86_64/routing
src/gz immortalwrt_telephony https://mirrors.ustc.edu.cn/openwrt/releases/21.02.7/packages/x86_64/telephony

# openwrt软件包国外官方源——备份
src/gz immortalwrt_core https://mirrors.vsean.net/openwrt/releases/21.02.7/targets/x86/64/packages
src/gz immortalwrt_base https://mirrors.vsean.net/openwrt/releases/21.02.7/packages/x86_64/base
src/gz immortalwrt_luci https://mirrors.vsean.net/openwrt/releases/21.02.7/packages/x86_64/luci
src/gz immortalwrt_packages https://mirrors.vsean.net/openwrt/releases/21.02.7/packages/x86_64/packages
src/gz immortalwrt_routing https://mirrors.vsean.net/openwrt/releases/21.02.7/packages/x86_64/routing
src/gz immortalwrt_telephony https://mirrors.vsean.net/openwrt/releases/21.02.7/packages/x86_64/telephony
```