# ![cf logo](./svg/cf.svg ':size=60') Proxy IP & CDN IP的相关优选操作

---

## 一、获取Proxy IP
**1、获取proxy ip**
- **方法①：FOFA网站**
  - 进入 ***[fofa网站](https://fofa.info/)*** 进行ip删选
  - 输入删选语法
    - 国内反代IP：`server=="cloudflare" && port=="80" && header="Forbidden" && country=="CN"`
    - 剔除CF：`asn!="13335" && asn!="209242"`
    - 阿里云：`server=="cloudflare" && asn=="45102"`
    - 甲骨文韩国：`server=="cloudflare" && asn=="31898" && country=="KR"`
    - 搬瓦工：`server=="cloudflare" && asn=="25820"`
  - 使用 ***[临时邮箱网站](https://www.linshiyouxiang.net/)*** 登录fofa，并在搜索栏内输入以下语言

```shell
server=="cloudflare" && port=="443" && header="Forbidden" && country=="US" && city=="Santa Clara" && asn!="13335" && asn!="209242"
# 这是优选美国加利福尼亚圣克拉拉的反代ip

server=="cloudflare" && port=="443" && header="Forbidden" && country=="US" && city=="San Jose" && asn!="13335" && asn!="209242"
# 这是优选美国加利福尼亚圣何塞的反代ip
```

>`server`代表服务商，`port`是端口号，`header`是头部相应，`country`是国家，`region`是地区，`city`是国家，`=`是模糊匹配，`==`是完全匹配，`!=`是剔除

- **方法②：Telegram群获取**
  - 主要有两个群：***CF中转IP发布*** &nbsp;**&**&nbsp; ***HeroMsg***
  - 新建一个`.txt`文件，输入`type *.txt>>all.txt`，并把文件后缀名改为`.bat`
  - 将bat文件和保存出来的ip文件置于同一个文件夹内，并运行bat文件，得到`all.txt`文件

<br>

**2、proxy ip测速**
- 主要用到github上XIU2大佬的 ***[CloudfalreST工具](https://github.com/XIU2/CloudflareSpeedTest)***
- 将保存好的ip，分别复制进`fdip.txt` `fdip-tg.txt` `fdip-ca.txt` `fdip-santa`
- 在下载好的`CloudflareST_windows_amd64`的文件夹的上方路径栏运行`cmd`命令，打开终端，输入以下命令

```shell
CloudflareST.exe -url https://cs.fraiz555.dynv6.net/100m -tp 443 -tl 999 -sl 5 -tlr 0.10 -f fdip.txt -o result-fdip.csv
# 这是来自fofa网站的ip库

CloudflareST.exe -url https://cs.fraiz555.dynv6.net/100m -tp 443 -tl 999 -sl 5 -tlr 0.10 -f fdip-tg.txt -o result-fdip-tg.csv
# 这是来自telegram的ip库

CloudflareST.exe -url https://cs.fraiz555.dynv6.net/100m -tp 443 -tl 999 -sl 5 -tlr 0.10 -dn 20 -f fdip-san.txt -o result-fdip-san.csv
# 这是专门优选San Jose的ip库

CloudflareST.exe -url https://cs.fraiz555.dynv6.net/100m -tp 443 -tl 999 -sl 5 -tlr 0.10 -dn 20 -f fdip-santa.txt -o result-fdip-santa.csv
# 这是专门挑选Santa Clara的ip库
```
>`-url`=测速地址，`-tp`=测试端口，`-tl`=延迟上限，`-sl`=速度下限，`-tlr`=丢包率上限，`-dn`=测试数量(默认10)，`-f`=测试文件名，`-o`=结果导出文件名

<br><br>

## 二、获取CDN IP
**1、FOFA删选优选ip：**
语法操作参考proxy ip
```shell
server=="cloudflare" && port=="443" && country=="US" && city=="Santa Clara" && header!="Forbidden"
server=="cloudflare" && port=="443" && country=="US" && city=="San Jose" && header!="Forbidden"
server=="cloudflare" && port=="443" && country=="JP" && region=="Tokyo"
server=="cloudflare" && port=="443" && country=="HK"
server=="cloudflare" && port=="443" && country=="TW"
server=="cloudflare" && port=="443" && country=="KR"
server=="cloudflare" && port=="443" && country=="SG"
```

<br>

**2、优选ip测速：**
具体操作参考proxy ip
```shell
CloudflareST.exe -url https://cs.fraiz555.dynv6.net/100m -tp 443 -tl 200 -sl 5 -tlr 0.10 -f yxip-us.txt -o result-us.csv
CloudflareST.exe -url https://cs.fraiz555.dynv6.net/100m -tp 443 -tl 200 -sl 5 -tlr 0.10 -f yxip-jp.txt -o result-jp.csv
CloudflareST.exe -url https://cs.fraiz555.dynv6.net/100m -tp 443 -tl 200 -sl 5 -tlr 0.10 -f yxip-hk.txt -o result-hk.csv
CloudflareST.exe -url https://cs.fraiz555.dynv6.net/100m -tp 443 -tl 200 -sl 5 -tlr 0.10 -f yxip-kr.txt -o result-kr.csv
CloudflareST.exe -url https://cs.fraiz555.dynv6.net/100m -tp 443 -tl 200 -sl 5 -tlr 0.10 -f yxip-sg.txt -o result-sg.csv
CloudflareST.exe -url https://cs.fraiz555.dynv6.net/100m -tp 443 -tl 200 -sl 5 -tlr 0.10 -f yxip-tw.txt -o result-tw.csv
```

<br>

**自建测速地址：**

|  Port   |                                      Link                                       |
|:-------:|:-------------------------------------------------------------------------------:|
| 80系列  | http://cs.decadefaiz.xyz/100m <br> http://csworker.decadefraiz.workers.dev/100m |
| 443系列 |     https://cs.decadefaiz.xyz/100m <br> https://cs.fraiz555.dynv6.net/100m      |
>80端口系列的都是http，443端口的是https，/100m是自定义下载包大小，可随意更改

<br>

**端口种类：**

|  Name   |                  Port                  |
|:-------:|:--------------------------------------:|
| 80系列  | 80，8080，8880，2052，2082，2086，2095 |
| 443系列 |   2053，2083，2087，2096，8443，443    |