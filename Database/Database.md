# MYSQL DataBase

## SELECT 查询

### **基本语法**

- `SELECT`: 关键字，用于指定要查询的列或表达式。
- `column1, column2, ...`: 指定要选择的列。您可以选择多个列，用逗号分隔，或使用通配符`*`选择所有列。
- `FROM`: 关键字，指定要从中检索数据的表或视图。
- `table_name`: 指定要查询的表名。
- `WHERE`: 关键字，可选项，用于指定筛选条件。
- `condition`: 指定筛选条件的逻辑表达式。只有满足条件的行才会被返回。

```mysql
SELECT (column) FROM (table_name) WHERE (condition)
```

1. 选择所有列：

   ```mysql
   SELECT * FROM students 
   ```

2. 选择特定列：

   ```mysql
   SELECT name FROM students 
   ```

3. 添加筛选条件：

   ```mysql
    SELECT name FROM students WHERE age>18 OR age IS null 
   ```

4. 使用逻辑运算符：

   ```mysql
    SELECT name FROM students WHERE age>18 AND sex = 'f' 
   ```

5. 排序结果：

   ```mysql
   SELECT name,age FROM students ORDER BY age DESC
   ```

6. 聚合函数和分组：

   ```mysql
   SELECT classes, AVG(grade) as avg_grade FROM students ORDER BY classes
   ```

7. 连接多个表：

   ```mysql
   SELECT classes.no, students.name FROM classes JOIN students ON classes.no = students.class
   ```

8. 限制结果集：

   ```mysql
   SELECT * FROM students LIMIT 10
   ```

9. 结果去重：使用`DISTINCT`关键字来去除结果中的重复行

   ```mysql
   SELECT DISTINCT product_name
   FROM products
   ORDER BY price ASC;
   ```

10. 



## **嵌套子查询**



### Leetcode绕题

- **每台机器的进程平均运行时间**

  表: `Activity`

  ```mysql
  +----------------+---------+
  | Column Name    | Type    |
  +----------------+---------+
  | machine_id     | int     |
  | process_id     | int     |
  | activity_type  | enum    |
  | timestamp      | float   |
  +----------------+---------+
  该表展示了一家工厂网站的用户活动.
  (machine_id, process_id, activity_type) 是当前表的主键.
  machine_id 是一台机器的ID号.
  process_id 是运行在各机器上的进程ID号.
  activity_type 是枚举类型 ('start', 'end').
  timestamp 是浮点类型,代表当前时间(以秒为单位).
  'start' 代表该进程在这台机器上的开始运行时间戳 , 'end' 代表该进程在这台机器上的终止运行时间戳.
  同一台机器，同一个进程都有一对开始时间戳和结束时间戳，而且开始时间戳永远在结束时间戳前面.
  ```

  现在有一个工厂网站由几台机器运行，每台机器上运行着相同数量的进程. 请写出一条SQL计算每台机器各自完成一个进程任务的平均耗时.

  完成一个进程任务的时间指进程的`'end' 时间戳` 减去 `'start' 时间戳`. 平均耗时通过计算每台机器上所有进程任务的总耗费时间除以机器上的总进程数量获得.

  结果表必须包含`machine_id（机器ID）` 和对应的 **average time（平均耗时）** 别名 `processing_time`, 且**四舍五入保留3位小数.**

  以 **任意顺序** 返回表。

  具体参考例子如下。

  **示例 1:**

  ```mysql
  输入：
  Activity table:
  +------------+------------+---------------+-----------+
  | machine_id | process_id | activity_type | timestamp |
  +------------+------------+---------------+-----------+
  | 0          | 0          | start         | 0.712     |
  | 0          | 0          | end           | 1.520     |
  | 0          | 1          | start         | 3.140     |
  | 0          | 1          | end           | 4.120     |
  | 1          | 0          | start         | 0.550     |
  | 1          | 0          | end           | 1.550     |
  | 1          | 1          | start         | 0.430     |
  | 1          | 1          | end           | 1.420     |
  | 2          | 0          | start         | 4.100     |
  | 2          | 0          | end           | 4.512     |
  | 2          | 1          | start         | 2.500     |
  | 2          | 1          | end           | 5.000     |
  +------------+------------+---------------+-----------+
  输出：
  +------------+-----------------+
  | machine_id | processing_time |
  +------------+-----------------+
  | 0          | 0.894           |
  | 1          | 0.995           |
  | 2          | 1.456           |
  +------------+-----------------+
  解释：
  一共有3台机器,每台机器运行着两个进程.
  机器 0 的平均耗时: ((1.520 - 0.712) + (4.120 - 3.140)) / 2 = 0.894
  机器 1 的平均耗时: ((1.550 - 0.550) + (1.420 - 0.430)) / 2 = 0.995
  机器 2 的平均耗时: ((4.512 - 4.100) + (5.000 - 2.500)) / 2 = 1.456
  ```

  **Solution**

  1. 先获取每个进程开始以及结束的时间戳作为子查询表；
  2. 计算该子查询表的平均时间得到新表。

  ```mysql
  # 获取每个进程开始以及结束的时间戳作为子查询表。
  # 当activity_type的值等于 'start'/'end' 时，返回 timestamp 的值，否则返回NULL；
  # 查询会按照machine_id和process_id进行分组，然后在每个组内计算最小开始时间和最大结束时间。
  # 为什么要算最大结束时间而不是最小结束时间呢？选择最大的结束时间而不是最小的结束时间是因为同一台机器上的进程任务可能存在重叠的情况。这意味着一个进程的开始时间可能早于另一个进程的结束时间。如果我们选择最小的结束时间，那么在计算耗时时会将这两个进程任务的时间跨度计算在内，导致耗时被计算为一个较长的时间。
  
  SELECT machine_id, process_id,  
  MIN(CASE WHEN activity_type = "start" THEN timestamp END) AS start_time, 
  Max(CASE WHEN activity_type = "start" THEN timestamp END) AS end_time
  FROM Activity
  GROUP BY machine_id, process_id
  ```

  ```mysql
  # 计算该子查询表的平均时间得到新表。
  # ROUND(...,3) 表示
  SELECT machine_id, ROUND(SUM(start_time-end_time)/Count(DISTINCT process_id),3) AS processing_time
  FROM (
      SELECT machine_id, process_id,  
      MIN(CASE WHEN activity_type = "start" THEN timestamp END) AS start_time, 
      Max(CASE WHEN activity_type = "start" THEN timestamp END) AS end_time
      FROM Activity
      GROUP BY machine_id, process_id
  ) AS subquery
  GROUP BY machine_id;
  ```

  

- 



## JION 连接

![sql-join](D:\大四\前端开发实习学习准备\Frontend-projects\Notebook\Database\pics\sql-join.png)

JOIN连接是用于将两个或多个表基于其关联列进行关联的操作。JOIN操作能够根据指定的连接条件将相关的行从不同的表中组合起来，生成一个包含多个表的结果集。

### 基本语法

1. 内连接 Inner Join

用场景：当需要返回两个或多个表中匹配的行时，内连接是最常用的JOIN类型。它只返回在连接条件下匹配的行。

```mysql
SELECT orders.oder_id, customers.customer_name FROM oders INNER JOIN customers ON oders.customer_id = customers.customer_id 
```

2. 左连接 Left Join

使用场景：当需要返回左表中所有行以及与之匹配的右表行时，左连接非常有用。如果右表中没有匹配的行，则返回NULL值。

```mysql
SELECT customers.customer_id, orders.oder_id FROM customers LEFT JOIN orders ON customers.customer_id = oders.customer_id
```

3. 右连接 Right Join

使用场景：与左连接相反，右连接返回右表中的所有行以及与之匹配的左表行。如果左表中没有匹配的行，则返回NULL值。

```mysql
SELECT customers.customer_id, orders.oder_id FROM customers RIGHT JOIN orders ON customers.customer_id = oders.customer_id
```

4. 全连接 Full Join

使用场景：全连接返回连接表中的所有行，无论是否匹配。如果在另一个表中没有匹配的行，则返回NULL值。

```mysql
SELECT customers.customer_id, orders.oder_id FROM customers FULL JOIN orders ON customers.customer_id = oders.customer_id
```



Join连接是可以持续使用的：

```mysql
SELECT *
FROM table1
LEFT JOIN table2 ON table1.id = table2.id
LEFT JOIN table3 ON table1.id = table3.id;
```

或者：

```mysql
SELECT *
FROM table1
RIGHT JOIN table2 ON table1.id = table2.id
RIGHT JOIN table3 ON table2.id = table3.id;
```



### Leetcode绕题

- **ON条件需要计算差值：**

  **上升的温度**

  ```mysql
  输入：
  Weather 表：
  +----+------------+-------------+
  | id | recordDate | Temperature |
  +----+------------+-------------+
  | 1  | 2015-01-01 | 10          |
  | 2  | 2015-01-02 | 25          |
  | 3  | 2015-01-03 | 20          |
  | 4  | 2015-01-04 | 30          |
  +----+------------+-------------+
  输出：
  +----+
  | id |
  +----+
  | 2  |
  | 4  |
  +----+
  解释：
  2015-01-02 的温度比前一天高（10 -> 25）
  2015-01-04 的温度比前一天高（20 -> 30）
  ```

  **Solution**

  ```mysql
  SELECT DISTINCT w1.id
  FROM Weather as w1
  CROSS JOIN Weather as w2
  ON DATEDIFF(w1.recordDate,w2.recordDate)=1 AND w1.temperature>w2.temperature
  ```

- **CROSS笛卡尔连接**

  **学生们参加各科测试的次数**

  学生表: `Students`

  ```mysql
  +---------------+---------+
  | Column Name   | Type    |
  +---------------+---------+
  | student_id    | int     |
  | student_name  | varchar |
  +---------------+---------+
  主键为 student_id（学生ID），该表内的每一行都记录有学校一名学生的信息。
  ```

  科目表: `Subjects`

  ```mysql
  +--------------+---------+
  | Column Name  | Type    |
  +--------------+---------+
  | subject_name | varchar |
  +--------------+---------+
  主键为 subject_name（科目名称），每一行记录学校的一门科目名称。
  ```

  考试表: `Examinations`

  ```mysql
  +--------------+---------+
  | Column Name  | Type    |
  +--------------+---------+
  | student_id   | int     |
  | subject_name | varchar |
  +--------------+---------+
  这张表压根没有主键，可能会有重复行。
  学生表里的一个学生修读科目表里的每一门科目，而这张考试表的每一行记录就表示学生表里的某个学生参加了一次科目表里某门科目的测试。
  ```

  要求写一段 SQL 语句，查询出每个学生参加每一门科目测试的次数，结果按 `student_id` 和 `subject_name` 排序。

  查询结构格式如下所示：

  ```mysql
  Students table:
  +------------+--------------+
  | student_id | student_name |
  +------------+--------------+
  | 1          | Alice        |
  | 2          | Bob          |
  | 13         | John         |
  | 6          | Alex         |
  +------------+--------------+
  Subjects table:
  +--------------+
  | subject_name |
  +--------------+
  | Math         |
  | Physics      |
  | Programming  |
  +--------------+
  Examinations table:
  +------------+--------------+
  | student_id | subject_name |
  +------------+--------------+
  | 1          | Math         |
  | 1          | Physics      |
  | 1          | Programming  |
  | 2          | Programming  |
  | 1          | Physics      |
  | 1          | Math         |
  | 13         | Math         |
  | 13         | Programming  |
  | 13         | Physics      |
  | 2          | Math         |
  | 1          | Math         |
  +------------+--------------+
  Result table:
  +------------+--------------+--------------+----------------+
  | student_id | student_name | subject_name | attended_exams |
  +------------+--------------+--------------+----------------+
  | 1          | Alice        | Math         | 3              |
  | 1          | Alice        | Physics      | 2              |
  | 1          | Alice        | Programming  | 1              |
  | 2          | Bob          | Math         | 1              |
  | 2          | Bob          | Physics      | 0              |
  | 2          | Bob          | Programming  | 1              |
  | 6          | Alex         | Math         | 0              |
  | 6          | Alex         | Physics      | 0              |
  | 6          | Alex         | Programming  | 0              |
  | 13         | John         | Math         | 1              |
  | 13         | John         | Physics      | 1              |
  | 13         | John         | Programming  | 1              |
  +------------+--------------+--------------+----------------+
  结果表需包含所有学生和所有科目（即便测试次数为0）：
  Alice 参加了 3 次数学测试, 2 次物理测试，以及 1 次编程测试；
  Bob 参加了 1 次数学测试, 1 次编程测试，没有参加物理测试；
  Alex 啥测试都没参加；
  John  参加了数学、物理、编程测试各 1 次。
  ```

  **Solution**

  1. 先让Students和Subjects交叉连接确保每个学生都包含所有的科目；
  2. 左连接Examinations表计算各个科目的考试次数。

  ```mysql
  SELECT S.student_id, S.student_name,S.subject_name, COUNT(E.student_id) AS attended_exams FROM
  ( SELECT St.student_id,St.student_name,Sub.subject_name 
    FROM Students St
    CROSS JOIN Subjects Sub
  ) AS S
  LEFT JOIN Examinations AS E
  ON S.student_id = E.student_id AND S.subject_name = E.subject_name
  Group by student_id, subject_name
  Order by student_id, subject_name  
  ```

- **计算多个值**

  **确认率**

  表: `Signups`

  ```mysql
  +----------------+----------+
  | Column Name    | Type     |
  +----------------+----------+
  | user_id        | int      |
  | time_stamp     | datetime |
  +----------------+----------+
  User_id是该表的主键。
  每一行都包含ID为user_id的用户的注册时间信息。
  ```

  表: `Confirmations`

  ```mysql
  +----------------+----------+
  | Column Name    | Type     |
  +----------------+----------+
  | user_id        | int      |
  | time_stamp     | datetime |
  | action         | ENUM     |
  +----------------+----------+
  (user_id, time_stamp)是该表的主键。
  user_id是一个引用到注册表的外键。
  action是类型为('confirmed'， 'timeout')的ENUM
  该表的每一行都表示ID为user_id的用户在time_stamp请求了一条确认消息，该确认消息要么被确认('confirmed')，要么被过期('timeout')。
  ```

  用户的 **确认率** 是 `'confirmed'` 消息的数量除以请求的确认消息的总数。没有请求任何确认消息的用户的确认率为 `0` 。确认率四舍五入到 **小数点后两位** 。

  编写一个SQL查询来查找每个用户的 确认率 。

  以 任意顺序 返回结果表。

  查询结果格式如下所示。

  **示例1:**

  ```mysql
  输入：
  Signups 表:
  +---------+---------------------+
  | user_id | time_stamp          |
  +---------+---------------------+
  | 3       | 2020-03-21 10:16:13 |
  | 7       | 2020-01-04 13:57:59 |
  | 2       | 2020-07-29 23:09:44 |
  | 6       | 2020-12-09 10:39:37 |
  +---------+---------------------+
  Confirmations 表:
  +---------+---------------------+-----------+
  | user_id | time_stamp          | action    |
  +---------+---------------------+-----------+
  | 3       | 2021-01-06 03:30:46 | timeout   |
  | 3       | 2021-07-14 14:00:00 | timeout   |
  | 7       | 2021-06-12 11:57:29 | confirmed |
  | 7       | 2021-06-13 12:58:28 | confirmed |
  | 7       | 2021-06-14 13:59:27 | confirmed |
  | 2       | 2021-01-22 00:00:00 | confirmed |
  | 2       | 2021-02-28 23:59:59 | timeout   |
  +---------+---------------------+-----------+
  输出: 
  +---------+-------------------+
  | user_id | confirmation_rate |
  +---------+-------------------+
  | 6       | 0.00              |
  | 3       | 0.00              |
  | 7       | 1.00              |
  | 2       | 0.50              |
  +---------+-------------------+
  解释:
  用户 6 没有请求任何确认消息。确认率为 0。
  用户 3 进行了 2 次请求，都超时了。确认率为 0。
  用户 7 提出了 3 个请求，所有请求都得到了确认。确认率为 1。
  用户 2 做了 2 个请求，其中一个被确认，另一个超时。确认率为 1 / 2 = 0.5。
  ```

  **Solution**

  1. 先算出time_stamp的值，再算出action为 "confirmed" 的和
  2. 最后算出确认率

  ```mysql
  # Way 1 
  SELECT T1.user_id, 
  CASE WHEN com_nums IS NULL THEN 0.00 ELSE Round((com_nums/all_nums),2) END as confirmation_rate
  FROM (
    SELECT S.user_id, COUNT(C.time_stamp) as all_nums
    FROM Confirmations C 
    RIGHT JOIN Signups S
    ON C.user_id = S.user_id
    GROUP BY S.user_id
  ) AS T1
  LEFT JOIN (
    SELECT S.user_id, 
    COUNT(C.action) as com_nums
    FROM Confirmations C 
    RIGHT JOIN Signups S
    ON C.user_id = S.user_id
    WHERE C.action = "confirmed"
    GROUP BY S.user_id
  ) AS T2
  ON T1.user_id = T2.user_id 
  GROUP BY T1.user_id
  ```

  ```mysql
  # Way 2
  SELECT S.user_id, 
  CASE WHEN com_nums IS NULL THEN 0.00 ELSE Round((com_nums/COUNT( C.time_stamp )),2) END as confirmation_rate
  FROM Signups AS S 
  LEFT JOIN Confirmations C ON C.user_id = S.user_id
  LEFT JOIN (
    SELECT C2.user_id, COUNT(C2.action) as com_nums
    FROM Confirmations C2
    WHERE C2.action = "confirmed"
    GROUP BY C2.user_id
  ) AS Sub ON Sub.user_id = S.user_id
  GROUP by S.user_id
  ```

  ```
  # Way 3
  ```

  



## 数据库函数





