title: 常用SQL语句

date: 2020-05-31 20:05:56

categories:
- 数据库

tags:
- database
- sql
---

### 常用的SQL语句

```sql
TRUNCATE TABLE 表名 #清空表

select * from job_finish_info;

select * from lsb_events_exechostlist;

select job_id, event_type from lsb_events;

mysql show columns from 表名;

INSERT INTO table_name VALUES (值1, 值2,....)

INSERT INTO table_name (列1, 列2,...) VALUES (值1, 值2,....)
```