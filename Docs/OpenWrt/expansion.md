# ![openwrt logo](./svg/openwrt.svg ':size=60') Openwrt虚拟机扩容教程
---
## 一、在虚拟机上添加额外的硬盘空间，容量大小自定义
一般没有特殊用途的话，建议不用太大，建议在8GB-16GB即可

## 二、在openwrt/immortalwrt上安装插件
* ***1. fdsik插件***
>用于查看磁盘分区信息
* ***2. block-mount插件***
>磁盘容量挂载点工具
* ***3. e2fsprogs插件*** 
>磁盘格式化工具
* ***4. diskman插件*** 
>磁盘管理工具，下载成功后，新加载的硬盘空间会显示unknow

## 四、通过SSH接入openwrt/immortalwrt
```shell
fdisk -l
# 查看分区属性，找到未分区磁盘的名字，例如/dev/sdb

mkfs .ext4 /dev/sdb
# 格式化分区，后面一串是未分区的名字
```

## 五、找到挂载点，添加挂载点，选择格式化的分区，挂载到根目录/

## 六、点击保存并应用，一定要先保存应用

## 七、代码修改
在上一步的保存应用中，会有一串代码，拷贝下来<br>将其中第四行的 ***/dev/sdb***（名字会随系统而变化），改成自己格式化分区的名字<br>然后在SSH上运行
```shell
mkdir -p /tmp/introot
mkdir -p /tmp/extroot
mount --bind / /tmp/introot
mount /dev/sdb /tmp/extroot
tar -C /tmp/introot -cvf - . | tar -C /tmp/extroot -xf -
umount /tmp/introot
umount /tmp/extroot
```

## 八、SSH运行完毕后，重启软路由即可
