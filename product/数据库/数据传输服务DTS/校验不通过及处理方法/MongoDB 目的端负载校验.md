
## 检查详情
- 检查要求：DTS 迁移会导致目的端负载变高，如果在迁移过程中，目的端有业务使用，则会发出校验警告。警告不会阻塞任务的继续，但会对业务有一定影响，请用户评估后自行决定是否忽略警告。
- 业务影响：MongoDB DTS 采用逻辑同步的方式进行数据迁移，会对目的端 CPU 负载造成一定压力，如果目的端有业务使用，请谨慎评估发起。

## 修改方法
停止目标端的业务使用，重新执行校验任务。
