备份

```sh
pg_dump -h 172.30.7.100 -p 5000 -U admin adf_db_other> D:\资料备份\数据库备份\adf_db_other.bak
```

还原

```sh
psql -h localhost -U postgres -d adf_db_other <  D:\资料备份\数据库备份\adf_db_other.bak
```



