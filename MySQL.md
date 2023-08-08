[01-MySQL教程简介_哔哩哔哩_bilibili](https://www.bilibili.com/video/BV1iq4y1u7vj?p=1&vd_source=cd526129166eb01a825598d4f6e06479)

<a name="BskZ1"></a>
#  RDBMS 和 非 RDBMS
:::tips
<a name="ITuvg"></a>
### 关系型
二元数据存储模式, 也就是表

- 复杂查询
   - 多表查询的复杂query
- 事务支持, 安全性, 比如多事务		
<a name="ARUhi"></a>
### 非关系型

- 性能优势
- 各种目标, 比如mongo是Document, 还有搜索引擎
:::
<a name="KdK5R"></a>
# ER 模型与表记录的4种关系
:::tips
<a name="teVvf"></a>
### ER

- E-R (Entity-relationship) 实体集, 属性, 联系集
- 一个实体集对应一个 table, 一个实体对应数据表种一行 row, 也称为record, 一个属性(attribute) 对应着数据表的列 column, 更多被称为field 字段.
<a name="cKFFh"></a>
### 表的关联关系
**一对一, 一对多, 多对多, 自我引用**

1. 一对一
   1. 应用不多, 因为一对一可以创建成一张表
   2. 如果有些字段是不常用, 可以考虑分表, 让原始表有更少字段
   3. 原则: 外键唯一, 主表的主键和从表的外键
2. 一对多
   1. 常用场景: 客户和订单表, 分类表和商品表, 部门与员工表
3. 多对多
   1. 要创建第三个表, 称为**连接表**, 它将多对多关系划分为两个多对一关系, 将两个表的主键都插入到第三个表中<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/29051556/1690956684752-e9ca0c4a-3db6-4c10-8343-67357033fb34.png#averageHue=%23fafaf9&clientId=uab5443cc-1dfd-4&from=paste&height=176&id=ue76f1174&originHeight=180&originWidth=498&originalType=binary&ratio=1&rotation=0&showTitle=false&size=58001&status=done&style=none&taskId=u5e6d14b9-a930-4e23-a153-f684657ab2d&title=&width=486)
4. 自我引用
   1. 比如员工表里, 主管也是员工, 有员工的主管是此员工, 构成自我引用.
:::
<a name="FwKTe"></a>
# MYSQL 基础信息
<a name="oGcf0"></a>
## 初始表
![image.png](https://cdn.nlark.com/yuque/0/2023/png/29051556/1691041932683-d3986167-acc2-4a22-9572-3b14989319d7.png#averageHue=%232f2f2e&clientId=u4c2425b4-6e1b-4&from=paste&height=149&id=uabb61c47&originHeight=149&originWidth=216&originalType=binary&ratio=1&rotation=0&showTitle=false&size=21870&status=done&style=none&taskId=u266a648a-455e-4b90-bce6-676ab220d97&title=&width=216)

1. information_schema: 保存table名, 系统信息等等
2. mysql: 保存运行时的系统信息, 比如系统文件夹, 当前字符集
3. performance_schema: 存储监控各项性能指标
4. sys: system, 也是存储一些信息
<a name="U7mJO"></a>
## 设置可储存中文
`show create table employees;`<br />会看到charset = latin
<a name="bQQwm"></a>
# 基本SELECT 语句
<a name="zgj8G"></a>
## SQL 语言的分类

1. DDL : Data Definition Languages, 数据定义语言
   1. CREATE \ ALTER \ DROP \ RENAME \ TRUNCATE
2. DML : Data Manipulation Language, 数据操作语言		
   1. INSERT \ DELETE \ UPDATE \ SELECT  增删改查
3. DCL : Data Control Language, 数据定义语言
   1. COMMIT \ ROLLBACK \ SAVEPOINT \ GRANT \ REVOKE
<a name="t6LeY"></a>
# MySQL 规则与规范
<a name="oiJ8O"></a>
## 基本规则

- SQL 可以写在一行或者多行，必要时换行和缩紧
- 每条命令以 ;  \g \G 结尾
- 关键字不能缩写或分行
- 标点符号
   - () '' "" 成对出现
   - 英文标点
   - 字符串类型和日期类型可以用单引号表示
   - 列的别名尽量使用双引号, 而且建议写 AS
<a name="Wd7mY"></a>
## 大小写规范

1. windows下 大小写不敏感
2. linux下 大小写敏感
   1. 数据库名, 表名, 别名, 变量名严格区分大小写
   2. 关键字, 函数名, 列名(字段名), 列的别名 是不区分大小写的
3. 统一规范
   1. 库名, 表名, 表别名, 字段名, 字段别名都小写
   2. 关键字, 函数名, 绑定变量都大写
<a name="BOxBy"></a>
## 注释

- 单行注释: `#注释文字`
- 单行注释: `-- 注释文字` 必须要加一个空格, 在--后面
- 多行注释: `/* 注释 */`
<a name="GrqEG"></a>
# SELECT
<a name="wO0WU"></a>
## 最基本的SELECT

- `SELECT 1;` --> 1
- `SELECT 1+!, 3*2;` --> 2 和 6 的一个表
- `SELECT * FROM employees;` 所有employees的所有字段 以及 结果, 称为**结果集**
- `SELECT employee_id, last_name, salary FROM employees;`
<a name="jjpbY"></a>
## 列的别名
`SELECT employee_id AS id FROM employee;`<br />也可以不加AS, `SELECT employee emp_id ...`<br />AS: alias, 别名<br />列的别名可以用双引号 `SELECT employee "emp_id" ...`
<a name="gzyCx"></a>
## 去除重复行: DISTINCT
`SELECT DISTINCT department_id FROM employees;` 只显示有多少个部门, 不显示员工<br />会出错误:

- `SELECT salary, DISTINCT department_id`
- 这对不上, 因为有很多salary对应一个salary

不会出错:

- `SELECT DISTINCT salary, department_id`
- 这个distinct是指所有字段, 如果出现id=60, 工资=5000, 那么下一个id=60工资也是5000的就不会加入到结果集
- 如果下一个id=60的员工工资是6000, 那么会被算成一个新的row
- 但是没有实际意义
<a name="gBacc"></a>
## 空值参与运算: NULL
空值解释

1. 什么是空值: 某个字段是NULL, 比如有员工没有部门
2. _NULL是否等同于0?_   NO, 不等同于0

空值参与运算例子

- `SELECT employee_id, salary, salary * (1 + commision_pct) * 12 AS annualSalary`
- 因为有的人没有commision_pct, 会导致null * 数字, 结果为0
- 如果有NULL参与运算, 结果会为NULL
- 解决方案: 可以用 `IFNULL` 字段来判断, 如果是NULL, 则会使用下一个 `IFNULL(commission_pct, 0)`
<a name="QFeGf"></a>
## 着重号 ``
使用场景 : 出现字段名, 表名, 和关键字重名

- 比如我有一张表, 叫order, 但是order又是一个关键字, 用于排序, 会造成出错
- 我们可以使用着重号, `SELECT * FROM `order`;` 就不会报错
<a name="QPnZ8"></a>
## 查询常数
`SELECT '公司', employee_id, last_name FROM employees;`

| 公司 | empolyee_id | lastname |
| --- | --- | --- |
| 公司 | 1 | King |
| 公司 | 2 | Tom |

这就是查询常数, 会给每一行都复制该常数
<a name="kPel0"></a>
## 显示表结构
`DESCRIBE` : 显示表的结构关键字<br />`DESCRIBE employees;` :  显示创建表的相关信息, 字段相关, 比如null, key, default
<a name="vLEAE"></a>
# 过滤数据 WHERE

1. 查询数据, 但是限定条件
2. 例如我只想要某个部门的员工, department_id 为 90;
   1. `SELECT * FROM employees WHERE departement_id = 90;`
3. 查询last_name为'King'的员工信息
   1. `SELECT * FROM employees WHERE last_name = 'King';`
   2. 注意, 在Windows里面大小写不敏感, 所以搜索 `'king'` 也可以
   3. 但是, 建议必须使用正确的大小写, 否则在oracle里面就无法运行
   4. 包括也不要使用双引号来括字符串, 
   5. WHERE 必须要跟在 FROM 结构的后面
<a name="MQrfd"></a>
# 运算符
<a name="O3gJ2"></a>
## 算数运算符
![image.png](https://cdn.nlark.com/yuque/0/2023/png/29051556/1691213640976-98054354-7d84-4b32-a0ee-a582edb8cf15.png#averageHue=%23efefef&clientId=u7a80ad41-1f67-4&from=paste&height=236&id=u7153d885&originHeight=422&originWidth=1176&originalType=binary&ratio=1&rotation=0&showTitle=false&size=144141&status=done&style=none&taskId=u333a58af-601a-47fe-bc98-f0c744bc243&title=&width=658)<br />加减运算符

1. `SELECT 100 + '1'`  在MySQL 里面会有隐式转换, 将字符串转换为数值, 结果是101
2. `SELECT 100 + 'a'` 此时将'a'看做0处理, 因为a无法转换成数值
3. `SELECT 100 + NULL` 结果为NULL, 因为NULL 参与运算
4. int 和 int 运算 => int
5. int 和 float 运算 => float

乘除运算符

1. int * int => int
2. int * float => float
3. int / int => float , 除法无论怎么样都会返回浮点,
4. 0 不能为分母, 1/0 = NULL

取模运算 % 或 mod

1. `SELECT  12 % 3, 12 % 5. 12 % -5. -12 % 5, -12 % -5 FROM DUAL;`
2. resultSet: 0 | 2 | 2 | -2 | -2 
3. 结果正负之和第一个数有关, 如果第一个数是负数, 那么结果就是正数
<a name="ZZtEV"></a>
## 比较运算符
结果为真返回 1, 假返回 0, 其他返回NULL

1. `=`
   1. `SELECT 1 = 2, 1 != 2` => 0, 1
   2. `SELECT 1 = '1'` => 进行了隐式转换, 换成了数字1, 结果为True: 1
   3. `SELECT 0 = 'a'` => 进行了隐式转换, 如果字符串不能转换成数字, 那么'a'会变成 0, 所以结果为 1
   4. `SELECT 1 = 'a'` => 1 != 0, 所以结果为 0 False
   5. `'a' = 'b'` => 字符与字符比较不会转换, 结果为 0 False
   6. `NULL = NULL, 1 = NULL` 都为 NULL
2. `<=>` 安全等于
   1. 唯一与等号的区别是 可以比较NULL
   2. `SELECT 1 <=> NULL` 结果 0
   3. `SELECT NULL <=> NULL` 结果 1
   4. 在两个操作数均为NULL时, 返回 1, 当一个操作数为NULL时, 返回0 而不是NULL
3. `!=` 有NULL 返回NULL
<a name="Gr9A6"></a>
## 关键字运算符
![image.png](https://cdn.nlark.com/yuque/0/2023/png/29051556/1691383802950-2f5bb29a-32c3-4dea-932f-36b4c753a074.png#averageHue=%23ececec&clientId=uaed0c252-1a13-4&from=paste&height=472&id=uea8fd480&originHeight=878&originWidth=1200&originalType=binary&ratio=1&rotation=0&showTitle=false&size=420813&status=done&style=none&taskId=u488f7b74-d45d-47e5-9975-dab7531ace1&title=&width=645)

1. `IS NULL`
   1. 用法 `WHERE example IS NULL`
2. `IS NOT NULL`
   1. 用法 `WHERE example IS NOT NULL`
3. `ISNULL`
   1. 用法 `WHERE ISNULL(example)`
   2. 这个更像一个函数
4. `WHERE NOT example <=> NULL`
   1. 这样也可以，因为先example 和 NULL 安全等于，再取反
<a name="gTaap"></a>
### Least() \ Greatest()

1. `SELECT LEAST('g', 'b', 't', 'm')` => 'b'
2. `SELECT GREATEST('g', 'b', 't', 'm')` => 't'
3. `SELECT LEAST(first_name, last_name) FROM employees` => 返回两个字符串比较更小的, 字符串比较.
4. 可以使用LENGTH 获取长度然后用LEAST `LENGTH(first_name)`
<a name="ZeNHY"></a>
### Between ... and

1. `SELECT ... FROM ... WHERE salary BETWEEN 6000 AND 8000;` 包括边界值, 包括6000 和 8000 
2. `WHERE salary >= 6000 && salary <= 8000`
3. `WHERE salary BETWEEN 8000 AND 6000;` 这个是不会有返回值的, 因为前面的是左边界, 后面的是右边界
4. 选中不在 a 和 b 之间范围的: `WHERE salary < 6000 OR salary > 8000;`
5. 另一个种方法 `WHERE salary NOT BETWEEN 6000 AND 8000`
<a name="ub3D2"></a>
### IN

1. 查询部门为10, 20, 30部门的员工信息
2. `WHERE department = 10 OR department = 20 OR department = 30`
3. `WHERE department **IN (10,20,30);**`
4. 查询工资不是6000 和 7000 `WHERE salary NOT IN (6000, 7000)`
<a name="auY1o"></a>
### LIKE : 模糊查询

1. `**%**` : 代表不确定个数的字符; `**_**` 代表一个不确定的字符
2. 查询last_name 包含字符 'a' 的员工
3. `WHERE last_name LIKE '%a%'`
4. 查询以 'a' 开头的信息 `WHERE last_name LIKE 'a%';`
5. 查询last_name 包含 a 并且包含 e : `WHERE last_name LIKE '%a%' AND last_name LIKE '%e%';`
6. 使用`_`的: 查询第二个字符是'a'的员工 `WHERE last_name LIKE '_a%';`
7. 查询第二个字符是`_` 第三个字符是`a`的字符 
   1. 因为`_`代表着一个所有字符, 所以要使用转义字符 `\`
   2. `WHERE last_name LIKE '_\_a%'`
8. 如果想自定义转义符, 可以使用 `ESCAPE` 关键字
   1. `WHERE last_name LIEK '_$_a%' ESCAPE '$';`
<a name="FqwSi"></a>
### 正则表达式 REGEXP \ RLIKE

1. `SELELCT 'string' REGEXP '^s'...`

<a name="qreXD"></a>
## 逻辑运算符
![image.png](https://cdn.nlark.com/yuque/0/2023/png/29051556/1691466768382-45df2da9-6145-4e99-87c0-8d6de767163b.png#averageHue=%23f0f0f0&clientId=uf2b8df77-0448-4&from=paste&height=284&id=ufa38bd77&originHeight=284&originWidth=1173&originalType=binary&ratio=1&rotation=0&showTitle=false&size=80002&status=done&style=none&taskId=u02987260-f9e4-40ae-9c73-1685dea7052&title=&width=1173)
<a name="wilEG"></a>
## 位运算符
![image.png](https://cdn.nlark.com/yuque/0/2023/png/29051556/1691467377585-7962cdec-ff52-469c-bf3f-c9639b7ad92d.png#averageHue=%23ededed&clientId=u73f5775a-7d42-4&from=paste&height=175&id=u06ef4801&originHeight=175&originWidth=663&originalType=binary&ratio=1&rotation=0&showTitle=false&size=47819&status=done&style=none&taskId=u70f19831-61b9-4783-a1d9-f5c55d06584&title=&width=663)
