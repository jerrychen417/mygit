# 在VPS中安装Warp，实现无障碍访问IPV4/IPV6资源

本文基于Euserv VPS的Debian11系统的部署环境，如您的系统不是Debian 11的建议另寻方法

如您的VPS是OpenVZ的VPS，请自行在服务商的控制面板开启TUN模块后使用脚本开启Warp

1.SSH登录VPS

2.复制、粘贴并运行以下代码

```shell
wget -N https://cdn.jsdelivr.net/gh/fscarmen/warp/menu.sh && bash menu.sh
COPY
```

1. 选择1或2选项

![image-20220110012251399](https://gitee.com/jerry-chen417/picgo/raw/master/img/202201100122471.png)



3.输入Warp+的key，如没有按`Enter`键跳过，使用免费账号

![image-20220110012425258](https://gitee.com/jerry-chen417/picgo/raw/master/img/202201100124288.png)



4.选择内核方案（按照脚本作者的介绍，BoringTun > Wireguard-GO）

![image-20220110012439068](https://gitee.com/jerry-chen417/picgo/raw/master/img/202201100124102.png)



5.等待安装依赖



![image-20220110012451904](https://gitee.com/jerry-chen417/picgo/raw/master/img/202201100124958.png)

## 常见问题

### 1. 无法注册？如下图



![image-20220110012537508](https://gitee.com/jerry-chen417/picgo/raw/master/img/202201100125552.png)

请重新运行脚本安装

OpenVZ的机器请检查自己的TUN模块是否开启

### 2. 为什么warp的ip地区和vps的地区不一样

CloudFlare的一些玄学问题吧，具体不清楚

### 3. 为何我无法启用WARP？

有可能你使用的是OpenVZ的VPS，请在其服务商官网打开TUN之后输入以下命令，确保启用TUN模块

```shell
cat /dev/net/tun
COPY
```

返回结果如下即代表开启TUN模块

![image-20220110012602019](https://gitee.com/jerry-chen417/picgo/raw/master/img/202201100126053.png)



返回结果以下即代表TUN模块未开启

![image-20220110012615445](https://gitee.com/jerry-chen417/picgo/raw/master/img/202201100126475.png)



# 如何使用mack-a脚本搭建结点，实现互联网上的真正自由

1.SSH登录VPS

2.（IPV4或双栈VPS可省略）根据我前面文章安装Warp

3.复制粘贴以下代码

```shell
wget -P /root -N --no-check-certificate "https://raw.githubusercontent.com/mack-a/v2ray-agent/master/install.sh" && chmod 700 /root/install.sh && /root/install.sh
```

4.输入`1`安装

![image-20220110013135774](https://gitee.com/jerry-chen417/picgo/raw/master/img/202201100131948.png)

5.输入`1`安装Xray-core （博主认为这个内核性能最佳）

![image-20220110013148019](https://gitee.com/jerry-chen417/picgo/raw/master/img/202201100131075.png)

6.等待安装必要组件

7.打开CloudFlare的域名DNS解析页面，点击`Add Record`增加解析记录，类型选`AAAA`（ipv4的ip为`A`），Name输入你自己想设置的二级域名，IP address输入VPS的外网IP，小云朵点灰，点击`Save`保存解析结果

![image-20220110013216465](https://gitee.com/jerry-chen417/picgo/raw/master/img/202201100132501.png)

8.转回SSH命令行，输入解析好的域名



9.如出现以下提示，输入`y`开放端口

![image-20220110013246254](https://gitee.com/jerry-chen417/picgo/raw/master/img/202201100132323.png)



10.输入路径，一般回车即可





11.设置CF优选IP，默认回车即可。原因：这个脚本默认的优选IP已经被拉黑了，需要自己手动设置





12.设置uuid，一般回车即可





13.回到CloudFlare的页面，打开域名解析，点亮小云朵，点击`Save`保存





14.复制生成的`Vmess Ws TLS CDN`的结点链接





15.测试结点是否可用（请使用真延迟测试）





## 常见问题

1. 为什么我无法连接结点？

①. 由于默认脚本生成的结点是针对新版客户端的，可能对一些客户端不兼容，请把默认生成的伪装路径的`?ed=2048`删去

![image-20220110013334685](https://gitee.com/jerry-chen417/picgo/raw/master/img/202201100133724.png)

②. IPV6 Only的VPS没有安装WARP，请按照前文教程安装



③. 转到CF的域名管理页面，转到`SSL`选项卡，选择`Full`选项即可

![](https://gitee.com/jerry-chen417/picgo/raw/master/img/202201100134774.png)



1. 防火墙需要放行端口？
   ①. 在UFW设置中放行443和80两个端口

②. 由于mack-a近期更新与warp脚本冲突，故会提示无法解析需要放行443/80端口，实际上是与warp的脚本冲突。在安装mack-a脚本到域名解析的步骤请新建一个命令行，使用`warp o`命令暂时关闭warp，待域名解析完成后使用`warp o`命令打开warp即可



[![img](https://gitee.com/jerry-chen417/picgo/raw/master/img/202201100128006.png)
