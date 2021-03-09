1. ## 实验内容

### 1.对Oracle12c中的HR人力资源管理系统中的表进行查询与分析。

都是查询部门人数及其平均工资

1. 解决用户授权问题
   1. ![](https://raw.githubusercontent.com/Breezeluoxi/oracle/master/test1/%E6%8E%88%E6%9D%83.png)
2. 查询语句一：
   1. ![](https://raw.githubusercontent.com/Breezeluoxi/oracle/master/test1/%E6%9F%A5%E8%AF%A2%E4%B8%80%E7%BB%93%E6%9E%9C.png)
   2. 直接使用 in 标签限制查询部门为（“IT”, "Sales"）
   3. 通过部门名称分组
   4. 运行效率分析
   5. ![](https://raw.githubusercontent.com/Breezeluoxi/oracle/master/test1/%E6%9F%A5%E8%AF%A21%E7%BB%9F%E8%AE%A1.png)
3. 查询二
   1. ![](https://raw.githubusercontent.com/Breezeluoxi/oracle/master/test1/%E6%9F%A5%E8%AF%A2%E4%BA%8C%E7%BB%93%E6%9E%9C.png)
   2. 直接按部门名称分组
   3. having 语句判断以实现部门筛选（“IT”, "Sales"）
   4. 查询结果统计
   5. ![](https://raw.githubusercontent.com/Breezeluoxi/oracle/master/test1/查询二统计.png)

### 2.首先运行和分析教材中的样例：本训练任务目的是查询两个部门('IT'和'Sales')的部门总人数和平均工资，以下两个查询的结果是一样的。但效率不相同。

- 优化建议
- ![](https://raw.githubusercontent.com/Breezeluoxi/oracle/master/test1/%E4%BC%98%E5%8C%96%E5%BB%BA%E8%AE%AE.png)
- 查询语句一要好一点优化建议创建物理索引
- 创建索引
- ![](https://raw.githubusercontent.com/Breezeluoxi/oracle/master/test1/%E5%88%9B%E5%BB%BA%E7%B4%A2%E5%BC%95.png)

### 3.设计自己的查询语句

```sql

-- 查询总的国家 及其平均工资
SELECT
	jimpie.country_name 国家, 
	AVG( jimpie.salary ) 平均工资 
FROM
	(
        -- 查询员工表与部门表关联 后与 询LOCATIONS与country关联表 的联合表 中的工资和国家名 
	SELECT
		jiam.salary,
		breeze.country_name 
	FROM
		( SELECT * FROM HR.employees e INNER JOIN HR.departments d ON e.department_id = d.department_id ) jiam
		INNER JOIN (
            -- 查询LOCATIONS与country关联表中 国家名字和地区ID
		SELECT
			l.LOCATION_ID,
			c.COUNTRY_NAME 
		FROM
			HR.LOCATIONS l
			INNER JOIN HR.COUNTRIES c ON l.country_id = c.country_id 
		) breeze ON jiam.LOCATION_ID = breeze.LOCATION_ID 
	) jimpie 
GROUP BY
-- 设置分组为国家名字
	jimpie.country_name
```

#### 3.1查询结果

![]()



