# 实验4：对象管理

## 实验目的：

了解Oracle表和视图的概念，学习使用SQL语句Create Table创建表，学习Select语句插入，修改，删除以及查询数据，学习使用SQL语句创建视图，学习部分存储过程和触发器的使用。

## - 实验场景：

假设有一个生产某个产品的单位，单位接受网上订单进行产品的销售。通过实验模拟这个单位的部分信息：员工表，部门表，订单表，订单详单表。

## 实验内容：

### 实验步骤

#### 1.登录用户

![image-20210413105100269](./image-20210413105100269.png)

#### 2.执行sql脚本(完成建表和数据插入)

![image-20210413112236337](./image-20210413112236337.png)

![](./image-20210426090355493.png)

![image-20210426090401307](./image-20210426090401307.png)

#### 3.查询数据

1.查询某个员工的信息

```.sql
select * from ORDERS where  order_id=1;
select * from ORDER_DETAILS where  order_id=1;
select * from VIEW_ORDER_DETAILS where order_id=1;
```

![image-20210413114025953](./image-20210413114025953.png)

![image-20210413114113696](./image-20210413114113696.png)

![image-20210413114054368](./image-20210413114054368.png)

2.递归查询某个员工及其所有下属，子下属员工。

```.sql
WITH A (EMPLOYEE_ID,NAME,EMAIL,PHONE_NUMBER,HIRE_DATE,SALARY,MANAGER_ID,DEPARTMENT_ID) AS
  (SELECT EMPLOYEE_ID,NAME,EMAIL,PHONE_NUMBER,HIRE_DATE,SALARY,MANAGER_ID,DEPARTMENT_ID
    FROM employees WHERE employee_ID = 11
    UNION ALL
  SELECT B.EMPLOYEE_ID,B.NAME,B.EMAIL,B.PHONE_NUMBER,B.HIRE_DATE,B.SALARY,B.MANAGER_ID,B.DEPARTMENT_ID
    FROM A, employees B WHERE A.EMPLOYEE_ID = B.MANAGER_ID)
SELECT * FROM A;
```

![image-20210413115135108](./image-20210413115135108.png)

```.sql
SELECT * FROM employees START WITH EMPLOYEE_ID = 11 CONNECT BY PRIOR  EMPLOYEE_ID = MANAGER_ID;
```

![image-20210413115216820](./image-20210413115216820.png)

3.查询订单表，并且包括订单的订单应收货款: Trade_Receivable= sum(ORDER_DETAILS.ProductNum*ORDER_DETAILS.ProductPrice)- Discount。

```.sql
select * from ORDERS;
```

![image-20210426085445061](./image-20210426085445061.png)

4.查询订单详表，要求显示订单的客户名称和客户电话，产品类型用汉字描述。

```.sql
select o.customer_name, o.customer_tel, p.product_type as 产品类型
from orders o,order_details d, products p
where o.order_id=d.order_id
and d.product_name=p.product_name
```

![image-20210426085647028](./image-20210426085647028.png)

5.查询出所有空订单，即没有订单详单的订单。

```.sql
select * 
from orders
where order_id not in 
(select o.order_id 
from orders o,order_details d 
where o.order_id=d.order_id)
```

![image-20210426085841218](./image-20210426085841218.png)

6.查询部门表，同时显示部门的负责人姓名。

```.sql
select d.* ,e.name
from departments d,employees e
where d.department_id=e.department_id
and e.manager_id=d.department_id
```

![image-20210426085931834](./image-20210426085931834.png)

7.查询部门表，统计每个部门的销售总金额。

```.sql

select d.department_name, SUM(sum1)
from(select (d.product_num*d.product_price) sum1
from order_details d,orders o, departments d, employees e 
WHERE d.department_id=e.department_id
and o.employee_id = e.employee_id 
and o.order_id=d.order_id),departments d
group by d.department_name
```

![image-20210426090008903](./image-20210426090008903.png)



#### 4、统计情况

1. 查看数据文件的使用情况

   ```.sql
   select * from dba_data_files;
   ```

   

   ![image-20210426092123471](./image-20210426092123471.png)

   2.查看表空间的使用情况

```.sql
SELECT a.tablespace_name "表空间名",
total "表空间大小",
free "表空间剩余大小",
(total - free) "表空间使用大小",
total / (1024 * 1024 * 1024) "表空间大小(G)",
free / (1024 * 1024 * 1024) "表空间剩余大小(G)",
(total - free) / (1024 * 1024 * 1024) "表空间使用大小(G)",
round((total - free) / total, 4) * 100 "使用率 %"
FROM (SELECT tablespace_name, SUM(bytes) free
FROM dba_free_space
GROUP BY tablespace_name) a,
(SELECT tablespace_name, SUM(bytes) total
FROM dba_data_files
GROUP BY tablespace_name) b
WHERE a.tablespace_name = b.tablespace_name
```



​	![image-20210426092250000](./image-20210426092250000.png)

​	3.查看数据文件大小

```.shell
ls -lh
```



![image-20210426092636540](./image-20210426092636540.png)

#### 6.总结

通过本次实验，我了解了Oracle表和视图的概念，Oracle 数据库数据对象中最基本的是表和视图，其他还有约束、序列、函数、存储过程、包、触发器等。对数据库的操作可以基本归结为对数据对象的操作,理解和掌握 Oracle数据库对象是学习Oracle的捷径。本次实验还让我学会了使用SQL语句Create Table创建表，学会了Select语句插入，修改，删除以及查询数据、使用SQL语句创建视图，并且学会了部分存储过程和触发器的使用。实验完成后我对oralce的使用更加熟练了希望能将所学的用到今后的学习生活中。