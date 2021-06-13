# Leetcode sql 题解

# [595. 大的国家](https://leetcode.com/problems/big-countries/)

```sql
# Write your MySQL query statement below
SELECT 
name, population, area
FROM 
World
WHERE 
area > 3000000 or population > 25000000;
```

# [627. Swap Salary](https://leetcode.com/problems/swap-salary/)

如果没有 ELSE sex 会将 sex 置空，但是这道题性别只有男女，所以不写也对。

```sql
# Write your MySQL query statement below
UPDATE salary
SET sex =
CASE sex
WHEN 'f' THEN 'm'
when 'm' THEN 'f'
ELSE sex
END
```

# [620. Not Boring Movies](https://leetcode.com/problems/not-boring-movies/)

ASC 升序 ， DESC 降序。默认 ASC

```sql
# Write your MySQL query statement below
SELECT *
FROM 
    cinema
WHERE
    id % 2 = 1 
    AND description != "boring"
ORDER BY 
    rating DESC;
```

# [596. Classes More Than 5 Students](https://leetcode.com/problems/classes-more-than-5-students/)

HAVING 针对组筛选，WHERE 针对行筛选

```sql
# Write your MySQL query statement below
SELECT 
    class
FROM
    courses
GROUP BY
    class
HAVING 
    COUNT(DISTINCT student) >= 5;
```

# [182. Duplicate Emails](https://leetcode.com/problems/duplicate-emails/)

```sql
# Write your MySQL query statement below
select
    Email
from
    Person
group by 
    Email
having 
    count(*) >= 2;
```

# [196. Delete Duplicate Emails](https://leetcode.com/problems/delete-duplicate-emails/)

```sql
# Write your MySQL query statement below
delete
    p1
from
    Person p1 , Person p2
where 
    p1.Email = p2.Email and p1.Id > p2.Id
;
```

# [176. 第二高的薪水](https://leetcode-cn.com/problems/second-highest-salary/)

首先找到最大值，然后在从中找出比最大值小的最大值就是第二高的值。

```sql
# Write your MySQL query statement below
select max(Salary)  SecondHighestSalary
from Employee
where Salary < (select max(Salary) from Employee)
```

# [177. 第N高的薪水](https://leetcode-cn.com/problems/nth-highest-salary/)

limit n子句表示查询结果返回前n条数据

offset n表示跳过x条语句

limit y offset x 分句表示查询结果跳过 x 条数据，读取前 y 条数据

```sql
CREATE FUNCTION getNthHighestSalary(N INT) RETURNS INT
BEGIN
	SET N = N - 1;
	RETURN (
	 SELECT DISTINCT salary 
     FROM employee 
     ORDER BY salary DESC 
     LIMIT N, 1
	);
END
```


# [181. 超过经理收入的员工](https://leetcode-cn.com/problems/employees-earning-more-than-their-managers/)

```sql
# Write your MySQL query statement below
select Name Employee
from Employee AS a
where Salary > (
    select Salary
    from Employee
    where Id = a.Managerid
)
```

# [183. 从不订购的客户](https://leetcode-cn.com/problems/customers-who-never-order/)

```sql
# Write your MySQL query statement below
select Name Customers
from Customers
WHERE Id NOT IN (
  SELECT CustomerId 
  FROM Orders 
  GROUP BY CustomerId
);
```

# [197. 上升的温度](https://leetcode-cn.com/problems/rising-temperature/)

```sql
# Write your MySQL query statement below
select a.Id 
from  Weather as a join Weather as b on a.Temperature > b.Temperature and dateDiff(a.RecordDate,b.RecordDate) = 1 
```