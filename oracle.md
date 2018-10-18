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
select userenv('language') from dual; //查看server段编码  
``` 
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