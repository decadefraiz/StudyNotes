# ![docker logo](./svg/docker.svg ':size=60') Docker软件的安装教程

## 一、Linux系统
```shell
sudo update apt-get update && sudo apt-get upgrade
# 刚装完系统后，进行相应的更新

sudo apt-get install openssh-server
# 安装ssh远程服务

/etc/init.d/ssh restart
# 启动ssh远程服务

sudo apt install docker.io docker-compose
# 安装docker

sudo usermod -aG docker ${USER}
# 将当前用户加入docker组

sudo apt install curl
# 安装通过网站链接拉取镜像的功能
```

## 二、Windows系统
* 该方式需先在`windows11专业版`上开启相应设置`设置——系统——更多windows功能`，勾选`Hyper-V`、`windows虚拟机监控程序平台`、`适用于Linux的windows子系统`、`虚拟机平台`全部，最后进行系统重启。
* 重启电脑后，以管理员身份运行Powershell，安装Wsl2

```shell
wsl --install
# 安装wsl

wsl --set-default-version 2
# 将wsl调至版本2

wsl -l -o
# 查看可安装的发行版

wsl --install --d name
# name为对应的系统版本名称如：ubuntu-20.04
# 安装完成后会弹出ubuntu的提示框，根据提示设定相应的用户名和密码即可

wsl -l -v
# 查看wsl的版本

```

* 通过[Docker官方网址](https://www.docker.com/products/docker-desktop/)，下载对应版本的`Docker Desktop`，安装时勾选`启用Hyper-V的WSl2`
* 安装后，进行相应设置`Resources——WSL integration`勾选全部Linux系统，在`General——Use the WSL 2 based engine`上打钩

## 三、安装Potainer图形管理工具
```shell
docker volume create Portainer_data
# 创建一个包含Portainer服务器管理的数据的Docker卷

docker volume ls
# 验证名为“Portainer_data”的Docker卷是否存在

docker run -d -p 8000:8000 -p 9000:9000 -p 9443:9443 --name portainer --restart=always -v /var/run/docker.sock:/var/run/docker.sock -v portainer_data:/data portainer/portainer-ce:latest
# 安装实际的Portainer服务器

docker ps -a
# 检查名为“Portainer”的Docker容器是否正在运行
```