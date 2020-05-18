# 4. MySQL数据库
## 4.1. 数据库基础
特点：
- 持久化存储数据，是一个文件系统
- 方便存储和管理数据
- 使用了统一的方法操作数据库

常见的数据库
- Oracle 大型数据库
- MySQL 开源的小型数据库
- Microsoft SQL Server 中型数据库
- Postgre SQL
- MongoDB
- DB2 常用于银行系统

## 4.2. MySQL
数据库：文件夹
表：文件
数据：数据

### 4.2.1. 什么是SQL？
结构化查询语言。定义了操作所有关系型数据库的规则。每一种数据库操作的方式存在不一样的地方，称为方言

### 4.2.2. 通用语法
- SQL语句可以单行或多行书写，以分号结尾。
- 不区分大小写，关键字建议使用大写。
- 3种注释

```
单行注释: -- (空格) 或#
多行注释： /* 注释 */
```

#### 4.2.2.1. SQL分类
1.DDL(Data Definition Language)数据定义语言
用来定义数据库对象：数据库，表，列等。关键字：create，drop，alter等

2.DML(Data Manipulation Language)数据操作语言
用来对数据库中表中的数据进行增删改。关键字：insert，delete，update等

3.DQL(Data Query Language) 数据查询语言
用来查询数据库中表的记录（数据）。关键字：select，where等

4.DCL(Data Control Language)数据控制语言（了解）
用来定义数据库的访问权限和安全级别，及创建用户。关键字：GRANT，REVOKE等

### 4.2.3. DDL 操作数据库、表
#### 4.2.3.1. 操作数据库：CRUD
- C(Create) 创建
    - 创建数据库 `create database 数据库名称;`
        - 判断数据库存不存在，不存在就创建`create database if not exists 数据库名`
        - 创建db4数据库，并且指定字符集gbk，判断是否存在
        `create database if not exists db4 character set gbk`
- R(Retrieve) 查询
    - 查询所有数据库的名称 `show databases;`
    - 查看某个数据库的字符集 查询 `show create database 数据库名字;`
- U(Update) 修改
    - 修改数据库的字符集
        - `alter database 数据名 character set gbk;`
- D(Delete) 删除
    - 删除数据库
        - `drop database 数据库名称;`
        - `drop database if exists db3;` 判断是否存在再删除
- 使用数据库
    - 查询当前正在使用的数据库名称 `select database();`
    - 使用数据库 `use 数据库名;`

#### 4.2.3.2. 操作表
- C(Create) 创建
    - 语法：
    
    ```
    create table 表名(
    列名1 数据类型1，
    列名2 数据类型2，
    ... 
    );
    ```
    - 数据类型：
        - int：整数类型
        - double：小数类型 `score double(5,2)`小数最多5位，小数点后面保留2位
        - date：日期类型，只包含年月日，yyyy-MM-dd
        - datetime：日期，包含年月日时分秒，yyyy-MM-dd HH:mm:ss
        - timestamp:时间戳类型，包含年月日时分秒，yyyy-MM-dd HH:mm:ss。**如果不给这个字段赋值，或赋值为null，会默认使用当前系统时间保存进去。**
        - varchar: 字符串类型 `name varchar(20)`最多20个字符
    - 复制表：`create table 表名 like 被复制的表名;`
      
```
create table student(
    id int,
    name varchar(32),
    age int,
    score double(4,1),
    birthday date,
    insert_time timestamp
);
```

- R(Retrieve) 查询
    - 查询某个数据库中的表名称 `show tables;`
    - 查询表结构 `desc 表名`
- U(Update) 修改
    - 修改表名
        - `alter table 表名 rename to 新的表名;`
    - 修改表的字符集
        - `alter table 表名 character set utf8;`
    - 添加一列
        - `alter table 表名 add 列名 数据类型;`
    - 修改列名称和类型
        - `alter table 表名 change 原列名 新表名 新数据类型;` 即改名字又改类型
        - `alter table 表名 modify 列名 新类型;` 只改类型
    - 删除列
        - `alter table 表名 drop 列名;`
- D(Delete) 删除
    - `drop table 表名;`
    - `drop table if exists 表名;`
- 使用数据库

### 4.2.4. DML 增删改表中数据
#### 4.2.4.1. 添加数据
语法：
`insert into 表名(列名1，列名2...) values(值1，值2，...)`

注意：
- 列名和值要一一对应
- 如果表名后，不定义列名，则默认给所有列添加值
- 除了数字类型，其他类型需要使用引号，单双都行

#### 4.2.4.2. 删除数据
语法：
`delete from 表名 [where 条件]`
`delete from stu where id=1;`

注意：
- 如果不写条件 会删除表中所有记录
- 如果要删除所有记录的方法
    - `delete from 表名;` 不推荐效率低
    - `truncate table 表名;` 先删除表再创建一张一样的表，效率高

#### 4.2.4.3. 修改数据
语法：
`update 表名 set 列名1=值1, 列名2 = 值2,... [where 条件]`

注意：
- 如果不写条件 会修改所有记录
- 

### 4.2.5. DQL查询表中的记录
`select * from  表名;` 

#### 4.2.5.1. 语法
- select
    - 字段名列表（可多个）
- from
    - 表名列表（可多个）
- where
    - 条件列表（可多个）
- group by
    - 分组字段
- having
    - 分组之后的条件
- order by
    - 排序
- limit
    - 分页限定

#### 4.2.5.2. 基础的查询
- 多个字段的查询
    - `select 字段名1，字段名2... from 表名`
    - `select name, age from student;`
- 去除重复
    - `select distinct 字段名 from 表名`
    - `select distinct address from student;`
- 计算列
    - 一般可以使用四则运算计算一些列的值，一般只会进行数值型的计算。
    - `ifnull(表达式1，表达式2)` 判断表达式1是否为null，如果是则替换为表达式2
    - `select name,math,english,math+english from student;`
    - 如果有null参与运算，计算结果都为null
    - `select name,math,english,math+IFNULL(english,0) from student;` 如果为null变为0
- 起别名
    - `select name 名字,math 数学,english 英语,math+IFNULL(english,0) AS 总分 from student;` AS或用一个空格都可以

#### 4.2.5.3. 条件查询
- where子句后跟条件
- 运算符
    - > < <= >= = <>(不等于)
      
        - `select * from student where age between 20 and 30;`
    - IN()
      
        - `select * from student where age IN (22,18,25);`
    - LIKE 模糊查询
        - 占位符 sql通配符
            - `_`:单个任意字符
            - `%`:多个任意字符
        - `select * from student where name like '马%';` 搜索姓马的
        - `select * from student where name like '_化%';` 搜索第二个字是化的
    - IS NULL
        - null不能使用等号去判断
        - `select * from student where english IS NULL;`
        - `select * from student where english IS NOT NULL;`
    - AND(推荐) 或 &&
    - OR(推荐) 或 ||
    - NOT(推荐) 或 !

#### 4.2.5.4. 排序查询
语法： order by 子句
`order by 排序字段1,排序方法1, 排序字段2,排序方法2... `
排序方式
- ASC 升序 默认
- DESC 降序

`select * from student order by math DESC;`

按数学成绩排名，如果一样就按照英语成绩排
`select * from student order by math ASC, english ASC;`

如果有多个排序条件，则当前面的条件值一样时，才会判断第二个条件

#### 4.2.5.5. 聚合函数
将一列数据作为一个整体，进行纵向的计算。比如求所有人数学成绩的平均分

注意：排除null值
- 用非空得了列（使用主键）
- 用IFNULL函数
- 用*

- count 计算个数
    - `select COUNT(name) from student;` 返回有几条记录。
    - `select COUNT(*) from student;`
- max 计算最大值
    - `select MAX(math) from student;`
- min 计算最小值
    - `select MIN(math) from student;`
- sum 计算和
    - `select SUM(english) from student;` 
- avg 计算平均值
    - `select AVG(math) from student;`

#### 4.2.5.6. 分组查询
语法：
`group by 分组字段;`

`select sex, AVG(english) from student group by sex;`

`select sex, AVG(english) from student where english>70 group by sex;` 只有成绩大于70的才会被用于计算平均值

`select sex, AVG(english), COUNT(id) from student where english>70 group by sex having COUNT(id)>2;` 限定人数大于2个才会显示 

`select sex, AVG(english), COUNT(id) 人数 from student where english>70 group by sex having 人数>2;`

注意：
- 分组之后查询的字段：分组字段、聚合函数
- where和having的区别
    - where在分组之前进行限定，如果不满足条件不分组。having在分组之后进行限定，如果不满足结果，则不会被查询出来
    - where后不可以跟聚合函数，having可以进行聚合函数的判断。

#### 4.2.5.7. 分页查询
同百度搜索结果太多需要分页。

语法
`limit 开始的索引, 每页查询的条数;`

第一页 查3条数据
`select * from student LIMIT 0,3;` 第一页

`select * from student LIMIT 3,3;` 第二页

公式：开始的索引 = (当前页码-1)*每页显示的条数

分页操作limit是一个“方言“ 这个limit只能在mysql中用，在别的数据库中不同

## 4.3. 约束
对表中的数据进行限定，保证数据的正确性，有效性和完整性。
分类
- 主键约束：primary key
- 非空约束：not null
- 唯一约束：unique
- 外键约束：foreign key

### 4.3.1. 非空约束 not null
1. 在创建表时添加约束 `name varchar(20) not null;`
2. 删除非空约束 `alter table stu modify name varchar(20);`
3. 创建完后添加非空约束 `alter table stu modify name varchar(20) not null;`

### 4.3.2. 唯一约束 unique
值不能重复
1. 在创建表时添加约束 `phone_number varchar(20) UNIQUE;`
2. **删除唯一约束 `alter table stu drop index phone_number;`**
3. 创建完后添加唯一约束 `alter table stu modify phone_number varchar(20) UNIQUE;`

注意：
数据为null不算，可以重复
如果数据已有重复，此时添加唯一约束会出错。

### 4.3.3. 主键约束
含义：非空且唯一
一张表只能有一个字段为主键，表中记录的唯一标识

1. 在创建表时添加主键约束 `id int primary key;`
2. 删除主键约束 `alter table stu drop primary key;`
3. 创建完后添加非空约束 `alter table stu modify phone_number varchar(20) primary key;`
4. 创建联合主键 `PRIMARY KEY(id, name)`

#### 4.3.3.1. 自动增长(一般与主键一起使用)
如果某一列是数值类型的，使用auto_increment可以来完成值得自动增长。

1. 在创建表时添加 `id int auto_increment;`
2. 删除自动增长 `alter table stu modify id int;`
3. 创建完后添加自动增长 `alter table stu modify id int auto_increment;`


给null就自动增长，但是也可以自己委派数值，其数值只与上一条有关系。
`insert into stu values(11,'ccc');`
`insert into stu values(null,'bbb');` 这一条的数值为12

### 4.3.4. 外键约束
数据有冗余的时候需要表的拆分。
**让表与表产生关系，从而保证数据的正确性**
比如一个员工表，里面有一个字段为部门的id。另外一个部门表，其中主键是部门id，还有部门名和部门位置。如果删除了一个部门，但是员工表中还有属于这个部门的员工。就会发生错误。

1. 在创建表时添加外键
    - 语法： `constraint 外键名称 foreign key (外键列名称) references 主表名称(主表列名称);`

    ```
    create table employee(
        id int primary key auto_increment,
        ....
        dep_id int,
        constraint emp_dept_fk foreign key (dep_id) references department(id)
    )

    此时去删department中的部分数据，会提示不让删除，因为有外键依赖
    也不能在employee中随意添加dep_id,必须department中存在的id才行
    ```
2. 删除外键 `alter table employee drop foreign key emp_dept_fk;`
3. 添加外键 `alter table employee add constraint emp_dept_fk foreign key dep_id references department(id);`

### 4.3.5. 级联操作
department中的id不能改，因为他已经关联到employee中的dep_id，所以要改的话需要使用级联操作

- 需要在添加外键的时候，设置级联更新
`constraint emp_dept_fk foreign key dep_id references department(id) on update cascade`
- 级联删除
`constraint emp_dept_fk foreign key dep_id references department(id) on delete cascade`

## 4.4. 数据库的设计
### 4.4.1. 学习多表之间的关系
#### 4.4.1.1. 1对1（了解）
如：人和身份证</br>
一个人只有一个身份证，一个身份证对应一个人

#### 4.4.1.2. 一对多（多对一）
如：部门和员工</br>
一个部门有多个员工，一个员工对应一个部门

#### 4.4.1.3. 多对多
如：学生和课程</br>
一个学生可以选择很多课程，一个课程也可以被很多学生选择

#### 4.4.1.4. 实现关系
- 一对多
  - 实现方式：在多的一方建立外键指向一的一方的主键。
    - 员工表（多）与部门表（一） 员工表中设置外键指向部门表的id
- 多对多的关系
  - 实现方式：建立一张中间表，至少包含两个多对多关系表的主键，作为第三张表的外键
- 一对一关系
  - 实现方式：在任意的一方添加外键，指向另一方的主键。并且让这个外键唯一。
  - 一般情况下合作一张表，不需要两张表

### 4.4.2. 数据库设计的范式
  概念：设计数据库时需要遵循的规范。

  > 设计关系数据库时，遵从不同的规范要求，设计出合理的关系型数据库，这些不同的规范要求被称为不同的范式，各种范式呈递次规范，越高的范式数据库冗余越小。</br>
  目前关系数据库有六种范式：第一范式（1NF）、第二范式（2NF）、第三范式（3NF）、巴斯-科德范式（BCNF）、第四范式(4NF）和第五范式（5NF，���称完美范式）。

| 学号    | 姓名  | 系名  | 系主任 | 课程名称  | 分数 |
|-------|-----|-----|-----|-------|----|
| 10010 | ���无忌 | 经济系 | 张三丰 | 高等数学  | 96 |
| 10010 | 张无忌 | 经济系 | 张三丰 | 大学英语  | 56 |
| 10011 | 令狐冲 | 法律系 | 任我行 | 计算机基础 | 77 |
| 10011 | 令狐冲 | 法律系 | 任我行 | 法理学   | 87 |
| 10012 | 杨过  | 法律系 | 任我行 | 大学英语  | 99 |

存在问题：
- 存在非常严重的数据冗余：姓名、系名、系主任
- 数据添加存在问题：添加新开设的系和系主任时，数据不合法
- 数据删除问题：张无忌毕业了，则删除数据，系的数据也会一起被删除

#### 4.4.2.1. 分类
1. 第一范式（1NF）：每一列都是不可分割的原子数据项
2. 第二范式（2NF）：在1NF的基础上，非码属性必须完全依赖于候选码（在1NF基础上消除非主属性对主码的部分函数依赖）
3. 第三范式（3NF）：在2NF基础上，任何非主属性不依赖于其它非主属性（在2NF基础上消除传递依赖）

所有能在数据库中创建出来的表都符合1NF。

#### 4.4.2.2. 第二范式
第二范式（2NF）：在1NF的基础上，非码属性必须完全依赖于候选码（在1NF基础上消除非主属性对主码的部分函数依赖）

概念：
- 函数依赖：A--->B，如果通过A属性（属性组）的值可以确定唯一B属性的值，则B依赖于A。
  - （学号--->学生名字） 
  - （学号，课程名称）--->分数
- 完全函数依赖： A--->B，如果A是一个属性组，则B属性值的确定需要依赖于A属性组中所有的属性值。 
  - （学号，课程名称）--->分数
- 部分函数依赖： A--->B,如果A是一个属性组，则B属性值的确定只需要依赖于A属性组中部分的属性值。 
  - （学号，姓名，课程名称）--->分数
- 传递函数依赖： A--->B，B--->C，如果A属性（属性组）的值可以确定唯一B属性的值，在通过B属性（属性组）的值可以确定唯一C属性的值。 
  - 学号--->系名，系名--->系主任
- 码：如果一张表中，一个属性或属性组，被其他所有属性所完全依赖，则这个属性（属性组）为码
  - （学号，课程名称）--->（姓名，系名，系主任，成绩）
- 主属性：码属性组中的所有属性
- 非主属性：除去码属性组的属性

分析：
- 主属性：学号，课程名称
- 消除姓名，系名，系主任，分数对学号，课程名称的部分依赖

| 学号    | 课程名称  | 分数 |
|-------|-------|----|
| 10010 | 高等数学  | 96 |
| 10010 | 大学英语  | 56 |
| 10011 | 计算机基础 | 77 |
| 10011 | 法理学   | 87 |
| 10012 | 大学英语  | 99 |

| 学号    | 姓名  | 系名  | 系主任 |
|-------|-----|-----|-----|
| 10010 | 张无忌 | 经济系 | 张三丰 |
| 10011 | 令狐冲 | 法律系 | 任我行 |
| 10012 | 杨过  | 法律系 | 任我行 |

存在问题：
- 数据添加存在问题：添加新开设的系和系主任时，数据不合法
- 数据删除问题：张无忌毕业了，则删除数据，系的数据也会一起被删除

解决问题：
- 存在非常严重的数据冗余：姓名、系名、系主任

#### 4.4.2.3. 第三范式
第三范式（3NF）：在2NF基础上，任何非主属性不依赖于其它非主属性（在2NF基础上消除传递依赖）

目前系名依赖于学号，系主任依赖于系名，因此系主任依赖于学号，传递依赖。

| 学号    | 课程名称  | 分数 |
|-------|-------|----|
| 10010 | 高等数学  | 96 |
| 10010 | 大学英语  | 56 |
| 10011 | 计算机基础 | 77 |
| 10011 | 法理学   | 87 |
| 10012 | 大学英语  | 99 |

| 学号    | 姓名  | 系名  |
|-------|-----|-----|
| 10010 | 张无忌 | 经济系 |
| 10011 | 令狐冲 | 法律系 |
| 10012 | 杨过  | 法律系 |

| 系名  |  系主任  |
|-----|-----|
| 经济系 | 张三丰 |
| 法律系 | 任我行 |

解决问题：
- 数据添加存在问题：添加新开设的系和系主任时，数据不合法
- 数据删除问题：张无忌毕业了，则删除数据，系的数据也会一起被删除

## 4.5. 数据库的备份和还原
### 4.5.1. 命令行
备份语法： `mysqldump -u用户名 -p密码 [指定数据库将名称] > 保存的路径`
还原语法：
- 登录数据库 `mysql -u用户名 -p用户名`
- 创建数据库 `create datebase 数据库名字`
- 使用数据库 `use 数据库名字`
- 执行文件 `source 文件路径`

## 4.6. 多表查询
查询语法：`select 列名列表 from 表名列表 where... `

### 4.6.1. 笛卡尔积
`select * from emp, dept;` 笛卡尔积 取A和B集合的所有组合情况

多表查询需要消除笛卡尔积中无用的数据

### 4.6.2. 多表查询的分类
- 内连接查询
- 外连接查询
- 子查询

#### 4.6.2.1. 内连接查询
- 隐式内连接：使用where条件消除无用数据
  - `select * from emp, dept where emp.dept_id = dept.id;`
  - `select emp.name, emp.gender, dept.name from emp, dept where emp.dept_id = dept.id;`


```sql
# 2. 标准写法
select 
    t1.name, # 员工表姓名
    t1.gender, # 员工表性别
    t2.name # 部门表名称
from
    emp t1, 
    dept t2
where
    t1.dept_id, 
    t2.id;
```

- 显示内连接
  - `select 字段列表 from 表名 inner join 表名2 on 条件;`
  - `select * from emp inner join dept on emp.dept_id=dept.id;`
  - `select * from emp join dept on emp.dept_id=dept.id;` INNER可以省略

- 注意事项：
  - 从哪些表中查询
  - 条件是什么
  - 查询哪些字段

#### 4.6.2.2. 外连接查询
- 左外链接
  - 查询的是左表所有记录，以及其交集部分
  - `select 字段列表 from 表1 left join 表2 on 条件;`

    ```sql
    # 查询所有员工信息，如果员工有部门，则查询部门名称，没有部门，则不显示部门名称
    # 如果用内连接，部门为null的不会被查询出来
    select 
        t1.*,
        t2.name
    from
        emp t1, 
    left join
        dept t2
    on
        t1.dept_id = t2.id;
    ```

- 右外链接
  - 查询的是右表所有记录，以及其交集部分
  - `select 字段列表 from 表1 right join 表2 on 条件;`

    ```sql
    # 查询所有员工信息，如果员工有部门，则查询部门名称，没有部门，则不显示部门名称
    # 如果用内连接，部门为null的不会被查询出来
    select 
        t1.*,
        t2.name
    from
        dept t1, 
    right join
        emp t2
    on
        t1.dept_id = t2.id;
    ```

### 4.6.3. 子查询
概念：查询中嵌套查询，称嵌套查询为子查询

> 比如查询最高工资的员工信息
> 1. 查询最高的工资是多少 9000
> 2. 查询员工信息，并且工资等于9000的

`select * from emp where emp.salary = (select max(salary) from emp);`

#### 4.6.3.1. 子查询的不同情况
- 结果是单行单列的
  - 可以作为条件，使用运算符去判断
    ```sql
    # 查询员工工资小于平均工资的人
    select * from emp where emp.salary <(select AVG(salary) from emp);
    ```
- 结果是多行单列的或单行多列的
  - 可以作为条件，使用运算符IN来判断
    ```sql
    #查询所有财务部和市场部的员工信息
    select * from emp where dept_id in (select id from dept where name='财务部' or name='市场部');
    ```
- 结果是多行多列的
  - 子查询可以作为一张虚拟表，进行表的查询
    ```sql
    # 查询2011-11-11日之后入职的员工信息和部门信息
    # 子查询
    select * from dept t1, (select * from emp where emp.join_date > '2011-11-11') t2 where t1.id=t2.dept_id;

    # 普通内连接
    select * from emp t1, dept t2 where t1.dept_id = t2.id and t1.join_date > '2011-11-11';
    ```

### 4.6.4. 练习
```sql
-- 部门表
CREATE TABLE dept (
    id INT PRIMARY KEY PRIMARY KEY, -- 部门id
    dname VARCHAR(50), -- 部门名称
    loc VARCHAR(50) -- 部门所在地
);

-- 添加4个部门
INSERT INTO dept(id,dname,loc) VALUES 
(10,'教研部','北京'),
(20,'学工部','上海'),
(30,'销售部','广州'),
(40,'财务部','深圳');



-- 职务表，职务名称，职务描述
CREATE TABLE job (
    id INT PRIMARY KEY,
    jname VARCHAR(20),
    description VARCHAR(50)
);

-- 添加4个职务
INSERT INTO job (id, jname, description) VALUES
(1, '董事长', '管理整个公司，接单'),
(2, '经理', '管理部门员工'),
(3, '销售员', '向客人推销产品'),
(4, '文员', '使用办公软件');



-- 员工表
CREATE TABLE emp (
    id INT PRIMARY KEY, -- 员工id
    ename VARCHAR(50), -- 员工姓名
    job_id INT, -- 职务id
    mgr INT , -- 上级领导
    joindate DATE, -- 入职日期
    salary DECIMAL(7,2), -- 工资
    bonus DECIMAL(7,2), -- 奖金
    dept_id INT, -- 所在部门编号
    CONSTRAINT emp_jobid_ref_job_id_fk FOREIGN KEY (job_id) REFERENCES job (id),
    CONSTRAINT emp_deptid_ref_dept_id_fk FOREIGN KEY (dept_id) REFERENCES dept (id)
);

-- 添加员工
INSERT INTO emp(id,ename,job_id,mgr,joindate,salary,bonus,dept_id) VALUES 
(1001,'孙悟空',4,1004,'2000-12-17','8000.00',NULL,20),
(1002,'卢俊义',3,1006,'2001-02-20','16000.00','3000.00',30),
(1003,'林冲',3,1006,'2001-02-22','12500.00','5000.00',30),
(1004,'唐僧',2,1009,'2001-04-02','29750.00',NULL,20),
(1005,'李逵',4,1006,'2001-09-28','12500.00','14000.00',30),
(1006,'宋江',2,1009,'2001-05-01','28500.00',NULL,30),
(1007,'刘备',2,1009,'2001-09-01','24500.00',NULL,10),
(1008,'猪八戒',4,1004,'2007-04-19','30000.00',NULL,20),
(1009,'罗贯中',1,NULL,'2001-11-17','50000.00',NULL,10),
(1010,'吴用',3,1006,'2001-09-08','15000.00','0.00',30),
(1011,'沙僧',4,1004,'2007-05-23','11000.00',NULL,20),
(1012,'李逵',4,1006,'2001-12-03','9500.00',NULL,30),
(1013,'小白龙',4,1004,'2001-12-03','30000.00',NULL,20),
(1014,'关羽',4,1007,'2002-01-23','13000.00',NULL,10);



-- 工资等级表
CREATE TABLE salarygrade (
    grade INT PRIMARY KEY,   -- 级别
    losalary INT,  -- 最低工资
    hisalary INT -- 最高工资
);

-- 添加5个工资等级
INSERT INTO salarygrade(grade,losalary,hisalary) VALUES 
(1,7000,12000),
(2,12010,14000),
(3,14010,20000),
(4,20010,30000),
(5,30010,99990);

-- 需求：
-- 1.查询所有员工信息。查询员工编号，员工姓名，工资，职务名称，职务描述
select emp.id, emp.ename, emp.salary, job.jname, job.description from emp, job where emp.job_id=job.id;

-- 2.查询员工编号，员工姓名，工资，职务名称，职务描述，部门名称，部门位置
select emp.id, emp.ename, emp.salary, job.jname, job.description, dept.dname, dept.loc from emp,job,dept where emp.job_id=job.id and emp.dept_id=dept.id;

-- 3.查询员工姓名，工资，工资等级
select emp.ename, emp.salary, salarygrade.grade from emp, salarygrade where emp.salary>salarygrade.losalary and emp.salary<salarygrade.hisalary;

-- 4.查询员工姓名，工资，职务名称，职务描述，部门名称，部门位置，工资等级
select emp.ename, emp.salary, job.jname, job.description, dept.dname, dept.loc, salarygrade.grade from emp,job,dept,salarygrade where emp.job_id=job.id and emp.dept_id = dept.id and emp.salary BETWEEN salarygrade.losalary and salarygrade.hisalary;

-- 5.查询出部门编号、部门名称、部门位置、部门人数
select dept.id,dept.dname,dept.loc,t2.total from dept,(select emp.dept_id,COUNT(id) total from emp group by dept_id) t2 where dept.id=t2.dept_id;

-- 6.查询所有员工的姓名及其直接上级的姓名,没有领导的员工也需要查询
select t1.ename, t1.mgr, t2.ename from emp t1 LEFT JOIN emp t2 on t1.mgr=t2.id;
```

## 4.7. 事务
### 4.7.1. 事务的基本介绍
#### 4.7.1.1. 概念
概念：如果一个包含多个步骤的业务操作被事务管理，这些操作要么同时成功，要么同时失败。

#### 4.7.1.2. 操作
开启事务：start transaction</br>
回滚：rollback</br>
提交：submit

```sql
CREATE TABLE account (
            id INT PRIMARY KEY AUTO_INCREMENT,
            NAME VARCHAR(10),
            balance DOUBLE
        );
        -- 添加数据
        INSERT INTO account (NAME, balance) VALUES ('zhangsan', 1000), ('lisi', 1000);


        SELECT * FROM account;
        UPDATE account SET balance = 1000;
        -- 张三给李四转账 500 元

        -- 0. 开启事务
        START TRANSACTION;
        -- 1. 张三账户 -500

        UPDATE account SET balance = balance - 500 WHERE NAME = 'zhangsan';
        -- 2. 李四账户 +500
        -- 出错了...
        UPDATE account SET balance = balance + 500 WHERE NAME = 'lisi';

        -- 发现执行没有问题，提交事务
        COMMIT;

        -- 发现出问题了，回滚事务
        ROLLBACK;
```

#### 4.7.1.3. 提交
事务是不会自动提交的，如果操作了事务，但是没有提交命令，关闭了数据库后会自动回滚。**但是如果改成了手动之后，输入了commit但是事务中的部分语句出错了，前面未错的还是会进行的，因此要手动回滚。**

两种提交方式
- 自动提交
  - mysql就是自动提交的，一条DML语句会自动提交一次
- 手动提交
  - oracle默认手动提交
  - 需要开启事务，再手动提交

修改事务的默认提交方式：
- 查看事务的默认提交方式 `select @@autocommit;` 1为自动提交 0为手动
- `set @@autocommit=0;` 设置手动提交

### 4.7.2. 事务的四大特征
- 原子性
  - 是不可分割的最小操作单位，要么同时成功，要么同时失败
- 持久性
  - 当事务提交或回滚后，数据库会持久化的保持数据
- 隔离性
  - 多个事物之间相互独立。
- 一致性
  - 事物操作前后数据总量不变

### 4.7.3. 事务的隔离级别（了解）
概念：多个事务之间是隔离的，相互独立的。但是多个事务操作同一批数据则会引发一些问题。设置不同的隔离级别可以解决这些问题。

存在的问题
- 脏读：一个事务，读取到另一个事务中没有提交的数据
- 不可重复度：在同一个事务中，两次读取到的数据不一样
- 幻读：一个事务操作（DML）数据表中所有记录，另一个事务添加了一条数据，则第一个事务查询不到自己的修改

隔离级别
- read uncommitted：读为提交
  - 产生的问题：脏读、不可重复读、幻读
- read committed：读已提交（Oracle默认）
  - 产生的问题：不可重复读、幻读
- repeatable read：可重复读 **（MySQL默认）**
  - 产生的问题：幻读
- serializable：串行化
  - 可以解决所有问题
  - 把表锁上，只有一个事务能执行

注意：隔离级别从小到大安全性越来越高，但是效率越来越低

操��
- 查询隔离级别 `select @@transaction_isolation;`
- 设置隔离级别 `set global transaction isolation level 级别字符串`

## 4.8. DCL
管理用户，并对用户进行授权

DBA:数据库管理员专门管理数据库。

### 4.8.1. 管理用户
- 用户添加
  - `create user '用户名'@'主机名' identified by '密码';`
  - 主机名用`@`表示任何电脑都可以登录
- 删除用户
  - `drop user '用户名'@'主机名';`
- 更改用户密码
  - `update user set password=password('新密码') where user = '用户名';`
  - `set password for '用户名'@'主机名' = password('新密码');`
  - 如果忘记了root用户的密码
    - `net stop mysql` 停止mysql服务，需要管理员身份
    - 使用无验证方式启动mysql服务 `mysqld --skip-grant-tables`，然后再打开一个终端直接输入`mysql`，进入数据库
    - `use mysql;`
    - `update user set password = password('新密码') where user='root';`
    - 手动结束mysqld进程
    - `net start mysql` 启动mysql服务
- 查询用户
  - 切换到mysql数据库 `use mysql;`
  - 查询user表 `select * from user;`


### 4.8.2. 授权
- 查询权限
  - `show grants for '用户名'@'主机名';`
- 授权权限
  - `grant 权限列表 on 数据库.表名 to '用户名'@'主机名';`
  - `grant ALL on *.* to '用户名'@'主机名';` 跟root一样的权限
- 撤销权限
  - `revoke 权限列表 on 数据库.表名 from '用户名'@'主机名';`

---
