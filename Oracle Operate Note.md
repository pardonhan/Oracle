# Oracle 数据库用户，表空间的创建删除

## 创建数据库表空间
``` sql    
   create tablespace news_tablespace datafile 'F:\oracle\product\10.1.0\oradata\news\news_data.dbf' size 500M;
```
## 设置表空间自动增长
``` sql
    alter database datafile 'route of dbf' autoextend on next 100m;
```
## 创建用户
``` sql
 create user user_name identified by password tablespace tablespace_name;
```
## 用户授权
``` sql
 grant connect,resource to user_name; 
```
## 删除用户
``` sql
    drop user user_name cascade;
```
## 删除表空间，删除表空间前先删除用户
```sql
    DROP TABLESPACE tablespace_name INCLUDING CONTENTS AND DATAFILES;
```

## 查询表空间的名字及其所在位置
   以下语句可直接复制进入 pl/sql Developer 运行。
``` sql
    select tablespace_name, file_id, file_name,
    round(bytes/(1024*1024),0) total_space
    from dba_data_files
    order by tablespace_name;
```

## 查询表空间使用情况

```sql
    select a.tablespace_name,a.bytes/1024/1024 "sum MB",
    (a.bytes-b.bytes)/1024/1024 "used MB",b.bytes/1024/1024 "free MB",
    round (((a.bytes-b.bytes)/a.bytes)*100,2) "used%" from
    (select tablespace_name,sum(bytes) bytes from dba_data_files group by tablespace_name) a,
    (select tablespace_name,sum(bytes) bytes,max (bytes) largest from dba_free_space group by tablespace_name)b
    where a.tablespace_name=b.tablespace_name
    order by ((a.bytes-b.bytes)/a.bytes) desc;
```

## 获取当前周的开始时间
```
SELECT TRUNC(TO_DATE(to_char(sysdate,'yyyy-mm-dd hh24:mi:ss'),'yyyy-mm-dd hh24:mi:ss'),'IW')FROM DUAL
```
