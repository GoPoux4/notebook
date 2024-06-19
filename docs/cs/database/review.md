# 复习

## 基本概念和关系代数

### 关系型数据库 Relational Database

一系列表的集合，一张表是一个基本单位，表中的每一行是一个关系。

### 基本概念

- relation 一条关系是一个元组 tuple
- attribute 属性指表中的列名
    - attribute type 指属性的数据类型，如 int, varchar
    - attribute value 指属性的值
        - domain 取值范围 \(D\)，一条关系 r 是 \(D_1 \times D_2 \times \cdots \times D_n\) 的子集
        - 属性值是 atomic 的，不可再分
- relation schema 关系模式
    - 指表的结构，\(R = (A_1, A_2, \cdots, A_n)\)，其中 \(A_i\) 是属性名
    - \(r(R)\) 表示关系 r 的 schema 是 R，r 是表中的一行。
    - 关系是无序的。
- keys 键
    - super key 超键，唯一标识一条记录的属性集合，可以冗余，如 \(A\) 是 super key，那么 \((A, B)\) 也是 super key
    - candidate key 候选键，最小的超键，没有冗余属性
    - primary key 主键，候选键中选择一个作为主键，在建表时指定
    - foreign key 外键，一个表中的属性，指向另一个表的主键

### 关系代数

- select 选择：\(\sigma_{p}(r)\)，选择满足条件 \(p\) 的元组
- project 投影：\(\Pi_{A_1, A_2, \cdots, A_n}(r)\)，选择属性 \(A_1, A_2, \cdots, A_n\) 的元组，结果去重
- union 并：\(r \cup s\)，两个关系的并集，两个关系的属性个数相同，且对应属性的类型相同。
- set difference 差：\(r - s\)，两个关系的差集，两个关系的属性个数相同，且对应属性的类型相同。
- Cartesian product 笛卡尔积：\(r \times s\)，两个关系的笛卡尔积，结果的属性个数是两个关系的属性个数之和，属性的类型是两个关系的属性类型的并集。
- rename 重命名：\(\rho_X(E)\) 引用
- intersection 交：\(r \cap s = r - (r - s)\)
- natural join 自然连接：\(r \bowtie s\)，同名属性相同的组合成一条记录。\(r \bowtie_\theta s = \sigma_\theta(r \bowtie s)\)
- outer join 外部连接
    - 左外连接 \(r \ltimes s\)，左边的表全部保留，右边的表中没有的用 null 填充
    - 右外连接 \(r \rtimes s\)，右边的表全部保留，左边的表中没有的用 null 填充
    - 全外连接 \(r \ltimes s \cup r \rtimes s\)，左右两边的表全部保留，没有的用 null 填充
- division 除法：\(r \div s\)，设 \(R = (A_1, A_2, \cdots, A_n, B_1, B_2, \cdots, B_m)\)，\(S = (B_1, B_2, \cdots, B_m)\)，\(R \div S = (A_1, A_2, \cdots, A_n)\)，表中保留的行 \(t\) 满足 \(\forall u \in s, t \times u \in r\)
- assignment 赋值：\(\leftarrow\)，将操作的结果进行命名。
- aggregation 聚合：\(_{G_1, G_2, \cdots, G_n}\gamma_{F_1(A_1), F_2(A_2), \cdots, F_m(A_m)}(r)\)，对关系 r 中 \(G_i\) 相同的元组进行聚合，然后在分别在属性 \(A_i\) 上进行聚合操作 \(F_i\)，如 count, sum, avg, max, min

## SQL

### 数据类型

char（定长字符串）、varchar（变长字符串）、int、float、date、time、timestamp、numeric(p,d)（p 有效数字位数，d 小数位数）

### 创建更新删除

- 创建表
    ```sql
    CREATE TABLE table_name (
        column_name1 data_type1,
        column_name2 data_type2,
        ...
        -- intergrity constraints
    );
    ```
    完整性约束指定 `primary key(x)`、 `foreign key(x) references r`
- 删除 `drop table table_name`
- 更新属性 `alter table table_name add column_name data_type`、`alter table table_name drop column_name`

### 查询

`select * from table_name`

- 去重 `select distinct * from table_name`，不去重 `select all * from table_name`
- from 字句中选择多个表，会先进行笛卡尔积
- 输出结果排序 `select * from table_name order by column_name asc/desc`
- 集合操作 `union`, `intersect`, `except` 连接两条查询结果，使用 `union all` 可以保留重复的行
- 聚合操作
    ```sql
    select count(*) from table_name group by column_name
    ```
    - having 对分组进行筛选
        ```sql
        select count(*) from table_name
        group by column_name having count(*) > 1
        ```
- 嵌套查询
    - 用 `in` 或者 `not in` 判断是否在子查询的结果中
        ```sql
        select * from table_name
        where column_name in (select column_name from table_name)
        ```
    - 用 `all` 或者 `some` 进行比较
        ```sql
        select * from table_name
        where column_name > all (select column_name from table_name)
        ```
    - 用 `exists` 或者 `not exists` 判断子查询是否为空
        ```sql
        select * from table_name as T1, table_name as T2
        where T1.column_name = T2.column_name and exists (
            select * from table_name as T3
            where T3.column_name = T1.column_name
        )
        ```
    - 用 `unique` 判断子查询是否为集合
- `with` 构建临时表
    ```sql
    with table_name(column_name) as (
        select column_name1 from table_name1
    )
    select * from table_name
    ```
- `join` 连接，使用 `join` 时默认是 `inner join`
    - `natural join` 自然连接，连接两个表中同名的属性
    - `inner join` 内连接，连接两个表中满足条件的属性
        ```sql
        select * from table_name1 inner join table_name2
        on table_name1.column_name = table_name2.column_name
        ```
    - `left join` 左连接，保留左表中的所有行
    - `right join` 右连接，保留右表中的所有行
    - `full join` 全连接，保留两个表中的所有行

### 插入删除更新

- 插入 `insert into table_name values (value1, value2, ...)`
- 删除 `delete from table_name where column_name = value`
- 更新 `update table_name set column_name = value where column_name = value`
    `case` 子句用于分类讨论
    ```sql
    update table_name
    set column_name = case
        when condition1 then value1
        when condition2 then value2
        else value3
    end
    where column_name = value
    ```

### 视图、索引

- 视图 `create view view_name as (select * from table_name)`
    - 不会创建新的表，只是隐藏了查询的细节
    - 可以用 `insert` 进行更新，有限制：
        - 创建视图时只用了一个表
        - 没有 `distinct` 和聚合操作
        - 没有空值和 default 值
- 索引 `create index index_name on table_name(column_name)`

### type 和 domain

- 用户定义 type
    ```sql
    create type type_name as numeric(10, 2) final
    ```
- 用户定义 domain
    ```sql
    create domain domain_name as numeric(10, 2)
    ```

不同 type 的变量不能相互运算，基础类型相同的 domain 可以相互运算。

### 完整性约束

- not null 值不能为空
- unique 值唯一
- primary key 主键
- check 检查约束
    ```sql
    create table table_name (
        column_name1 data_type1 check (column_name1 > 0),
        column_name2 data_type2,
        check (column_name2 in ('value1', 'value2'))
    )
    ```
- 引用完整性约束
    ```sql
    create table table1 (
        primarykey1 data_type1 primary key
    )
    create table table2 (
        primarykey2 data_type2 primary key,
        foreignkey data_type2 references table1(primarykey1)
    )
    ```
- 级联操作
    - `on delete cascade` 删除主表中的记录时，从表中的记录也会被删除
        比如删除 `table1` 中的记录 \(r\)，`table2` 中 `foreignkey` 指向 \(r\) 的记录也会被删除
    - `on update cascade` 更新主表中的记录时，从表中的记录也会被更新

### 事务

事务是一系列操作的集合，要么全部执行，要么全部回滚，由 `begin`, `commit`, `rollback` 控制。

ACID 特性：

- atomicity 原子性：事务是一个不可分割的工作单位，要么全部执行，要么全部回滚
- consistency 一致性：事务执行前后数据库的完整性约束没有被破坏
- isolation 隔离性：事务之间是相互隔离的，一个事务的执行不会影响其他事务
- durability 持久性：事务执行成功后，对数据库的修改是永久的

### 权限 authoization

分类：

- 数据层面：`select`, `insert`, `update`, `delete`
- 数据库层面：`create`, `drop`, `alter`

基本操作：

- `grant` 授权
    ```text
    grant <privilege_list> on <relation_name or view_name> to <user_list>
    ```
    user_list 可以是 user 或者 role 或者 public 代表所有用户
    ```sql
    grant select on table_name to user1
    grant update(column_name) on table_name to user1
    grant all privileges on table_name to user1
    ```
- `revoke` 撤销权限，用法和 `grant` 类似

#### role

一组权限的集合，可以赋予用户或者其他角色。

- 创建 `create role role_name`
- 赋予权限 `grant select on table_name to role_name`
- 赋予用户 `grant role_name to user_name`
- 赋予其他角色 `grant role_name1 to role_name2`

#### 其他

- 引用权限，决定是否能够通过外键约束删除或者更新其他表中的记录
    ```sql
    grant references(column_name) on table_name to user1
    ```
- 级联权限
    - `grant select on table_name to user1 with grant option`，user1 可以将 select 权限授予其他用户
    - `revoke select on table_name from user1 cascade`，级联撤销权限
    - `revoke select on table_name from user1 restrict`，只撤销 user1 的权限
    - `revoke grant option for select on table_name from user1`，撤销 user1 的授权权限

### 函数

```sql
create function dep_count(department_name varchar)
returns integer
begin
    declare count integer;
    select count(*) into count
    from employee
    where department = department_name;
    return count;
end
```

返回值可以是 table：

```sql
create function get_employee()
returns table(name varchar, department varchar)
begin
    return table(
        select name, department
        from employee
    );
end
```

### Procedural Constructs 过程化结构

- while 循环
    ```sql
    begin
        declare count integer;
        set count = 0;
        while count < 10 do
            set count = count + 1;
        end while;
    end
    ```
- repeat 循环，相当于 do while
    ```sql
    begin
        declare count integer;
        set count = 0;
        repeat
            set count = count + 1;
        until count = 10
        end repeat;
    end
    ```
- for 循环，枚举查询结果的每一行
    ```sql
    begin
        declare sum integer;
        for r as select * from table_name do
            set sum = sum + r.column_name;
        end for;
    end
    ```

### 触发器

ECA rule:

- event 事件，触发器的触发事件，比如 insert, update, delete
- condition 条件，触发器的执行条件
- action 动作，触发器触发后执行的动作

```sql
create trigger trigger_name
after update of column_name on table_name -- event
referencing new row as nrow
referencing old row as orow
for each row
when (nrow.column_name <> orow.column_name) -- condition
begin -- action
    insert into log_table values (nrow.column_name);
end
```

## Entity-Relationship Model

### ER 模型

由关系和实体组成。

- entity 实体：由长方形表示，primary key 用下划线表示
- relationship 关系：由菱形表示，连接两个及以上的实体
- constraint 约束
    - 映射基数：一对一、一对多、多对一、多对多
        - 箭头表示一，直线表示多
        - 三元关系中箭头只能出现一次
        - 直线上标注 `x...y` 表示最少 x 个，最多 y 个元素参与关系，`*` 表示任意个
    - 参与度约束：如果全部参与到关系中，要用两条线
- 弱实体集 weak entity set
    - 没有 primary key，依赖于其他实体存在，实体集内属性用虚线表示
    - 与其依赖的实体之间的关系用双菱形表示，称为 identifying relationship 标识型关系
    - 比如：一个 course 下会有多个 section，section 依赖于 course 的存在，所以 section 是 weak entity

### ER 模型转换为关系模型

- 强实体集：每个实体集转换为一个表，每个属性转换为一个列
    `course(course_id, title, dept_name)`
- 弱实体集：每个实体集转换为一个表，每个属性转换为一个列，加上一个指向标识型关系的外键
    `section(course_id, sec_id, semester, year, building, room_number)`
- 多对多关系：转换为一个新的表，包含两个实体集的 primary key 和其他属性
    `takes(student_id, course_id, sec_id)`
- 多对一关系：可以不转换，直接在多的一方加上一个外键指向一的一方

### 泛化关系

- 概化 generalization：将多个实体集合并为一个更高层次的实体集，从下往上
    相当于从子类合并到父类
- 特化 specialization：将一个实体集分为多个更低层次的实体集，从上往下
    相当于从父类继承到子类
    - partial specialization 部分特化：子类的实体集不包含父类的所有属性，将子类和父类进行连接才是所有信息
    - total specialization 全部特化：子类的实体集包含父类的所有属性

约束：

- overlap 约束：子类的实体集之间可以有重叠
- disjoint 约束：子类之间不重叠

## 关系数据库设计

### Lossless-join Decomposition 无损分解

一个关系 R 被分解为 R1 和 R2，如果 R1 和 R2 的 natural join 等于 R，那么称为无损分解。

如果公共属性是其中一个关系的 primary key，那么一定是无损分解。\(R_1 \cap R_2 \rightarrow R_1\) 或者 \(R_1 \cap R_2 \rightarrow R_2\)。

### Functional Dependency 函数依赖

\(A \rightarrow B\) 表示 A 唯一确定 B

- trival: 全集决定子集，对于所有关系模式都成立
- 闭包：属性集合能推导出来的所有属性构成的集合。\(A\) 的属性闭包表示为 \(A^+\)

### Normal Form 范式

- 1NF：属性是原子的，不可再分
- BCNF：对于任意一个函数依赖 \(X \rightarrow Y\)，要么是平凡的，要么 X 是 super key
    计算 X 的属性闭包，如果闭包包含了所有属性，那么满足 BCNF
- 3NF：对于任意一个函数依赖 \(X \rightarrow Y\)，满足以下条件之一
    - 依赖是平凡的
    - X 是 super key
    - 对于每个 \(Y - X\) 中的属性，都包含在一个 candidate key 中

### Canonical Cover 正则覆盖

- 无关属性：对于 F 中的函数依赖 \(\alpha \rightarrow \beta\)
    - \(\alpha\) 中的属性 A 是多余的，如果 \(\alpha - A\) 也能推导出 \(\beta\)
        如果 \((\alpha - A)^+\) 包含 \(\beta\)，那么 A 是多余的
    - \(\beta\) 中的属性 B 是多余的，如果 \(\alpha \rightarrow \beta - B\) 也成立
        将 \(\alpha \rightarrow \beta\) 替换为 \(\alpha \rightarrow \beta - B\)，如果 \(\alpha^+\) 包含 \(B\)，那么 B 是多余的

- 计算最小覆盖
    1. 用 union rule 将满足 \(\alpha \rightarrow \beta_1 \land \alpha \rightarrow \beta_2\) 的函数依赖合并为 \(\alpha \rightarrow \beta_1 \beta_2\)
    2. 去除重复属性
    3. 重复 a,b 两步，直到不能合并为止

## 数据库设计理论

### Storage Hierarchy 存储层次

- 易失存储 volatile storage
- 非易失存储 nonvolatile storage

#### 磁盘存储 Magnetic Disk

- 读写头 read/write head
- 磁道 tracks，每个硬盘有多个磁道
- 扇区 sector，由磁道划分，每个扇区 512 字节

性能评估：

- access time = seek time + rotational delay
    - seek time 寻道时间，读写头移动到正确的磁道上的时间
    - rotational delay 旋转延迟，等待扇区旋转到读写头下的时间
- data transfer rate 数据传输速率，读写数据的速率
- MTTF：出现故障的平均时间

### Buffer Management 缓冲管理

见 minisql

### Index 索引

衡量指标：

- point query 单点查询
- range query 范围查询

!!! example "B+ 树节点数估计"
    对于 schema：
    ```sql
    person(pid char(18) primary key,
           name char(8),
           age smallint,
           address char(40));
    ```
    block size 为 4KB，共有 \(10^6\) 条记录
    
    - person record size: \(18 + 8 + 2 + 40 = 68 bytes\)
    - 每个 block 可以存储 \(\lfloor 4096 / 68 \rfloor = 60\) 条记录
    - 存储所有记录需要 \(\lceil 10^6 / 60 \rceil = 16667\) 个 block
    - B+ 树 fan-out：索引项为 18 bytes，指针为 4 bytes，每个节点中指针比索引项多 1，所以 fan-out 为 \(\lfloor (4096 - 4) / (18 + 4) \rfloor + 1 = 187\)。
        - 对于内部节点，最多有 187 个子节点，最少有 94 个子节点
        - 对于叶子节点，最多有 186 个索引项，最少有 93 个索引项
    - 不同层数 b+ 树的节点数：

        - 2-level: min=\(2 \times 93 = 186\), max=\(187 \times 186 = 34782\)
        - 3-level: min=\(2 \times 94 \times 93 = 17484\), max=\(187 \times 187 \times 186 = 6504234\)
        - 4-level: min=\(2 \times 94 \times 94 \times 93 = 1643496\), max=\(187 \times 187 \times 187 \times 186\)

        得到 \(10^6\) 条记录的索引树有 3 层。

LSM 树：将索引结构分为若干层，越底层的索引越大。

- 插入：当上层的索引满了之后，将数据合并到下层。
- 查找：从上往下查找，如果上层没有找到，就去下层查找。

### Query Processing 查询处理

#### 基础步骤

- parsing and translation 将查询语句转换为关系代数表达式
- optimization 优化查询计划，转换为执行计划
- evaluation 执行计划，获取结果

#### 代价估算

估算指标：

- number of seeks：磁盘寻找次数
- number of block transfers：磁盘读写次数

**Select Operation**

- A1（linear search）：扫描所有记录，找到满足条件的记录
    - worst cost: \(t_{seek} + b_r \times t_{trans}\)，\(t_s\) 为 seek time，\(b_r\) 为要传输的块数，\(t_t\) 为传输一块的时间
    - average cost: \(t_s + (b_r / 2) \times t_t\)
- A2（B+ index search, on key）：通过 B+ 树索引找到记录，B+ 树中的 key 唯一。
    - cost: \((h_i + 1) \times (t_s + t_t)\)，\(h_i\) 为 B+ 树的高度，对于树上查找路径的每一块和最后实际存放记录的块都要进行 seek 和 transfer
- A3（B+ index search, on non-key）：通过 B+ 树索引找到记录，B+ 树中的 key 不唯一。
    - cost: \(h_i \times (t_s + t_t) + t_s + b \times t_t\)，B+ 树中存放的是每个 key 首次出现的位置，通过叶节点的指针找到实际存放记录的块，然后扫描块找到满足条件的记录