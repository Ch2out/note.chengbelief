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

### 如果在json转移字符串需要变量

``` java
    String nums = MessageJunit.achieveNum();
    String s = "{\"code\":\"nums\"}".replaceAll("nums", nums);
```

### [qq邮箱发送](https://blog.csdn.net/weixin_46713508/article/details/117537495#:~:text=springboot%E5%8F%91%E9%80%81QQ%E9%82%AE%E7%AE%B1%201%201.%E5%BC%80%E5%90%AFqq%E9%82%AE%E7%AE%B1%E5%BC%80%E5%90%AFIMAP%2FSMTP%E6%9C%8D%E5%8A%A1%2A%20%E9%A6%96%E5%85%88%E8%BF%9B%E5%85%A5qq%E9%82%AE%E7%AE%B1%20%E7%82%B9%E5%87%BB%E8%AE%BE%E7%BD%AE%20%E7%82%B9%E5%87%BB%E8%B4%A6%E6%88%B7%2C%E7%84%B6%E5%90%8E%E5%BE%80%E4%B8%8B%E6%8B%89%20%E5%BC%80%E5%90%AFIMAP%2FSMTP%E6%9C%8D%E5%8A%A1%20%E5%BC%80%E5%90%AF%E6%88%90%E5%8A%9F%E5%BE%97%E5%88%B0%E6%8E%88%E6%9D%83%E5%AF%86%E7%A0%81%2C%E8%BF%99%E4%B8%AA%E8%A6%81%E8%AE%B0%E4%BD%8F%2C%E4%B8%80%E4%BC%9A%E7%94%A8,3.%E9%85%8D%E7%BD%AEapplication.properties%20...%204%204.%E5%88%9B%E5%BB%BA%E5%8F%91%E9%80%81%E7%94%B5%E5%AD%90%E9%82%AE%E7%AE%B1controller%20...%205%205.%E6%9F%A5%E7%9C%8B%E6%95%88%E6%9E%9C%20)
