# 零散知识点

## [返回首页](../README.md)

### VPN知识点

今天通过一天的时间 去实践各种vpn
发现： 中国防火墙对中国物联网的保护 保护的效果的特别强 基本能将大部分vpn阻挡。
实践 ： 我今天在网上找了 四个vpn(皆为免费) 发现大量vpn无法直接通过中国互联网访问到vpn服务器 有两个 可以访问 一个连接速度惨不忍睹 连谷歌都打不开 一个基本能正常使用 但有流量限制(10Gb)

<hr>
对vpn的使用的理解 
首先先理解 私网IP和公网IP的区别 首先 互联网中 我们可以通过公网IP访问到每台不同的服务器 我们购买的服务器很大一部分买的就是公网IP 因为公网IP遵守的是IP4 协议所以随着时间推移推出了IP6还有私网ip技术  我们通过公网IP找到那台服务器 然后通过私网IP查找到自己的设备 一个公网ip中可以拥有大量私网IP
<br>
我们这里把服务器理解成VPN 我们通过公网访问我们指定的服务器 如果服务器没有在国内则去需要翻墙 等于是 我们通过访问指定VPN让自己的请求打包 在通过VPN（服务器） 来传递数据来做到绕开防火墙的功能  

### md分级

和普通普通的文件管理一样 但是记得在gitee中需要分级管理需要从/开始不能使用相对定位。
在githup上面则不会有此烦恼

### git pull遇到错误：[error: Your local changes to the following files would be overwritten by merge](https://blog.csdn.net/misakaqunianxiatian/article/details/51103734)

### 用于git上传的ssh公钥私钥

我们如果项目克隆方式是ssh方式进行克隆（和后续的代码提交）我们需要在gitee和github上面设置自己的ssh的公钥。当新电脑配置新的公私钥的时（建议直接使用复制久电脑的sshkey 这样就可以避免大量git配置项重配，包括服务器里面的用于git的key）如果我们通过git克隆下来的ssh公私key我们需要更换空格模式（因为使用git时他会自动的把lf换成lfef导致异常）。

> 服务器中的sshkey
>
> 服务器有两种访问方式
>
> 1. 通过密码进行访问：但是此访问方法很危险。在后续配置好ssh钥后将取消这种访问方式。
>
> 2. 通过sshkey访问：由服务器生成公key然后通过putty生成私key然后选择私key位置。将公私key进行绑定这样就能够就能够直接访问服务器（私key一定不能掉，否则整个服务器只能进行重置。）
>
> 这里的sshkey是为了在访问服务器的时候来权限是否有权限。

### 如果在json转移字符串需要变量

``` java
    String nums = MessageJunit.achieveNum();
    String s = "{\"code\":\"nums\"}".replaceAll("nums", nums);
```

### [qq邮箱发送](https://blog.csdn.net/weixin_46713508/article/details/117537495#:~:text=springboot%E5%8F%91%E9%80%81QQ%E9%82%AE%E7%AE%B1%201%201.%E5%BC%80%E5%90%AFqq%E9%82%AE%E7%AE%B1%E5%BC%80%E5%90%AFIMAP%2FSMTP%E6%9C%8D%E5%8A%A1%2A%20%E9%A6%96%E5%85%88%E8%BF%9B%E5%85%A5qq%E9%82%AE%E7%AE%B1%20%E7%82%B9%E5%87%BB%E8%AE%BE%E7%BD%AE%20%E7%82%B9%E5%87%BB%E8%B4%A6%E6%88%B7%2C%E7%84%B6%E5%90%8E%E5%BE%80%E4%B8%8B%E6%8B%89%20%E5%BC%80%E5%90%AFIMAP%2FSMTP%E6%9C%8D%E5%8A%A1%20%E5%BC%80%E5%90%AF%E6%88%90%E5%8A%9F%E5%BE%97%E5%88%B0%E6%8E%88%E6%9D%83%E5%AF%86%E7%A0%81%2C%E8%BF%99%E4%B8%AA%E8%A6%81%E8%AE%B0%E4%BD%8F%2C%E4%B8%80%E4%BC%9A%E7%94%A8,3.%E9%85%8D%E7%BD%AEapplication.properties%20...%204%204.%E5%88%9B%E5%BB%BA%E5%8F%91%E9%80%81%E7%94%B5%E5%AD%90%E9%82%AE%E7%AE%B1controller%20...%205%205.%E6%9F%A5%E7%9C%8B%E6%95%88%E6%9E%9C%20)
