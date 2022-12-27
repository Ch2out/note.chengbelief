

# mybatis基础知识

## [返回首页](/README.md)

[官网文档](/%E5%90%8E%E7%AB%AF/mybaits/mybatis/%E5%AE%98%E7%BD%91mybatis.pdf)
[笔记总结](https://blog.csdn.net/CherryChenieth/article/details/123237754?spm=1001.2014.3001.5501)

### 基本知识

java知识补充

>在一个实体类中什么叫属性?
>在目前来说 成员变量我们就可以理解成为属性 
>当随着后面的学习 我们通过框架获取获取 通过反射get/set来进行bean属性的注入的时候 我们看到 应该看到get名称/set名称 此时去掉set和get后的名称 便可以称为属性 在这个角度上 会遇到 成员变量和get/set后名称不同的情况 此时我们不能说set名称这个名称不是属性

3:[MyBatis下载地址](https://github.com/mybatis/mybatis-3)

#### 5:mybatis依赖

``` xml
username=?
password=?
driver=com.mysql.cj.jdbc.Driver
url=jdbc:mysql://localhost:3306/mybaties
<dependencies>
<!-- Mybatis核心 -->
  <dependency>
    <groupId>org.mybatis</groupId>
    <artifactId>mybatis</artifactId>
    <version>3.5.7</version>
  </dependency>
<!-- MySQL驱动 -->
  <dependency>
    <groupId>mysql</groupId>
    <artifactId>mysql-connector-java</artifactId>
    <version>8.0.27</version>
  </dependency>
</dependencies>
```

#### 8mybatis核心配置文件

``` xml
<!-- 指xml版本号 编码字符集-->
<?xml version="1.0" encoding="UTF-8" ?>
<!-- 文件约束 -->
<!DOCTYPE configuration
  PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
  "http://mybatis.org/dtd/mybatis-3-config.dtd">
<!-- 跟标签 -->
<configuration>
<!--设置连接数据库的环境 default:默认选择的数据连接的地址环境--> 
    <environments default="development">
    <!-- 环境一 可以有多个environment 在需要那个就将default换成那给  -->
      <environment id="development">
      <!-- transactionManager：设置事务管理方式
        属性：
        type：设置事务管理方式，type="JDBC|MANAGED"
        type="JDBC"：设置当前环境的事务管理都必须手动处理
        type="MANAGED"：设置事务被管理，例如spring中的AOP
      -->
          <transactionManager type="JDBC"/>
          <!-- dataSource：设置数据源
            属性：
            type：设置数据源的类型，type="POOLED|UNPOOLED|JNDI"
            type="POOLED"：使用数据库连接池，即会将创建的连接进行缓存，下次使用可以从
            缓存中直接获取，不需要重新创建
            type="UNPOOLED"：不使用数据库连接池，即每次使用连接都需要重新创建
            type="JNDI"：调用上下文中的数据源
            -->
          <dataSource type="POOLED">
              <property name="driver" value="com.mysql.jdbc.Driver"/>
              <property name="url"value="jdbc:mysql://localhost:3306/MyBatis"/>
              <property name="username" value="root"/>
              <property name="password" value="123456"/>
          </dataSource>
      </environment>
   </environments>
  <!--引入映射文件-->
    <mappers>
    <!-- 单个映射文件 一共mybatis可以引入多个mapper文件 -->
         <mapper resource=""/>
    </mappers>
</configuration>
```

#### 10:创建mapper对应的映射文件

每个xml中的sql语句对应唯一的方法名 方法id 不能重复

每个mapper.xml 的名字必须要和我们对应的mapper类做对照

``` xml
<?xml version="1.0" encoding="UTF-8" ?>

<!DOCTYPE mapper
PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
"http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<!-- 每个mapper对应每个mapper接口  namespace 对应  mapper所在的全接口名-->
<mapper namespace="org.mybatis.example.BlogMapper">
<!-- 执行的语句 id为方法名 -->
    <select id="selectBlog" resultType="Blog">
    select * from Blog where id = #{id}
    </select>
</mapper>
```

#### 11:如何在普通类中使用mybaties框架

``` java
<!-- 将mybatis核心文件 以输入流的方式获取 -->
InputStream resourceAsStream = Resources.getResourceAsStream("mybatisConfig.xml");
<!-- 创建SqlSeeion 数据库会话工厂构建对象 -->
SqlSessionFactoryBuilder sqlSessionFactoryBuilder = new SqlSessionFactoryBuilder();
<!-- 通过SqlSeeion 数据库会话工厂构建对象.build(核心文件流) 此过程中核心文件会自动把其他配置文件通过mappers引入-->
SqlSessionFactory sqlSessionFactory = sqlSessionFactoryBuilder.build(resourceAsStream);
<!-- 通过sqlseesionFactory.openSession获取sqlSeeion对象 (true:表述是否需要自动提交 默认为false 需要自己手动提交)  -->
SqlSession sqlSession = sqlSessionFactory.openSession(true);
<!-- 在通过getMapper 获取指定的 UserMapperclass 他会自己实现那个接口的子类来调用mapper中同id的语句 -->
UserMapper mapper = sqlSession.getMapper(UserMapper.class);
```

#### 12：加入log4j日志输出文件

只需要在加入内路径 默认自动输出与数据库的交互

``` xml
<!-- maven依赖 -->
<dependency>
  <groupId>log4j</groupId>
  <artifactId>log4j</artifactId>
  <version>1.2.17</version>
</dependency
```

>xml配置
>日志的级别
>FATAL(致命)>ERROR(错误)>WARN(警告)>INFO(信息)>DEBUG(调试)
>从左到右打印的内容越来越详细

``` xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE log4j:configuration SYSTEM "log4j.dtd">

<log4j:configuration xmlns:log4j="http://jakarta.apache.org/log4j/">
    <appender name="STDOUT" class="org.apache.log4j.ConsoleAppender">
      <param name="Encoding" value="UTF-8" />
      <layout class="org.apache.log4j.PatternLayout">
          <param name="ConversionPattern" value="%-5p %d{MM-dd HH:mm:ss,SSS}
          %m (%F:%L) \n" />
      </layout>
    </appender>

    <logger name="java.sql">
         <level value="debug" />
    </logger>
    <logger name="org.apache.ibatis">
        <level value="info" />
    </logger>
    <root>
        <level value="debug" />
        <appender-ref ref="STDOUT" />
    </root>
</log4j:configuration>
```

#### 14:单查询 集合查询 如何获取查询的值 

<!-- 执行的语句 id为方法名 -->

``` xml
<!-- 
  在我们通过查询需要返回Bean对象时 我们需要在语句中指定选择合适的给bean注入
  resultType：自动映射，用于属性名和表中字段名一致的情况
  resultMap：自定义映射，用于一对多或多对一或字段名和属性名不一致的情况
-->
<select id="selectBlog" resultType="包名+bean类">
  select * from Blog where id = #{id}
</select>
```

#### 15:关于mybatisCogfig配置

核心配置文件中的标签必须按照固定的顺序：

>//如果没有声明其中的标签将无所谓<br>properties?,settings?,typeAliases?,typeHandlers?,objectFactory?,objectWrapperFactory?,reflectorFactory?,plugins?,environments?,databaseIdProvider?,mappers?

#### 16:在mybatisCofig引入properties

``` xml
<!--引入properties文件，此时就可以${属性名}的方式访问属性值-->
<properties resource="jdbc.properties"></properties>
<!-- 引入后我们再需要引用的地方通过${key}-->
```

#### 17:在mybatisConfig  标签typeAliases

>用来处理当mapper中有大量的select 需要返回pojo对象 导致代码大量冗余

``` xml
  <typeAliases>
    <!--
        typeAlias：设置某个具体的类型的别名
        属性：
        type：需要设置别名的类型的全类名
        alias：设置此类型的别名，若不设置此属性，该类型拥有默认的别名，即类名且不区分大小
        写
        若设置此属性，此时该类型的别名只能使用alias所设置的值
    -->
  <typeAlias type="com.atguigu.mybatis.bean.User"></typeAlias>-->
      <typeAlias type="com.atguigu.mybatis.bean.User"alias="abc"></typeAlias>
      <!--以包为单位，设置改包下所有的类型都拥有默认的别名，即类名且不区分大小写-->
      <package name="com.atguigu.mybatis.bean"/>
  </typeAliases>
```

#### 18:在mybatisConfig引入mapper

``` xml
<mappers>
    <mapper resource="UserMapper.xml"/>
      <!--
      以包为单位，将包下所有的映射文件引入核心配置文件
      注意：
      1此方式必须保证mapper接口和mapper映射文件必须在相同的包下
      2mapper接口的名字和引进的mapper.xml同名这样才能找到绑定
      -->
    <package name="com.atguigu.mybatis.mapper"/>
</mappers>
```

### select详解

####  如何将参数注入到sql语句中

``` java
MyBatis获取参数值的两种方式：${}和#{}
${}的本质就是字符串拼接，#{}的本质就是占位符赋值
// 使用例子
在通过properties引入的值需要${}
在需要动态sql where 1 = 1 
${}使用字符串拼接的方式拼接sql，若为字符串类型或日期类型的字段进行赋值时，需要手动加单引号;
// 正常情况都使用拼接
#{}使用占位符赋值的方式拼接sql，此时为字符串类型或日期类型的字段进行赋值时，可以自
动添加单引号
```

本质上 就是将传入的参数全部以key-value存人到一个map中

#### 1：多半使用直接传入一个实体对象 2：使用@param注解方法（对数组，集合）3:自己传入一个map

1：当只有一个参数时则key不指定 随意使用 **使用2**
2：当大于一个 就会使用默认的 agr0 agr1 或者parmie1 parmie2  **使用2**
3：当参数传人一个map便可以使用我们自己规定的key 来获取value **使用3**
4:当参数传入一个user 在底层会将bean对象自动转成一个map 我们可以通过get属性名的属性名 来获取需要的value值  **使用1**
5:通过@Param注解 来指定#{}中所引用的名称
因为在没有写明情况下默认使用value属性
@Param(value = "username") 或 @Param  ("username") 引用时便可以通过#{username} 来直接调用传进来的参数值 大大优化了第二种写法

@param注解详解

>

#### 查询返回结果功能介绍

当需要什么类型的时 便直接使用指定类型resultType 
需要全类名 如自定义实体类 包名.实体类名

>如果是java八大基本数据类型 可以直接使用(因为在Configuration XML中的 typeAliases 由mybaties 另外命名)
>如果需要返回的结果为单个实体类的 为map 便是(属性名:属性值)
>返回是集合 :
>一：可以用 List<Map<String,Object>> 
>二：需要使用map @mapkey 这样会将查询的结果返回的一个值作为key  将resultType指定的类型 作为value 放入map中

#### 查询条件总需要使用${}的各种情况

``` java
一：当我们使用like 的情况 因为ilke是模拟查询'%%'需要放入在里面 如果这个时候我们使用#{} 他会解析成 '%?%' 他会出现 强行注入参数的问题 可以使用${}
二：批量删除在where id in(#{})参数时 用#{}会自动帮我们添加''会导致无法查询到需要条件 使用${}便满足条件
三：当我们需要动态的选择表名时 由于表名不能添加''所以我们智能使用${}
```

我们如何在mybaties获取我们当前添加的自增长列的值 
在指定的sql语句中添加  
useGeneratedKeys="true"-获取自增长列的值  默认为false 
keyProperty="" 填入我们自增长列的字段名 便会将自增长列的值赋值到自增长字段名的属性值

#### 当查询结果的字段和实体类Bean属性不同的处理方法

1 通过给查询语句的字段名重命名 将和bean属性不同的字段名重名名为属性名（老方法）
2 通过在mybatis-config.xml添加 《settings》标签
name="mapUnderscoreToCamelCase" value ="true"
作用 ：将数据库返回字段中字段名带有_ 如t_user 将下划线变成驼峰命名 tUser 则会变成如下 默认为false
3 通过resultMap来指定返回的值

``` java
<!-- id填入和查询语句绑定的句子  type会返回的类型  -->
    使用resultMap时必须将主键包括其他所有字段名全部写人 不能单写只需要改的单字段
    <resultMap id="" type="">
    <!-- id 主键 -->
        <id column="eid" property="eid"></id>
        <!-- 主键外其他的字段名 -->
        <result column="emp_name" property="empName"></result>
        <result column="email" property="email"></result>
        <result column="age" property="age"></result>
        <result column="sex" property="sex"></result>
    </resultMap>
    <!-- colunm表述数据库中的字段名 property表述bean中的属性bean -->
```

#### 遇到多对一的情况时的处理办法

如部门和员工  我们在员工对象中加入虚拟属性(指不在数据库中真实存在) 部门对象，我们需要用到连接查询，我们如何将查询结果返回成我们的员工对象
一：通过直接咋resultMap中级联方式处理映射关系

``` java
// 通过给员工中的虚拟对象的属性 采取级联赋值的方法直接注入
 <result column="did" property="dept.did"></result>
 <result column="dname" property="dept.dname"></result>
```

二：通过使用用association处理映射关系

``` java
// 通过association标签 
//将property对应我们需要赋值的虚拟对象属性 
//javaType ：指定我们存在的java对象类型 包名+类名 
<association property="dept" javaType="Dept">
    <id column="did" property="did"></id>
    <result column="dname" property="dname"></result>
</association>
```

三：通过分步查询（重点）
详细介绍：用员工和部门举例子

一：首先需要在员工和部门都创建一个mapper，并且为mapper创造合适的xml文件
二：在resultMap中，我们通过mapper绑定的是第一步的查询

``` java
// 查询过程：通过eid查询出来员工-->通过员工的did查询出来部门信息-->最后一起返回成Emp对象
<resultMap id="empDeptStepMap" type="Emp">
    <id column="eid" property="eid"></id>
    <result column="ename" property="ename"></result>
    <result column="age" property="age"></result>
    <result column="sex" property="sex"></result>

    <association property="dept"
    // select：设置分步查询，查询某个属性的值的sql的标识（namespace.sqlId）mapper所在的地址 + 对应的mapper 语句的id 
    // column：将sql以及查询结果中的某个字段设置为分步查询的条件
        select="com.atguigu.MyBatis.mapper.DeptMapper.getEmpDeptByStep" column="did">
    </association>
</resultMap>    
<select id="getEmpByStep" resultMap="empDeptStepMap">
    select * from t_emp where eid = #{eid}
</select>
```

为什么使用分步查询 ：延时加载

> 当我们通过分步查询外 我们还需要在mybatis-config中的setting标签中设置两个属性
> lazyLoadingEnabled：延迟加载的全局开关。当开启时，所有关联对象都会延迟加载 默认为false
> aggressiveLazyLoading：当开启时，任何方法的调用都会加载该对象的所有属性。 否则，每个属性会按需加载 默认为false </br>
> 此时就可以实现按需加载，获取的数据是什么，就只会执行相应的sql。此时可通过association和collection中的fetchType属性设置当前的分步查询是否使用延迟加载，fetchType="lazy(延迟加
> 载)|eager(立即加载)"

根据描述为了按需加载，暂时想象不到使用场景

#### 遇到一对多的情况时的处理办法

如在部门中需要有员工数组的情况(不会与 员工有虚假部门对象这种情况同时发生)
一；通过collection来处理

``` java
// 在resultMap中
//property：指向需要赋值的虚拟属性 
//ofType：设置collection标签所处理的集合属性中存储数据的类型
<collection property="emps" ofType="Emp">
// 和查询到的数据依次排列
    <id property="eid" column="eid"></id>
    <result property="ename" column="ename"></result>
    <result property="age" column="age"></result>
    <result property="sex" column="sex"></result>
</collection>

```

二：分步查询 同样支持延时查询
使用前提 需要清楚的知道如何通过分步查询来实现 

``` java
//property：指向需要赋值的虚拟属性 
<collection property="emps" 
// 是否延时加载
fetchType="eager"
// 指向唯一的mapper中的查询语句
select="com.atguigu.MyBatis.mapper.EmpMapper.getEmpListByDid" 
// 需要的参数
column="did">

```
### mybatis知识补充

* [动态sql](#sql)
* [mybaits的缓存](#myBatisCache)
* [mybaits逆向工程](#reverseengineering)
* [mybaits分页插件](#page)

#### <span id="sql">动态sql</sapn>

存在意义：我们通过前端获取数据来写满足用户需求的sql语句,这时如果用户提交的条件不同如果没有动态sql语句我们就需要自己取窜写非常麻烦，拥有动态sql后我们可以通过写一个OA对象来满足所有的where条件
动态sql标签写在sql语句中

#### if标签

if标签可通过test属性的表达式进行判断，若表达式的结果为true，则标签中的加入sql语句；反之标签中
的内容不会加入

``` java
    <select id="getEmpListByMoreTJ" resultType="Emp">
    //在没有使用where标签时 我们为了处理前面包括and我们通过在where 1=1 因为结果肯定为真所以对结果不会有影响 
        select * from t_emp where 1=1
        <if test="ename != '' and ename != null">
            and ename = #{ename}
        </if>
    </select>
```

#### where标签

where和if一般结合使用：
a>若where标签中的if条件都不满足，则where标签没有任何功能，即不会添加where关键字
b>若where标签中的if条件满足，则where标签会自动添加where关键字，并将条件最前方多余的
and去掉
注意：where标签不能去掉条件最后多余的and

``` java
    <select id="getEmpListByMoreTJ" 
    resultType="Emp">
    //在没有使用where标签时 我们为了处理前面包括and我们通过在where 1=1 因为结果肯定为真所以对结果不会有影响 
        select * from t_emp where 1=1
        <where>
          <if test="ename != '' and ename != null">
            and ename = #{ename}
           </if>
        </where>

    </select>
```

#### trim标签

>trim用于去掉或添加标签中的内容
常用属性：
prefix：在trim标签中的内容的前面添加某些内容
prefixOverrides：在trim标签中的内容的前面去掉某些内容
suffix：在trim标签中的内容的后面添加某些内容
suffixOverrides：在trim标签中的内容的后面去掉某些内容
**相比于where更加的灵活可以处理  ename = #{ename} and 的情况**

#### choose、when、otherwise

choose 由这个包围、when：if...else、otherwise ：else

``` java
    <choose>
      <when test="empName != null and empName !='' ">
         emp_name = #{empName}
      </when>
      <when test="sex != null and sex != ''">
         or sex = #{sex} and
      </when>
      <otherwise>
      // 可以不写死 
          did=1
      </otherwise>
    </choose>
```

#### foreach标签

>属性：
collection：设置要循环的数组或集合
item：表示集合或数组中的每一个数据
separator：设置循环体之间的分隔符
open：设置foreach标签中的内容的开始符
close：设置foreach标签中的内容的结束符
用来处理批量操作 如批量删除，批量增加等

``` java
 <insert id="insertMoreEmp">
        insert into t_emp values
        //如果遇到集合或者是数组 我们将取出来的对象 使用 item.属性名的方法来获取对象
        <foreach collection="emps" item="emp" separator=",">
            (null,#{emp.ename},#{emp.age},#{emp.sex},#{emp.email},null)
        </foreach>
  </insert>
```

#### sql标签

sql片段，可以记录一段公共sql片段，在使用的地方通过include标签进行引入

``` xml
<sql id="empColumns">
eid,ename,age,sex,did
</sql>
select <include refid="empColumns"></include> from t_emp
```

#### <span id="myBatisCache">mybatis缓存功能</sapn>

mybatis缓存意义 ： 我们通过查询(也仅需要对查询结果进行缓存)来得到的数据，可能在一段时间里重复使用，通过缓存将数据存入内存效率绝对比通过数据库中访问的效率高
**一级缓存**
>默认启动一级缓存不需要额外设置
作用范围：在同一个sqlseeion中生效
使一级缓存失效的四种情况：
1 不同的SqlSession对应不同的一级缓存
2 同一个SqlSession但是查询条件不同
3 同一个SqlSession两次查询期间执行了任何一次增删改操作
4 同一个SqlSession两次查询期间手动清空了缓存

#### 二级缓存

>默认启动二级缓存在需要的mapper中添加cache标签
作用范围：二级缓存是SqlSessionFactory级别
Serializable
二级缓存是SqlSessionFactory级别，通过同一个SqlSessionFactory创建的SqlSession查询的结果会被
缓存；此后若再次执行相同的查询语句，结果就会从缓存中获取
二级缓存开启的条件：
1.在核心配置文件中，设置全局配置属性cacheEnabled="true"，默认为true，不需要设置
2.在映射文件中设置标签cache
3.二级缓存必须在SqlSession关闭或提交之后有效
4.查询的数据所转换的实体类类型必须实现序列化的接口
使二级缓存失效的情况：
两次查询之间执行了任意的增删改，会使一级和二级缓存同时失效

配置信息
>在mapper配置文件中添加的cache标签可以设置一些属性：
eviction属性：缓存回收策略
LRU（Least Recently Used） – 最近最少使用的：移除最长时间不被使用的对象。
FIFO（First in First out） – 先进先出：按对象进入缓存的顺序来移除它们。
SOFT – 软引用：移除基于垃圾回收器状态和软引用规则的对象。
WEAK – 弱引用：更积极地移除基于垃圾收集器状态和弱引用规则的对象。
默认的是 LRU。
flushInterval属性：刷新间隔，单位毫秒
默认情况是不设置，也就是没有刷新间隔，缓存仅仅调用语句时刷新
size属性：引用数目，正整数
代表缓存最多可以存储多少个对象，太大容易导致内存溢出
readOnly属性：只读，true/false
true：只读缓存；会给所有调用者返回缓存对象的相同实例。因此这些对象不能被修改。这提供了
很重要的性能优势。
false：读写缓存；会返回缓存对象的拷贝（通过序列化）。这会慢一些，但是安全，因此默认是
false。

**MyBatis缓存查询的顺序**
先查询二级缓存，因为二级缓存中可能会有其他程序已经查出来的数据，可以拿来直接使用。
如果二级缓存没有命中，再查询一级缓存
如果一级缓存也没有命中，则查询数据库
SqlSession关闭之后，一级缓存中的数据会写入二级缓存  (先在大范围中寻找 找不到再去小范围 实在没有查询数据库)

**[配置第三方缓存和slf4j日志](https://blog.csdn.net/CherryChenieth/article/details/123227137)**

#### <span id="reverseengineering">mybaits逆向工程</sapn>

**正向工程**：先创建Java实体类，由框架负责根据实体类生成数据库表。Hibernate是支持正向工程
的。
**逆向工程**：先创建数据库表，由框架负责根据数据库表，反向生成如下资源：
>Java实体类
Mapper接口
Mapper映射文件

1:先在maven中添加依赖和插件

``` java
<!-- 实现逆向工程需要依赖mybatis -->
<!-- 依赖MyBatis核心包 -->
<dependency>
    <groupId>org.mybatis</groupId>
    <artifactId>mybatis</artifactId>
    <version>3.5.7</version>
</dependency>
        <!-- 控制Maven在构建过程中相关配置 -->
<build>
<!-- 构建过程中用到的插件 -->
    <plugins>
        <!-- 具体插件，逆向工程的操作是以构建过程中插件形式出现的-->
        <plugin>
            <groupId>org.mybatis.generator</groupId>
            <artifactId>mybatis-generator-maven-plugin</artifactId>
            <version>1.3.0</version>
            <!-- 插件的依赖 -->
            <dependencies>
                <!-- 逆向工程的核心依赖 -->
                <dependency>
                    <groupId>org.mybatis.generator</groupId>
                    <artifactId>mybatis-generator-core</artifactId>
                    <version>1.3.2</version>
                </dependency>
                <!-- 数据库连接池 -->
                <dependency>
                    <groupId>com.mchange</groupId>
                    <artifactId>c3p0</artifactId>
                    <version>0.9.2</version>
                </dependency>
                <!-- MySQL驱动 -->
                <dependency>
                    <groupId>mysql</groupId>
                    <artifactId>mysql-connector-java</artifactId>
                    <version>5.1.8</version>
                </dependency>
            </dependencies>
        </plugin>
    </plugins>

</build>
```

2 创建逆向工程的配置文件
文件名必须为：generatorConfig.xml

``` xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE generatorConfiguration
        PUBLIC "-//mybatis.org//DTD MyBatis Generator Configuration 1.0//EN"
        "http://mybatis.org/dtd/mybatis-generator-config_1_0.dtd">
<generatorConfiguration>
    <!--
    targetRuntime: 执行生成的逆向工程的版本
    MyBatis3Simple: 生成基本的CRUD（清新简洁版）
    MyBatis3: 生成带条件的CRUD（奢华尊享版）
    -->
    <context id="DB2Tables" targetRuntime="MyBatis3Simple">
        <!-- 数据库的连接信息 -->
        <jdbcConnection driverClass="com.mysql.jdbc.Driver"
                        connectionURL="jdbc:mysql://localhost:3306/mybaties"
                        userId="root"
                        password="123456">
        </jdbcConnection>
        <!-- javaBean的生成策略-->
        <javaModelGenerator targetPackage="com.cheng.mybatis.bean"
                            targetProject=".\src\main\java">
            <property name="enableSubPackages" value="true"/>
            <property name="trimStrings" value="true"/>
        </javaModelGenerator>
        <!-- SQL映射文件的生成策略 -->
        <sqlMapGenerator targetPackage="com.cheng.mybatis.mapper"
                         targetProject=".\src\main\resources">
            <property name="enableSubPackages" value="true"/>
        </sqlMapGenerator>
        <!-- Mapper接口的生成策略 -->
        <javaClientGenerator type="XMLMAPPER"
                             targetPackage="com.cheng.mybatis.mapper" targetProject=".\src\main\java">
            <property name="enableSubPackages" value="true"/>
        </javaClientGenerator>
        <!-- 逆向分析的表 -->
        <!-- tableName设置为*号，可以对应所有表，此时不写domainObjectName -->
        <!-- domainObjectName属性指定生成出来的实体类的类名 -->
        <table tableName="t_emp" domainObjectName="Emp"/>
        <table tableName="t_dept" domainObjectName="Dept"/>
    </context>
</generatorConfiguration>
```

3 执行MBG插件的generate目标便能成功 <br>
**复杂版逆向工程**
简单来说 简单版的逆向工程为我们提供了基本的增删改查无法完成复杂的业务需要，所以出现复杂版
主要变化 ：多了一个BeanExample对象，根据链式来完成复杂的请求，个人觉得不如写动态语句直观
<u>[*详细说明*](https://blog.csdn.net/CherryChenieth/article/details/123232304)</u>

#### <span id="page">分页插件</sapn>

1添加依赖

```xml
    <dependency>
        <groupId>com.github.pagehelper</groupId>
        <artifactId>pagehelper</artifactId>
        <version>5.2.0</version>
    </dependency>
```

2:mybatisconfig.xml的配置文件中插入插件

```xml
    <plugins>
        <!--设置分页插件-->
        <plugin interceptor="com.github.pagehelper.PageInterceptor"></plugin>
    </plugins>
```

3-分页插件的使用
        a>在查询功能之前使用PageHelper.startPage(int pageNum, int pageSize)开启分页功能
        pageNum：当前页的页码
        pageSize：每页显示的条数<br>
        b>在查询获取list集合之后，使用PageInfo<T> pageInfo = new PageInfo<>(List<T> list, int navigatePages)获取分页相关数据
    list：分页之后的数据
    navigatePages：导航分页的页码数
    c>分页相关数据

``` java
    PageInfo{
    pageNum=8, pageSize=4, size=2, startRow=29, endRow=30, total=30, pages=8,

    list=Page{count=true, pageNum=8, pageSize=4, startRow=28, endRow=32, total=30,
    pages=8, reasonable=false, pageSizeZero=false},
    
    prePage=7, nextPage=0, isFirstPage=false, isLastPage=true, hasPreviousPage=true,
    hasNextPage=false, navigatePages=5, navigateFirstPage4, navigateLastPage8,
    navigatepageNums=[4, 5, 6, 7, 8]
    }
```

常用数据：
    pageNum：当前页的页码
    pageSize：每页显示的条数
    size：当前页显示的真实条数
    total：总记录数
    pages：总页数
    prePage：上一页的页码
    nextPage：下一页的页码
    isFirstPage/isLastPage：是否为第一页/最后一页
    hasPreviousPage/hasNextPage：是否存在上一页/下一页
    navigatePages：导航分页的页码数
    navigatepageNums：导航分页的页码，[1,2,3,4,5]

<hr>

### 常见问题

####  MyBatis获取自增长ID（当插入的时候我们需要获取当前插入的数据是自增长第几个时的处理方式）

useGeneratedKeys：（仅对 insert 和 update 有用）这会让MyBatis使用JDBC的getGeneratedKeys方法获取自增长ID,默认值false.

keyProperty：（仅对 insert 和 update 有用）唯一标记一个属性。MyBatis会通过getGeneratedKeys的返回值若希望得到多个生成的列，用逗号分隔属性名称.

useGeneratedKeys设置true，默认false；keyProperty设置生成的自增长值所绑定的属性，如参数对象User有id属性，设置keyProperty="id"即会将值绑定到参数对象User对应的属性上。

``` xml
<insert id="saveUser" parameterType="User" useGeneratedKeys="true" keyProperty="id">
	insert into t_user (user_name, user_sex) values (#{user_name, #{user_sex}})
</insert>
```



