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
    - 全外连接 \(r \Join s\)，两个表中的所有记录都保留，没有的用 null 填充
- division 除法：\(r \div s\)，设 \(R = (A_1, A_2, \cdots, A_n, B_1, B_2, \cdots, B_m)\)，\(S = (B_1, B_2, \cdots, B_m)\)，\(R \div S = (A_1, A_2, \cdots, A_n)\)，表中保留的行 \(t\) 满足 \(\forall u \in s, t \times u \in r\)
- assignment 赋值：\(\leftarrow\)，将操作的结果进行命名。
- aggregation 聚合：\(_{G_1, G_2, \cdots, G_n}\mathcal{G}_{F_1(A_1), F_2(A_2), \cdots, F_m(A_m)}(r)\)，对关系 r 中 \(G_i\) 相同的元组进行聚合，然后在分别在属性 \(A_i\) 上进行聚合操作 \(F_i\)，如 count, sum, avg, max, min

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

\(A \rightarrow B\) 表示 \(A\)唯一确定 \(B\)

- trival: 全集决定子集，对于所有关系模式都成立
- 闭包：属性集合能推导出来的所有属性构成的集合。\(A\) 的属性闭包表示为 \(A^+\)

#### Dependency Preservation 依赖保持

一个关系 R 被分解为 R1 和 R2，如果 \(F_1\) 和 \(F_2\) 的闭包能推导出 \(F\) 的闭包，那么称为依赖保持。

### Normal Form 范式

- 1NF：属性是原子的，不可再分
- BCNF：对于任意一个函数依赖 \(X \rightarrow Y\)，要么是平凡的，要么 \(X\)是 super key
    计算 \(X\)的属性闭包，如果闭包包含了所有属性，那么满足 BCNF
    - Decomposing a Schema into BCNF
        1. 选择一个不满足 BCNF 的函数依赖，将其单独作为一个关系
        2. 重复这个过程，直到所有关系都满足 BCNF
        3. 将可合并的关系进行合并
- 3NF：对于任意一个函数依赖 \(X \rightarrow Y\)，满足以下条件之一
    - 依赖是平凡的
    - \(X\)是 super key
    - 对于每个 \(Y - X\) 中的属性，都包含在一个 candidate key 中

### Canonical Cover 正则覆盖

- 无关属性：对于 \(F\) 中的函数依赖 \(\alpha \rightarrow \beta\)
    - \(\alpha\) 中的属性 \(A\) 是多余的，如果 \(\alpha - A\) 也能推导出 \(\beta\)

        如果 \((\alpha - A)^+\) 包含 \(\beta\)，那么 \(A\) 是多余的

    - \(\beta\) 中的属性 \(B\) 是多余的，如果将 \(\alpha \rightarrow \beta\) 替换为 \(\alpha \rightarrow \beta - B\)，\(\alpha^+\) 包含 \(B\)

- 计算最小覆盖
    1. 用 union rule 将满足 \(\alpha \rightarrow \beta_1 \land \alpha \rightarrow \beta_2\) 的函数依赖合并为 \(\alpha \rightarrow \beta_1 \beta_2\)
    2. 去除重复属性
    3. 重复 a, b 两步，直到不能合并为止

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

- primary index 主索引，索引的 key 是 primary key，在文件中按照 key 的顺序存储
- secondary index 辅助索引，索引顺序和文件中的顺序不一定相同，不一定是唯一的

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

## Query Processing 查询处理

### 基础步骤

- parsing and translation 将查询语句转换为关系代数表达式
- optimization 优化查询计划，转换为执行计划
- evaluation 执行计划，获取结果

### 代价估算

估算指标：

- number of seeks：磁盘寻找次数
- number of block transfers：磁盘读写次数

#### Select Operation

1. A1（linear search）：扫描所有记录，找到满足条件的记录
    - worst cost: \(t_{seek} + b_r \times t_{trans}\)，\(t_s\) 为 seek time，\(b_r\) 为要传输的块数，\(t_t\) 为传输一块的时间
    - average cost: \(t_s + (b_r / 2) \times t_t\)
2. A2（B+ index search, on key）：通过 B+ 树索引找到记录，B+ 树中的 key 唯一。
    - cost: \((h_i + 1) \times (t_s + t_t)\)，\(h_i\) 为 B+ 树的高度，对于树上查找路径的每一块和最后实际存放记录的块都要进行 seek 和 transfer
3. A3（B+ index search, on non-key）：通过 B+ 树索引找到记录，B+ 树中的 key 不唯一。
    - cost: \(h_i \times (t_s + t_t) + t_s + b \times t_t\)，B+ 树中存放的是每个 key 首次出现的位置，通过叶节点的指针找到实际存放记录的块，然后扫描块找到满足条件的记录
4. A4（secondary index search, equality on key）：建立在 key 上的辅助索引，和主索引类似
    - cost: \((h_i + 1) \times (t_s + t_t)\)
5. A4'（secondary index search, equality on non-key）：建立在非 key 上的辅助索引
    - 叶节点存放的是指向存放指向实际记录的指针块的指针，找到叶节点后，要在指针块中找到指针，再找到实际记录
    - cost: \((h_i + m + n) \times (t_s + t_t)\)，m 为指针块的数量，n 为实际记录的数量，最坏情况下每个记录都在不同的块中
6. A5（primary B+ index, comparison）：主索引上的比较查找，需要找到第一个满足条件的记录，往后扫描
    - cost: \(h_i \times (t_s + t_t) + t_s + b \times t_t\)
7. A6（secondary index, comparison）：辅助索引上的比较查找
    - 一般用线性扫描

#### Complex Selection

1. Conjunctive Selection：多个条件的 and \(\sigma_{\theta_1 \land \theta_2 \land \cdots \land \theta_n}(r)\)
    - 如果其中一个条件有索引，就用索引找到记录，然后再进行其他条件的判断
    - 有联合索引的情况下，可以直接用联合索引找到记录
    - 分别用索引找到记录，然后取交集
2. Disjunctive Selection：多个条件的 or \(\sigma_{\theta_1 \lor \theta_2 \lor \cdots \lor \theta_n}(r)\)
    - 用索引找到记录，然后取并集
    - 线性扫描
3. Not Selection：\(\sigma_{\lnot \theta}(r)\)
    - 直接使用索引
    - 线性扫描

#### External Sorting

假设有 M 个内存块

1. 创建初始归并段 run
    - 读取 M 个块到内存中
    - 对这些块进行排序
    - 写回到磁盘中，得到一个 run
    - 重复这个过程，直到所有记录都被读取，总共的 run 数为 \(N = \lceil b_r / M \rceil\)
2. 归并
    - 如果 \(N < M\)，那么只需要一次 N 路归并，将内存中 N 块作为输入缓冲，1 块作为输出缓冲
        - 每次将输入缓冲中最小的记录写入输出缓冲
        - 若输出缓冲满，写入磁盘，清空输出缓冲
        - 若输入缓冲中有块读完，读取对应 run 的下一块
    - 如果 \(N \geq M\)，则需要多次 \(M-1\) 路归并，每次将 \(M-1\) 个 run 归并为一个 run，直到所有 run 归并为一个 run
        - 每次 run 的个数会减少到原来的 \(\frac{1}{M-1}\)，总共需要 \(\lceil \log_{M-1} N \rceil\) 次归并

**Cost Analysis**

- runs 个数：\(N = \lceil b_r / M \rceil\)
- 归并轮数：\(\lceil \log_{M-1} \lceil b_r / M \rceil \rceil\)
- transfer 次数
    - 创建初始归并段和每次归并需要 \(2 \times b_r\) 次 transfer，读和写各一次
    - 最后一次归并的结果不一定需要写入磁盘，这种情况总共需要 \(2 b_r (\lceil \log_{M-1} \lceil b_r / M \rceil \rceil + 1) - b_r = b_r \times (2 \times \lceil \log_{M-1} \lceil b_r / M \rceil \rceil + 1)\) 次 transfer
- seek 次数
    - 创建初始归并段需要 \(2 \times N\) 次 seek，每个 run 读和写各一次
    - 每次归并需要 \(2 \times b_r\) 次 seek，因为数据是按照块读取写回的
    - 最后一次归并的结果不一定需要写入磁盘，这种情况总共需要 \(b_r (2 \lceil \log_{M-1} \lceil b_r / M \rceil \rceil - 1) + 2 \lceil b_r / M \rceil\) 次 seek

假如内存中给每一路分配了 \(b_b\) 个输入缓冲块，同时输出缓冲块也是 \(b_b\) 个，那么

- 归并轮数：\(\lceil \log_{M/b_b - 1} \lceil b_r / M \rceil \rceil\)
- transfer 次数：
    - 创建初始归并段和每次归并需要的 transfer 次数不变，为 \(2 \times b_r\) 次
    - 最后一次归并的结果不一定需要写入磁盘，这种情况总共需要 \(b_r (2 \lceil \log_{M/b_b - 1} \lceil b_r / M \rceil \rceil + 1)\) 次 transfer
- seek 次数：
    - 创建初始归并段需要 \(2 \times N\) 次 seek
    - 每次归并需要 \(2 \times \lceil b_r / b_b \rceil\) 次 seek
    - 最后一次归并的结果不一定需要写入磁盘，这种情况总共需要 \(\lceil b_r / b_b \rceil (2 \lceil \log_{M/b_b - 1} \lceil b_r / M \rceil \rceil - 1) + 2 \lceil b_r / M \rceil\) 次 seek

#### Join Operation

1. Nested-loop join：两重循环，外循环 r，内循环 s
    - transfer 次数：\(b_r + n_r \times b_s\)
    - seek 次数：\(n_r + b_r\)
2. Block nested-loop join：外循环 r 的块，内循环 s 的块，每次对两个块做 nested-loop join
    - transfer 次数：\(b_r + b_r \times b_s\)
    - seek 次数：\(2 \times b_r\)
    - 将较小的关系作为外关系能减少代价。
    - 如果分配 \(M - 2\) 块给 r，\(1\) 块给 s，那么
        - transfer 次数：\(b_r + \lceil b_r / (M - 2) \rceil \times b_s\)
        - seek 次数：\(2\lceil b_r / (M - 2) \rceil\)
3. Index nested-loop join：如果 s 上有索引，可以用索引找到满足条件的记录
    - 时间花费：\(b_r(t_s + t_t) + n_r \times c\)，\(c\) 为每次索引查找花费的时间
4. Merge join：两个关系都有序，可以直接进行归并
    - transfer 次数：\(b_r + b_s\)
    - seek 次数：\(b_r + b_s\) 最坏情况是在两个关系中切换
    - 将 M 块内存分为 \(x_r + x_s\) 块，\(x_r\) 块用来存储 r，\(x_s\) 块用来存储 s，那么
        - transfer 次数：\(b_r + b_s\)
        - seek 次数：\(\lceil b_r / x_r \rceil + \lceil b_s / x_s \rceil\)
        - 选择 \(x_r = \frac{\sqrt{b_r}}{\sqrt{b_r} + \sqrt{b_s}} \times M\) 和 \(x_s = \frac{\sqrt{b_s}}{\sqrt{b_r} + \sqrt{b_s}} \times M\) 可以最小化 seek 次数
5. Hash join: 将两个关系用相同的 hash 函数进行划分，hash 后将原数据分为 \(n\) 个域，然后对每个域进行 join
    - 需要满足 \(n \geq \lceil b_s / M \rceil\)，即每个域能一次性放入内存中。如果放不下，可以进行二次划分
    - s 称为 build relation，r 称为 probe relation
    - 过程：
        1. 对 s 进行 hash 划分，对于每个划分，为其在内存中分配一个块用于写回
        2. 对 r 进行 hash 划分
        3. 对于每个划分：
            1. 读取 s 的一个划分到内存中，在内存中建立 hash 索引
            2. 将 r 的划分一块一块读取到内存中，对于每个块中每条记录，查找 hash 索引，找到满足条件的记录进行输出
    - recursive partitioning：如果一个域不能放入内存中，可以进行二次划分，直到可以放入内存中，此时 \(M > \lceil b_s / n \rceil + 1\)，近似为 \(M > \sqrt{b_s}\)
    - 代价估算：
        - 如果不需要 recursive partitioning
            - transfer 次数：\(2(b_r + b_s) + b_r + b_s = 3(b_r + b_s)\)，划分时读取写入各一次，join 时读取一次（实际可能比这个值大，因为可能有些块不满）
            - seek 次数：\(2 (\lceil b_r / b_b \rceil + \lceil b_s / b_b \rceil)\)，划分时 seek 一次，join 时 seek 一次
        - 如果需要 recursive partitioning，会进行 \(\lceil \log_{M/b_b - 1}\lceil b_s / M \rceil \rceil\) 次划分
            - transfer 次数：\(2(b_r+b_s)\times \lceil \log_{M/b_b - 1}\lceil b_s / M \rceil \rceil + b_r + b_s\)
            - seek 次数 \(2 (\lceil b_r / b_b \rceil + \lceil b_s / b_b \rceil) \times \lceil \log_{M/b_b - 1}\lceil b_s / M \rceil \rceil\)

### Query Optimization 查询优化

#### Equivalence Rules

- Conjunctive Selection deconstruct：\(\sigma_{\theta_1 \land \theta_2}(r) \equiv \sigma_{\theta_1}(\sigma_{\theta_2}(r))\)
- Selection commutativity：\(\sigma_{\theta_1}(\sigma_{\theta_2}(r)) \equiv \sigma_{\theta_2}(\sigma_{\theta_1}(r))\)
- Projection omission：\(\Pi_{L_1}(\Pi_{L_2}(...(\Pi_{L_n}(r))...)) \equiv \Pi_{L_1}(r)\)，只保留最后一个 projection
- 选择操作和笛卡尔积嵌套：
    - \(\sigma_{\theta}(r \times s) \equiv r \bowtie_{\theta} s\)
    - \(\sigma_{\theta_1}(r \bowtie_{\theta_2} s) \equiv r \bowtie_{\theta_2 \land \theta_1} s\)
- Theta join commutativity：\(r \bowtie_{\theta} s \equiv s \bowtie_{\theta} r\)
- associativity：
    - Natural join associativity：\(r \bowtie s \bowtie t \equiv r \bowtie (s \bowtie t) \equiv (r \bowtie s) \bowtie t\)
    - Theata join 在特定情况下可结合：\((r \bowtie_{\theta_1} s) \bowtie_{\theta_2 \land \theta_3} t \equiv r \bowtie_{\theta_1 \land \theta_3} (s \bowtie_{\theta_2} t)\)，其中 \(\theta_2\) 只和 s 和 t 有关
- 投影在 theta join 中的分配
    - 如果 \(\theta\) 只包含了 \(L_1 \cup L_2\) 中的属性
        \(\Pi_{L_1 \cup L_2}(r \bowtie_{\theta} s) \equiv \Pi_{L_1}(r) \bowtie_{\theta} \Pi_{L_2}(s)\)
    - 否则，设 \(\theta\) 包含了 \(r\) 中的属性 \(L_1 \cup L_3\)，\(s\) 中的属性 \(L_2 \cup L_4\)，那么
        \(\Pi_{L_1 \cup L_2}(r \bowtie_{\theta} s) \equiv \Pi_{L_1 \cup L_2}(\Pi_{L_1 \cup L_3}(r) \bowtie_{\theta} \Pi_{L_2 \cup L_4}(s))\)

#### Cost Estimation

Statics:

- \(n_r\)：r 中的记录数
- \(b_r\)：存储 r 的块数
- \(l_r\)：r 每条记录的大小
- \(f_r\)：blocking factor of r，每个块中的记录数
- \(V(A, r)\)：r 中属性 A 的不同值的数量

1. \(\sigma_{A = V}(r)\)
    - 估计选中的记录数：\(n_r / V(A, r)\)
2. \(\sigma_{A \leq V}(r)\)
    - 如果可以获得 \(\min(A, r)\) 和 \(\max(A, r)\)，那么可以估计选中的记录数
        - 如果 \(V < \min(A, r)\)，那么选中的记录数为 \(0\)
        - 如果 \(V > \max(A, r)\)，那么选中的记录数为 \(n_r\)
        - 否则，选中的记录数为 \(\frac{V - \min(A, r)}{\max(A, r) - \min(A, r)} \times n_r\)
    - 如果无法获得，那么估计选中的记录数为 \(n_r / 2\)
3. 设条件 \(\theta_i\) 的选率为 \(s_i/n_r\)，假设条件直接相互独立
    - \(\sigma_{\theta_1 \land \cdots \land \theta_n}(r)\)，选中的记录数为 \(n_r \times \prod_{i=1}^n s_i/n_r\)
    - \(\sigma_{\theta_1 \lor \cdots \lor \theta_n}(r)\)，选中的记录数为 \(n_r \times (1 - \prod_{i=1}^n (1 - s_i/n_r))\)
4. \(r \bowtie_\theta s\)
    - 如果 \(r \cap s = \emptyset\)，那么选中的记录数为 \(n_r \times n_s\)
    - 如果连接属性是 r 的 key，那么选中的记录数 \(n \leq n_s\)
    - 如果连接属性是 s 指向 r 的外键，那么选中的记录数 \(n = n_s\)
    - 否则估计为 \(\min(\frac{n_r}{V(A, r)}\times n_s, \frac{n_s}{V(A, s)}\times n_r)\)
5. \(\Pi_{A}(r)\)
    - 选中的记录数为 \(V(A, r)\)
6. 聚合 \(_G \mathcal{G}_{f(A)}(r)\)
    - 选中的记录数为 \(V(A, r)\)

## 事务

简化为只有 read 和 write 操作的事务。

### Transaction State

- active：事务正在执行
- partially committed：事务执行完成，但是还没有提交
- committed：事务执行完成并且提交
- failed：事务执行失败
- aborted：事务执行失败并且回滚

### Concurrency Execution

#### 典型错误

1. Lost update: 两个事务同时读取一个值，然后修改，然后写回，会导致一个事务的修改被另一个事务覆盖。
2. Dirty read: 一个事务读取了另一个事务未提交的数据，然后另一个事务回滚，导致读取的数据是脏数据。
3. Unrepeatable read: 一个事务读取了一个值，然后另一个事务修改了这个值，导致第一个事务再次读取时值不同。
4. Phantom read: 一个事务读取了一个范围内的值，然后另一个事务插入了一个新的值，导致第一个事务再次读取时范围内的值不同。

#### Schedule

调度，事务执行顺序。

- 串行调度：事务按照顺序执行，一个事务执行完后另一个事务再执行，一定是隔离的
- 非串行调度：事务并发执行，交叉调度。如果能够交换单个不冲突的操作的执行顺序使得结果和串行调度一致，那么执行结果和串行调度一致。

#### Serializability

如果一个交叉调度如果等价于一个串行调度，那么这个交叉调度是可串行化的。

一对冲突的操作的顺序决定可串行化的交叉调度等价的串行调度的顺序：如果 \(T_1\) 先执行 \(\mathrm{read}(Q)\)，然后 \(T_2\) 执行 \(\mathrm{write}(Q)\)，那么等价的串行调度中，\(T_1\) 在 \(T_2\) 之前执行。

#### Test for Serializability

precedence graph 前驱图：如果 \(T_i\) 中能够找到一个操作需要先于 \(T_j\) 中的操作执行，那么在图中添加一条边 \(T_i \rightarrow T_j\)。

如果找到一个环，那么这个调度是不可串行化的。

#### Different Forms

- conflict serializability：如上
- view serializability：对于非串行化调度和串行化调度，如果初始值由同一个事务读取，中间值由同一个事务写入，再由同一个事务读取，最终值由同一个事务写入，那么这个调度是 view serializable 的。
    冲突可串行化都是视图可串行化的，反之不一定。

### Recoverable Schedule

如果事务 \(T_i\) 读取了事务 \(T_j\) 的脏数据，那么 \(T_j\) 必须在 \(T_i\) 之前提交。

#### Cascading Rollback

如果事务 \(T_i\) 读取了事务 \(T_j\) 的脏数据，然后 \(T_k\) 读取了 \(T_i\) 的脏数据，那么 \(T_j\) 回滚，\(T_i, T_k\) 需要级联回滚。

要避免发生级联回滚，否则开销会很大。

### Transaction Isolation Level

1. Serializable：默认级别，最高级别，保证事务串行执行
2. Repeatable read：保证没有重复读
3. Read committed：保证不读取脏数据
4. Read uncommitted：允许读取脏数据

## Concurrent Control

并发控制协议

- Lock-based protocol：加锁
- Timestamp-based protocol：时间戳，事务按照时间戳顺序执行
- Validation-based protocol：事务执行完后验证是否可串行化

### Lock-based Protocol

#### Lock

- exclusive lock (X)：排他锁，可以读写
- shared lock (S)：共享锁，用于读操作

S 与 S 兼容，S 与 X 不兼容，X 与 X 不兼容。

#### Two-phase Locking Protocol

一个事务分为两个阶段：

1. growing phase：事务可以获取锁，但是不能释放锁
2. shrinking phase：事务可以释放锁，但是不能获取锁

获得最后一个锁的时间点称为事务的 lock point。按照 lock point 的顺序执行事务，可以保证可串行化。

基本两阶段封锁不能保证可恢复性和避免级联回滚。

- Strict two-phase locking protocol 严格两阶段封锁协议：在事务提交之前不能释放 X 锁，保证可串行化和可恢复性
- Rigorous two-phase locking protocol 强两阶段封锁协议：在事务提交之前不能释放任何锁。

#### Lock Conversion

- First phase: lock upgrade，第一阶段可以申请锁或者升级锁
- Second phase: lock downgrade，第二阶段可以降级锁或者释放锁

### Implementation of Lock-based Protocol

lock table：用 hash 表的形式，对记录 id 建立 hash 表，每个记录下有一个锁队列。

### Deadlock

解决死锁的方法：

- 一次性分配所有锁
- Impose partial order：对数据访问顺序进行限制
- Timeout-based scheme：超时回滚

#### Deadlock Detection

定期检测死锁，然后回滚一个事务。

建立等待图，如果事务 \(T_i\) 等待事务 \(T_j\) 的锁，那么在图中添加一条边 \(T_i \rightarrow T_j\)。

### Graph-based Protocol

假如知道数据按照偏序访问，那么可以通过图来执行事务。

#### Tree Protocol

- 只有 X 锁
- 第一个锁可以放在任意位置，后续的锁必须在父节点锁上时才能上锁
- 可以在任意时刻释放锁
- 释放锁后不能再上锁

没有死锁，冲突可串行化。不能保证可恢复性。

### Multiple Granularity

可以对记录上锁，也可以对表上锁。锁可以加在：

- database
- area
- table
- record

#### Intention Lock Mode

意向锁

- 如果记录上加 X 锁，那么在表上要加意向排他锁 IX
- 如果记录上加 S 锁，那么在表上要加意向排他锁 IS
- 如果表中记录有 X 锁，但是要对表上 S 锁，那么要加共享意向排他锁 SIX

相容表：

|     | IS | IX | S | SIX | X |
|:-:|:-:|:-:|:-:|:-:|:-:|
| IS  | T | T | T | T | F |
| IX  | T | T | F | F | F |
| S   | T | F | T | F | F |
| SIX | T | F | F | F | F |
| X   | F | F | F | F | F |

## Recovery System

### Failure Classification

- Transaction failure：事务执行失败，常用方法 undo
- System crash：系统崩溃，已经提交的事务要 redo，未提交的事务要 undo
- Disk failure：介质故障，要进行备份

要将日志写入 stable storage，保证即使系统崩溃，日志也不会丢失。

### Log Records

- `<Ti start>` 事务开始
- `<Ti X V1 V2>` 事务 \(T_i\) 对 \(X\) 从 \(V_1\) 修改为 \(V_2\)
- `<Ti commit>` 事务提交
- `<Ti abort>` 事务回滚
- `<Ti X V>` 事务 \(T_i\) 对 \(X\) 修改为 \(V\)，用于回滚

### Write-ahead Logging

先写日志原则。数据修改之前，和数据有关的记录要先写入日志。

### Transaction Commit

日志写入代表事务提交，具体数据不一定写入磁盘。

### Checkpoint

重演时，从最近的 checkpoint 开始重演。生成 checkpoint 过程：

1. 日志从内存中写入 stable storage
2. 把 buffer 中的数据全部写入磁盘
3. 写一行日志 `<checkpoint L>`，L 为活跃事务表
4. 生成 checkpoint 时所有其他事务都会暂停

根据日志量决定何时生成 checkpoint。

### Log Record Buffering

将数据 buffer 中的块写入磁盘时，要先将对应的日志写入磁盘。

- no-force policy: 事务 commit 了但不强制日志写入磁盘
- steal policy: 事务未 commit 但是能将脏数据写入磁盘（同时对应日志也写入磁盘）

### Fuzzy Checkpoint

做 checkpoint 时，不一次将所有脏数据写入磁盘。而是记录脏数据位置，在之后慢慢写入，当脏数据全部写入后，再认定该 checkpoint 有效。

### Failure with Loss of Nonvolatile Storage

如果发生了介质故障，要进行恢复。基本操作是做 backup。

发生故障时做快照，然后慢慢备份。

### ARIES Recovery Algorithm

- LSN：log sequence number，日志序列号。
- Page LSN：每一个数据块记录一个 LSN，记录这个块反映了最近日志的修改结果
- Dirty Page Table：记录 checkpoint 时的脏数据块
    - 记录 PageLSN 和 RecLSN，RecLSN 代表从此 LSN 开始的日志记录都没有反映到磁盘上（即哪条日志记录导致了这个脏数据块）

#### Recovery Process

1. Analysis：分析日志，找到最近的 checkpoint
    - undo-list：得到哪些事务需要回滚，根据活跃事务表，再根据日志找到哪些事务需要回滚
    - dirty page table：得到最新的 dirty page table
    - redoLSN：redo 需要从哪条日志开始，可能在 checkpoint 之前，dirty page table 中 RecLSN 的最小值
2. Redo：重演日志：从 redoLSN 开始，重演日志
3. Undo：回滚事务：根据 undo-list 回滚事务