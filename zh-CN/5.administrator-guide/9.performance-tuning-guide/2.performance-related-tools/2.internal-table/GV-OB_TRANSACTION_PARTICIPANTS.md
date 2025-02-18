# GV$OB_TRANSACTION_PARTICIPANTS
#docslug#/oceanbase-database/oceanbase-database/V3.2.3/__all_virtual_trans_stat
## 功能

`GV$OB_TRANSACTION_PARTICIPANTS` 视图用于展示正在进行的事务参与者状态信息。

## 字段说明

|          字段名称           |      类型       | 是否可以为 NULL |       描述        |
|-------------------------|---------------|------------|-----------------|
| TENANT_ID               | bigint(20)    | NO         | 租户 ID           |
| SVR_IP                  | varchar(46)   | NO         | IP地址            |
| SVR_PORT                | bigint(20)    | NO         | 端口              |
| SESSION_ID             | bigint(20)    | NO         | 会话 ID           |
| SCHEDULER_ADDR    | varchar(64)   | NO   |  事务 Session 所在机器 IP   |
| TX_TYPE              | varchar(11)    | NO         | 事务类型            |
| TX_ID                | bigint(20))  | NO         | 事务 ID           |
| LS_ID             | bigint(20)    | NO   |  事务记录涉及的分区信息   | 
| PARTICIPANTS      | varchar(1024) | NO   | 事务涉及的所有分区    | 
| CTX_CREATE_TIME   | timestamp(6)  | YES  | ctx 创建时间     | 
| TX_EXPIRED_TIME     | timestamp(6)  | YES  | 事务超时时间   | 
| STATE             | varchar(13)    | NO   |  状态   |
| ACTION         | varchar(10)    | NO   |  当前事务执行的动作：<ul><li>action = 1 NULL</li><li> action = 2 START </li><li>action = 3 COMMIT </li><li>action = 4 ABORT </li><li>action = 5 DIED</li><li> action = 6 END</li><ul>   | 
| PENDING_LOG_SIZE    | bigint(20)   | NO   |  本事务该参与者尚未刷盘的日志大小   | 
| FLUSHED_LOG_SIZE    | bigint(20)    | NO   | 本事务该参与者已经刷盘的日志大小    | 
| ROLE              | varchar(8)    | NO   |  该副本的角色，取值 0 和 1 分别表示 Leader 和 Follower。   |



## 常用 SQL

`GV$OB_TRANSACTION_PARTICIPANTS` 视图用于统计集群内部所有活跃事务的信息。事务涉及 N 个日志流的写操作，每个日志流产生一条记录，通过该视图可统计耗时长的事务。

统计 `GV$OB_TRANSACTION_PARTICIPANTS` 虚拟表中耗时超过 100 s 的活跃事务参与者。SQL 语法如下：

```sql
obclient> SELECT svr_ip, tx_id, ls_id from GV$OB_TRANSACTION_PARTICIPANTS where tenant_id = xxx and ctx_create_time < date_sub(now(), INTERVAL 100 SECOND);
```

基于查询结果 `tx_id` 字段，可以去 `GV$OB_SQL_AUDIT` 中搜索本事务内的所有 SQL 及耗时等信息。
