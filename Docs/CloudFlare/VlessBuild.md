# ![cf logo](./svg/cf.svg ':size=60') Vless节点搭建教程
---
本项目起源于Github的`zizifn大佬`开发的EDtunnel项目，是利用Cloudflare平台的服务进行VPN节点搭建，后续又由`3Kmfi6HP大佬`进行接手继续深化。  
后续又出现其他大佬不断对代码进行修改完善，现已有很多教程博主的仓库。

>zizifn大佬EDtunnel仓库地址：https://github.com/zizifn/edgetunnel/blob/main/src/worker-vless.js  
>3Kmfi6HP大佬EDtunnel仓库地址：https://github.com/3Kmfi6HP/EDtunnel/blob/main/_worker.js

>[!note|style:callout]
>本记录为了照顾后续的订阅器项目，所以使用的是`cmliu大佬的代码`  
>仓库地址为：https://github.com/decadefraiz/edgetunnel-CM/blob/main/_worker.js

## 一、Workers部署方式
### 1.注册Cloudflare账号
来到[Cloudflare官网](https://dash.cloudflare.com/)注册账号，该网站可以直接通过国内网络访问，无需翻墙，注册完毕后可通过右上角切换至简体中文。  

### 2.创建workers
从侧边栏找到`Workers和Pages`，点击`创建应用程序`，继续点击`创建Woeker`，输入想要创建的名称，空格会被`-`代替，最后点击`部署`。   
点击`编辑代码`，清空里面全部的代码，随后拷贝大佬仓库里的`_worker.js`里面的代码，全部粘贴进去，然后点击右上角的`部署`，然后点击`保存并部署`
>建议保存后，返回再进入查看是否成功。有时候因为网络问题可能会需要多部署几次。

### 3.配置变量
在代码部署成功后，一般用户只需要注意代码的两个地方即可。  

```js
let userID = '90cd4a77-141a-43c9-991b-08263cfe9c10'
// 修改UUID，可以通过v2ray软件的创建服务器的用户ID生成

let proxyIP = '';
// 这是反代ip，是为了用创建的节点访问cloudflare相关网站使用的，小白可直接填入cdn.xn--b6gac.eu.org, cdn-all.xn--b6gac.eu.org, workers.cloudflare.cyou，不填将无法访问cloudflare相关的网站
```

### 4.notls裸奔模式
该模式不需要绑定自定义域名，但是存在风险，墙不仅知道你在翻墙，也知道你在翻墙看什么，等于明文翻墙。  
该模式在刚配置的`Workers`项目里，点击`设置`——`触发器`——`路由`，在浏览器输入路由里面的地址后，加上`/你的UUID`。  
例如`https://worker-green-sea-0651.decadefraiz.workers.dev/90cd4a77-141a-43c9-991b-08263cfe9c10`,即可跳转至节点连接界面，复制粘贴进v2ray即可使用，clash用户需要去节点转换网页自行转换一下。

### 5.tls安全模式
该模式需要绑定一个托管在cloudflare上的自定义域名，绑定后的节点相对安全，墙同样知道你在翻墙，但是不知道你在翻墙做什么，仅此而已。  
同样，在刚配置的`Workers`项目里，点击`设置`——`触发器`——`添加自定义域名`，然后输入你已经托管在cloudflare上的域名的下一层级域名。  
例如：你托管的域名是`xxxxx.top`，则添加自定义域名的时候，添加为`abc.xxxxx.top`，其中`abc`是你自己设定的名称。  
后续也一样在浏览器输入`https://abc.xxxxx.top/90cd4a77-141a-43c9-991b-08263cfe9c10`即可跳转至节点连接地址，复制进v2ray即可使用。

### 6.优选ip
一般刚创建出来的节点速度可能都不是很理想，所以此处需要用到第三方ip来进行加速，详情操作请见[优选ip](./Iptest.md)  
优选出来的ip直接填入v2ray客户端节点中的`地址（address）`中即可。
>[!attention]
>针对notls的节点，只适用于`80，8080，8880，2052，2082，2086，2095`这些端口号。  
>针对tls的节点，需要改变端口号为`2053，2083，2087，2096，8443，443`这些端口。

## 二、Pages部署方式（🌟推荐方式）
### 1.Fork项目
登录[GitHub官网](https://github.com/)，注册一个账号。  
进入大佬的项目仓库，点击右上角的`Fork`和`Star`，fork后可以自定义名字。   
<p><img src="/Docs/CloudFlare/img/fork.jpg" alt="如何fork的图片"></p>  

### 2.创建Pages
从侧边栏找到`Workers和Pages`，点击`创建应用程序`，继续点击`Pages`，点击`连接到Git`，输入自己的Github账号密码，并绑定，选择`fork`的项目名称，然后点击`开始设置`，项目名称可以自定义，最后点击`保存并部署`。  
等待上传完成即可完成操作。

### 3.修改变量
同样地，我们需要修改`UUID`和`Proxy IP`，不同的是，由于pages是远程连接到github，所以要么直接在github上修改，要么通过`环境变量`的方式修改。  
进入部署的`Pages项目`，点击`设置`——`环境与变量`——`添加变量`。  
**变量名称**就是`UUID`和`PROXYIP`，**值**就是`90cd4a77-141a-43c9-991b-08263cfe9c10`和`cdn.xn--b6gac.eu.org, cdn-all.xn--b6gac.eu.org, workers.cloudflare.cyou`
>[!attention|style:callout]
>UUID的值可以自定义，PROXYIP的值建议小白就默认不要动

### 4.绑定自定义域名
由于Pages部署方式无需托管在Cloudflare上的域名，操作相对便捷友善，推荐都绑定一个，免费的二级域名可以自行搜索，网上很多。  
点击`自定义域`——`设置自定义域`，输入自定义的域名，原理和Workers的一致，然后点击`开始CNAME设置`。  
登录自己的域名管理后台，添加`CNAME记录`，`自己域名里的Name`对应`cloudflare里的名称`，`自己域名的Data`对应`cloudflare里面的目标`
>[!attention|style:callout]
>有些域名管理后台在添加CNAME的时候，Data一栏需要添加`.`作为结尾才能解析成功  

最后在Cloudflare点击`检查DNS记录`，然后等待生效即可。

### 5.生成节点链接
完成上述操作操作之后，一定要回到项目部署页面，点击`所有部署后面...`——`重试部署`，一定要重新部署一下才能生效之前的所有操作。  
最后获取连接的操作方式与Wokers一致，都是`自定义域名/UUID`的形式。

### 6.优选ip
详情操作请见[优选ip](./Iptest.md)

>[!attention]
>- 新建的节点在优选ip之后，尽量不要用代理软件进行测速，就去`youtube`观看4K视频看看速度即可，否则ip会很容易死
>- `域名/UUID`的获取节点连接的方式是通用方式，在`Error1101`事件之后，每个大佬的代码都有所修改来绕过Cloudflare的检测，因此推荐Pages部署，定期检查大佬是否更新代码