Scheduler作为集群的管理调度中心，主要功能:
1. 管理set，提供创建，删除set，set内节点替换等工作；
2. 监控set内各个节点的存活状态，当Set内主节点故障，发起高一致性主备切换流程；
3. 根set内个节点运行状况，更新set状态，设置watch节点，支持自动退化和恢复等；
4. 管理扩容、回档等流程工作；
5. Scheduler自身的容灾通过zk的选举机制完成，保证中心控制节点无单点。
