[返回首页](/README.md)

# Maven笔记
<hr>
##### maven 子父模块
在IDEA中 项目(模块之间的继承中 )
1 首先根据 relativePath标签在用户指定的位置去寻找  注意：此标签是根据模块所在的物理地址 来进行选择 （在同一个工作区 则可以无视）
2  根据artifactId 在本地的maven仓库中寻找 可能遇到情况 maven中没有此模块 所以为红色 将此模块安装 然后在刷新即可
在IDEA进行聚合的时遇到问题 无法安装成功  情况一下两种 如果 没有把父程序导入在仓库中  一 将他导入仓库中 刷新 这样可以根据  artifactId找到 二 导入过 但实际上库中并没有这个项目 所以在进行聚合安装的时候就会出现   Non-resolvable parent POM for com.atguigu.maven:MakeFriends:0.0.1-SNAPSHOT: Could not find artifact com.atguigu.maven:mavenGun:pom:0.0.1-SNAPSHOT and 'parent.relativePath' points at wrong local POM @ line 11, column 13 -> [Help 2]的错误 解决方法 通过  relativePath标签在当前工作区中找到父程序的pom 即可 进行 聚合安装
聚合安装 会先从 父程序 然后到被依赖的程序 到依赖程序 设计的比较人性 哈哈
<hr>
##### 用maven跑web项目的大概流程
首先通过maven命令将javaweb项目打包成war 然后将maven项目的trager文件的war给他添加tomcat输出环境 然后 运行 当有修改时 首先complie 后 在通过 重新部署来修改代码