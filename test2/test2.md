# 											实验二

## 1.实验目的

掌握用户管理、角色管理、权根维护与分配的能力，掌握用户之间共享对象的操作技能。

## 2.实验内容

- 在pdborcl插接式数据中创建一个新的本地角色con_res_view，该角色包含connect和resource角色，同时也包含CREATE VIEW权限，这样任何拥有con_res_view的用户就同时拥有这三种权限。
- 创建角色之后，再创建用户new_user，给用户分配表空间，设置限额为50M，授予con_res_view角色。
- 最后测试：用新用户new_user连接数据库、创建表，插入数据，创建视图，查询表和视图的数据。

## 3.实验步骤

- #### 以system 身份登录到pdborcl，创建角色，并授权分配空间
  - 登录

  ![image-20210315085216147](https://raw.githubusercontent.com/Breezeluoxi/oracle/master/test2/image-20210315085216147.png)
  - 创建角色

  ![image-20210315085243873](https://raw.githubusercontent.com/Breezeluoxi/oracle/master/test2/image-20210315085243873.png)

- 授权角色

  ![image-20210315085303141](https://raw.githubusercontent.com/Breezeluoxi/oracle/master/test2/image-20210315085303141.png)
  - 创建用户，授权用户

  ![image-20210315085544467](https://raw.githubusercontent.com/Breezeluoxi/oracle/master/test2/image-20210315085544467.png)

  - 授权访问表空间

  ![image-20210315085734553](https://raw.githubusercontent.com/Breezeluoxi/oracle/master/test2/image-20210315085734553.png)

  - 授权

  ![image-20210315085635480](https://raw.githubusercontent.com/Breezeluoxi/oracle/master/test2/image-20210315085635480.png)

- #### 第2步：新用户new_pjm连接到pdborcl，创建表mytable和视图myview，插入数据，最后将myview的SELECT对象权限授予hr用户。

  - 登录新用户，创建表

    ![image-20210315090511630](https://raw.githubusercontent.com/Breezeluoxi/oracle/master/test2/image-20210315090511630.png)

  - 插入数据，创建视图

    ![image-20210315090537009](https://raw.githubusercontent.com/Breezeluoxi/oracle/master/test2/image-20210315090537009.png)

  - 查看视图并授权给hr用户

    ![image-20210315090617362](https://raw.githubusercontent.com/Breezeluoxi/oracle/master/test2/image-20210315090617362.png)

- ### 第3步：用户hr连接到pdborcl，查询new_user授予它的视图myview

  - 登录hr

    ![image-20210315090807233](https://raw.githubusercontent.com/Breezeluoxi/oracle/master/test2/image-20210315090807233.png)

  - 查看视图

    ![image-20210315090829774](https://raw.githubusercontent.com/Breezeluoxi/oracle/master/test2/image-20210315090829774.png)

- ## 查看数据库的使用情况

  - 登录system

    ![image-20210315091036884](https://raw.githubusercontent.com/Breezeluoxi/oracle/master/test2/image-20210315091036884.png)

  - 查看空间使用情况

    ![image-20210315091241036](https://raw.githubusercontent.com/Breezeluoxi/oracle/master/test2/image-20210315091241036.png)

    ![image-20210315091637601](https://raw.githubusercontent.com/Breezeluoxi/oracle/master/test2/image-20210315091637601.png)

    ![image-20210315091500737](https://raw.githubusercontent.com/Breezeluoxi/oracle/master/test2/image-20210315091500737.png)

    ![image-20210315091658190](https://raw.githubusercontent.com/Breezeluoxi/oracle/master/test2/image-20210315091658190.png)

## 4.实验总结

完成了这次的实验也是对有关用户的创建，角色的管理有了深刻的认识，明白了应该怎样将自己的表共享给其他用户查看，也是学会了怎样查看磁盘空间占用情况。

