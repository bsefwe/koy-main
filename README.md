# koyeb容器云部署Xray高性能代理服务**XRay 将在部署时会自动实配安装`最新版本`。**

在koyeb容器云使用[xray](https://github.com/XTLS/Xray-core)+caddy同时部署通过ws传输的vmess vless trojan shadowsocks socks等协议，并默认已配置好伪装网站。
* 支持tor网络，且可通过自定义网络配置文件启动xray和caddy来按需配置各种功能  
* 支持存储自定义文件,目录及账号密码均为UUID,客户端务必使用TLS连接  
## 注意

请勿滥用本仓库
# 请勿使用常用的账号部署此项目，以免封号！！

## 部署步骤

1. fork本仓库
2. 在`Dockerfile`内第3-5行修改自定义设置，说明如下：

`AUUID`：用来部署节点的UUID，如有需要可在[uuidgenerator](https://www.uuidgenerator.net/)生成

`CADDYIndexPage`：伪装站首页文件

`ParameterSSENCYPT`：ShadowSocks加密协议

3. 去[Docker Hub](https://hub.docker.com/)注册一个账号，如有账号可跳过
4. 编辑Actions文件`docker-image.yml`，按照“name: Docker Hub ID/自定义镜像名称”格式修改第13行
5. 添加Actions的Secrets变量，变量说明如下

`DOCKER_USERNAME`：Docker Hub ID

`DOCKER_PASSWORD`：Docker Hub 登录密码

6. 打开某容器云主页，新建一个应用
7. 应用配置如下所示

`Docker Image`：Docker Hub镜像地址，格式为“docker.io/Docker Hub ID/自定义镜像名称”

`Container size`：部署配置，一般默认即可

`Port`：80

Environment variables：`Key`：PORT，`Value`：80
`Name`：自己定义


<details>
<summary>V2rayN(Xray、V2ray)</summary>

```bash
地址：xxx-xxx.koyeb.app 或 CF优选IP
端口：443
默认UUID：69349e47-755f-4643-a0a4-70ad7571c421
vmess额外id：0
加密：none
传输协议：ws
伪装类型：none
伪装域名：xxx-xxx.prod-glb.koyeb.app
路径：/69349e47-755f-4643-a0a4-70ad7571c421-vless
vless使用(/自定义UUID码-vless)，vmess使用(/自定义UUID码-vmess)
底层传输安全：tls
跳过证书验证：false
```

</details>

<details>
<summary>Trojan-Go</summary>

```bash
{
    "run_type": "client",
    "local_addr": "127.0.0.1",
    "local_port": 1080,
    "remote_addr": "xxx-xxx.prod-glb.koyeb.app",
    "remote_port": 443,
    "password": [
        "69349e47-755f-4643-a0a4-70ad7571c421"
    ],
    "websocket": {
        "enabled": true,
        "path": "/69349e47-755f-4643-a0a4-70ad7571c421-trojan",
        "host": "xxx-xxx.prod-glb.koyeb.app"
    }
}
```

</details>

<details>
<summary>Shadowsocks</summary>

```bash
服务器地址: xxx-xxx.koyeb.app
端口: 443
密码：69349e47-755f-4643-a0a4-70ad7571c421
加密：chacha20-ietf-poly1305
插件程序：xray-plugin_windows_amd64.exe
说明：需将插件 https://github.com/shadowsocks/xray-plugin/releases 下载解压后放至shadowsocks同目录
插件选项: tls;host=xxx-xxx.prod-glb.koyeb.app;path=/69349e47-755f-4643-a0a4-70ad7571c421-ss
```

</details>


<details>
<summary>可以使用Cloudflare的Workers来中转流量，（支持VLESS\VMESS\Trojan-Go的WS模式）配置为：</summary>

```js
const SingleDay = 'xxx.herokuapp.com'
const DoubleDay = 'xxx.herokuapp.com'
addEventListener(
    "fetch",event => {
    
        let nd = new Date();
        if (nd.getDate()%2) {
            host = SingleDay
        } else {
            host = DoubleDay
        }
        
        let url=new URL(event.request.url);
        url.hostname=host;
        let request=new Request(url,event.request);
        event. respondWith(
            fetch(request)
        )
    }
)
```
</details>

<details>

<summary>变量CADDYIndexPage用于CADDY部署完成后点击View显示的页面内容</summary>
```bash

> 选择你中意的链接地址复制后作为变量CADDYIndexPage变量值，欢迎PR，一些推荐：  
  
 [欢迎使用caddy页面]https://raw.githubusercontent.com/caddyserver/dist/master/welcome/index.html
  
[3DCEList元素周期表]https://github.com/wulabing/3DCEList/archive/master.zip 

 [Spotify-Landing-Page-Redesign]https://github.com/WebDevSimplified/Spotify-Landing-Page-Redesign/archive/master.zip  

 [dev-landing-page]https://github.com/flexdinesh/dev-landing-page/archive/master.zip 
  
 [free-for-dev]https://github.com/ripienaar/free-for-dev/archive/master.zip  
  
 [tailwindtoolbox-Landing-Page]https://github.com/tailwindtoolbox/Landing-Page/archive/master.zip 

 [sandhikagalih/simple-landing-page]https://github.com/sandhikagalih/simple-landing-page/archive/master.zip
  
 [StartBootstrap/startbootstrap-new-age]https://github.com/StartBootstrap/startbootstrap-new-age/archive/master.zip 

 [mikutap 一个好玩带音乐的页面]https://github.com/AYJCSGM/mikutap/archive/master.zip

 [WebGL流体模拟]https://github.com/PavelDoGreat/WebGL-Fluid-Simulation/archive/master.zip
  
 [loruki-website]https://github.com/bradtraversy/loruki-website/archive/master.zip  

 [bongo.cat一只音乐的猫]https://github.com/Externalizable/bongo.cat/archive/master.zip

```

</details>


