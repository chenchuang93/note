# 登陆
sqlplus username/password@sid

sqlplus / as sysdba

# 切换用户
conn /as sysdba

# 创建表空间
sqlplus / as sysdba  

select name from v$tempfile; 
select name from v$datafile;

create tablespace TEST01 datafile '/u01/app/oracle/oradata/XE/test01.dbf' size 100M autoextend on next 5M maxsize unlimited;

create user chuang identified by chuang123 default tablespace TEST01;

grant connect,resource to chuang;
grant unlimited tablespace to chuang;
grant create database link to chuang;
grant select any sequence,create materialized view to chuang;

ALTER PROFILE DEFAULT LIMIT PASSWORD_LIFE_TIME UNLIMITED;  

# 中文乱码
查看server段编码 
```
select userenv('language') from dual; 

USERENV('LANGUAGE')
----------------------------------------------------
AMERICAN_AMERICA.AL32UTF8
```

再客户端设置环境变量NLS_LANG=AMERICAN_AMERICA.AL32UTF8

# tnsnames.ora
```
XE = 
 (DESCRIPTION = 
  (ADDRESS_LIST = 
  (ADDRESS = (PROTOCOL = TCP)(HOST = 192.168.1.10)(PORT = 1521)))
  (CONNECT_DATA = 
    (SERVICE_NAME = xe) 
  )
 )
```

# 启动监听
lsnrctl start

# sql脚本地址
http://www.forta.com/books/0672336073/ 

# 视图
grant select any table, create view to chuang;
```
create view ProductCustomers as
  select cust_name, cust_contact, prod_id
    from customers, orders, orderItems
   where customers.cust_id = orders.cust_id
     and orderitems.order_num = orders.order_num;
```
```
select cust_name, cust_contact
  from ProductCustomers
 where prod_id = 'RGAN01';
```

# 存储过程
存储过程的is与as在存储过程(PROCEDURE)和函数(FUNCTION)中没有区别；  
在视图(VIEW)中只能用AS不能用IS；  
在游标(CURSOR)中只能用IS不能用AS。  
```
create procedure MailingListCount (
  ListCount out integer
 )
 is v_rows integer; -- 定义变量
begin 
  select count(*) into v_rows from customers where not cust_email is null;
  ListCount := v_rows;
end;
```
调用
```
SQL> var ReturnValue NUMBER
SQL> EXEC MailingListCount(:ReturnValue);

PL/SQL procedure successfully completed

ReturnValue
---------
3
SQL> print ReturnValue
ReturnValue
---------
3
```
# 排序order by
```
select prod_name 
from products 
order by prod_name;
```
order by 是select语句中的最后一条语句。  
```
select prod_id, prod_price, prod_name 
from products 
order by prod_price, prod_name;
```
asc升序(默认)，desc降序。
```
select prod_name 
from products 
order by prod_name desc;
```
```
select prod_id, prod_price, prod_name 
from products 
order by prod_price desc, prod_name;
```

# 模糊查询like
区分大小写。
```
select prod_id, prod_name 
from products 
where prod_name like 'Fish%';
```

# 拼接字段
有的用'+',有的用'||'。
```
select vend_name || '(' || vend_country || ')' 
from vendors order by vend_name;
```
RTRIM函数去掉右边空格，别名as
```
select rtrim(vend_name) || '(' || rtrim(vend_country) || ')' as vend_title
from vendors order by vend_name;
```
mysql用函数concat
```
select Concat(vend_name, '(', vend_country, ')') as vend_title
from vendors order by vend_name;
```

# 算术计算
```
select prod_id,
       quantity,
       item_price,
       quantity * item_price as expended_price
  from Orderitems
 where order_num = 20008;
```

# 函数
文本处理函数
```
select vend_name, upper(vend_name) as vend_name_upcase
  from vendors
 order by vend_name;
```
SOUNDEX()函数
```
select cust_name, cust_contact
  from customers
 where soundex(cust_contact) = soundex('Michael Green');
```

日期和时间处理函数  
postgresql中是date_part(),sql server是datepart()
```
select order_num, order_date
  from orders
 where to_number(to_char(order_date, 'YYYY')) = 2012;
```
to_date
```
 select order_num
   from orders
  where order_date between to_date('1-1-2012', 'dd-mm-yyyy') and
        to_date('12-31-2012', 'mm-dd-yyyy');
```

# 聚集函数
avg()对表中行数就行计数并计算其列值之和，求得该列的平均值。
```
select avg(prod_price) as avg_price from products where vend_id = 'DLL01';
```
count函数确定表中行的数目或符合特定条件的行的数目。
```
select count(*) as num_cust from customers;
```
```
select count(cust_email) as num_cust from customers;
```
MAX函数返回指定列中的最大值,MIN函数返回指定列中的最小值。
```
select MAX(prod_price) as max_price from products;
```
SUM返回指定列值的和（总计）
```
select order_num, sum(quantity) as items_ordered
  from orderitems
 where order_num = 20005;
```

# 组合函数，group by
group by子句中列出的每一列都必须是检索列或者有效的表达式。
除聚集计算语句外，select语句中的每一列都必须在group by子句中给出。
group by子句必须出现在where子句之后，order by子句之前。
```
select vend_id, count(*) as num_prods from products group by vend_id;
```
group by子句可以包含任意数目的列
```
select order_num, prod_id, sum(quantity) as items_ordered
  from orderitems
 group by order_num, prod_id;
```
过滤分组，having（where 过滤的是行而不是分组）
```
select cust_id, count(*) as orders
  from orders
 group by cust_id
having count(*) >= 2;
```

```
select vend_id, count(*) as num_prods
  from products
 where prod_price >= 4
 group by vend_id
having count(*) >= 2;
```
```
select order_num, count(*) as items
  from orderitems
 group by order_num
having count(*) >= 3
 order by items, order_num;
```

# 子查询
```
select cust_name, cust_contact
  from customers
 where cust_id in
       (select cust_id
          from orders
         where order_num in
               (select order_num from orderitems where prod_id = 'RGAN01'));
```

作为计算字段使用子查询
```
select cust_name,
       cust_state,
       (select count(*) from orders where orders.cust_id = customers.cust_id) as orders
  from customers
 order by cust_name;
```

# 联结表
叉联结，没有where则返回笛卡儿积
```
select vend_name, prod_name, prod_price
  from vendors, products
```
内联结。等值联结。
```
select vend_name, prod_name, prod_price
  from vendors, products
 where vendors.vend_id = products.vend_id;
```
```
select vend_name, prod_name, prod_price
  from vendors inner join products
    on vendors.vend_id = products.vend_id;
```
使用连接替代子查询
```
select cust_name, cust_contact
  from customers
 where cust_id in
       (select cust_id
          from orders
         where order_num in
               (select order_num from orderitems where prod_id = 'RGAN01'));

select cust_name, cust_contact
  from customers, orders, orderitems
 where customers.cust_id = orders.cust_id
   and orders.order_num = orderitems.order_num
   and orderitems.prod_id = 'RGAN01';
```
自联结。oracle中列别名能用as，表别名不能用as。
```
select c1.cust_id, c1.cust_name, c1.cust_contact
  from customers c1, customers c2
 where c1.cust_name = c2.cust_name
   and c2.cust_contact = 'Jim Jones';
```
自然联结。大部分内联结都是自然联结。
```
```
外联结
```
select customers.cust_id, orders.order_num
  from customers
  left outer join orders
    on customers.cust_id = orders.cust_id;
```
带聚集函数的联结
```
select customers.cust_id, count(orders.order_num) as num_ord
  from customers
 inner join orders
    on customers.cust_id = orders.cust_id
 group by customers.cust_id;
```
```
select customers.cust_id, count(orders.order_num) as num_ord
  from customers
  left outer join orders
    on customers.cust_id = orders.cust_id
 group by customers.cust_id;
```

# 组合查询
```
select cust_name, cust_contact, cust_email
  from customers
 where cust_state in ('IL', 'IN', 'MI')
union
select cust_name, cust_contact, cust_email
  from customers
 where cust_name = 'Fun4All'
 order by cust_name, cust_contact;
```





