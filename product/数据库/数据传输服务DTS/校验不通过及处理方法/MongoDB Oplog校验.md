
## 检查详情
- 检查要求：进行全量 + 增量迁移时，能够从源端获取到 Oplog。
- 检查说明：增量迁移需要通过 Oplog 进行回放，如果源端 local 库下不存在 oplog.rs 或者 oplog.$main 表格，则无法获取 Oplog。

## 修复方法
将源端以副本集或者主从方式启动，保证操作能够产生 Oplog，并且记录在源端 local 库下。
