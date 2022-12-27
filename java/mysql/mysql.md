# mysql补充笔记

## 函数详解

### 聚合函数

>使用 avg() sum() count()  max()  min() 时默认不会将字段为null的加入运算
而我们需要求一个可能有null字段的平均值的时候
我们必须使用 sum(*)\conut(1)

#### where 函数的条件为什么不能是聚合函数

> 聚集函数也叫列函数，它们都是基于整列数据进行计算的，而where子句则是对数据行进行过滤的(这里过滤是在一个记录里边过滤的,基于"行")，在筛选过程中依赖“基于已经筛选完毕的数据得出的计算结果”是一种悖论，这是行不通的。更简单地说，因为聚集函数要对全列数据时行计算，因而使用它的前提是：结果集已经确定！
而where子句还处于“确定”结果集的过程中，因而不能使用聚集函数。

#### group by(更据指定字段分组)

>当我们遇到 group by函数时 select中出现的非聚合函数的字段必须声明在GRUOP BY中 如果出现非GRUOP BY 中的字段 或非 聚合函数的字段出现在select中是严重错误的   因为在gourp by中 除了聚合函数和broup by字段 其他的所有字段在分组中都是没有意义的>> 聚合与分组，如果select中出现了聚合函数列，那么所有不是聚合函数的列都必须出现在group by中

### order by(根据指定字段排序)

根据字段排序默认升序（ASC），如果需要选择倒序（DESC）
>如果遇到按年龄排序，年龄相等的按成绩排序，这种多重排序可以采取

``` sql
-- 同样可以使用正倒排序
SELECT * FROM user ORDER BY age , salary 或 
SELECT * FROM user ORDER BY age ORDER BY salary
```

### limit函数

> limit 0，5
> 0代表是从0这个索引开始，5代表的是查询的数量。
> limit 4
> 表示从0索引开始。查询的数量为4
> 分页实现
> limit (number-1)*size,size

## mysql特殊字段类型

### 时间类型

TIMESTAMP ：时间戳类型

INSERT INTO t_time VALUES ('2022-12-08 12:18:17.778')  可以通过'yyyy-MM-dd hh-mm-ss'插入
这个类型对应java中的 java.util.Date类。能通过getTime来进行比较

其他常见裂隙

| DATE     | 3   | 1000-01-01/9999-12-31                          | YYYY-MM-DD          | 日期值           |
| -------- | --- | ---------------------------------------------- | ------------------- | ---------------- |
| TIME     | 3   | '-838:59:59'/'838:59:59'                       | HH:MM:SS            | 时间值或持续时间 |
| YEAR     | 1   | 1901/2155                                      | YYYY                | 年份值           |
| DATETIME | 8   | '1000-01-01 00:00:00' 到 '9999-12-31 23:59:59' | YYYY-MM-DD hh:mm:ss | 混合日期和时间值 |

### boolea类型

MySQL `BOOLEAN`数据类型来存储布尔值：`true`和`false`。

MySQL没有内置的布尔类型。 但是它使用[TINYINT(1)](http://www.yiibai.com/mysql/int.html)。 为了更方便，MySQL提供`BOOLEAN`或`BOOL`作为`TINYINT(1)`的同义词。

在MySQL中，`0`被认为是`false`，非零值被认为是`true`。 要使用布尔文本，可以使用常量`TRUE`和`FALSE`来分别计算为`1`和`0`。

在和java之间使用时。完全能够通过true，false来进行赋值。并且在java获取数据库中数据时任然拿出来的是false和ture

### enum枚举类型

在MySQL中，ENUM是一个字符串对象(必须是字符串，不能是数值或boolean类型)，其值是从列创建时定义的允许值列表中选择的。
ENUM数据类型提供以下优点：
>紧凑型数据存储，MySQL ENUM使用数字索引(1，2，3，…)来表示字符串值。可读查询和输出。
要定义ENUM列，请使用以下语法：
CREATE TABLE table_name (
    ...
    col ENUM ('value1','value2','value3'),
    ...
);
在这种语法中，可以有三个以上的枚举值。但是，将枚举值的数量保持在20以下是一个很好的做法。
<hr>
下面来看看看下面的例子。
假设我们必须存储优先级为：low, medium 和 high 的票据信息。 要将priority列分配给ENUM类型，请使用以下CREATE TABLE语句：

USE testdb;
CREATE TABLE tickets (
    id INT PRIMARY KEY AUTO_INCREMENT,
    title VARCHAR(255) NOT NULL,
    priority ENUM('Low', 'Medium', 'High') NOT NULL
);

priority列只接受三个Low, Medium, High值。 在后台，MySQL将每个枚举成员映射到数字索引。在这种情况下，Low，Medium和High分别映射到1,2和3(注意：与数组不同，这不是从0开始的)。

#### 枚举类型在java中的使用

如果mysql是enum枚举类型（此时我们的java中映射到则可以是字符串）
如果java是enum枚举类型（此时mysql是int---)。我们就书写sql语句时，将mysql的类型值，通过javaenum类型.get属性来获取到值来显示或提交。（或者使用mybatis中的通用枚举类型，在对应的mysql类型上的java属性上添加
1.@EnumValue注解
2.在application.yaml 配置type-enums-package:来配置我们的enum所在的包。
