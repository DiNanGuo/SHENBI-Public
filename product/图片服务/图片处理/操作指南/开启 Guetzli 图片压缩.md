## 操作场景
Guetzli 图片压缩是数据万象推出的视觉无损压缩服务，能够对 JPG 图像进行高比例压缩，为使用者节省下载流量，并加快用户下载速度，提升体验。它利用人眼对于部分色域及图片细节的不敏感性，在不影响视觉效果的前提下有选择地丢弃细节信息，使得在相同质量条件下比原图节省约35% - 50%的图片大小。

## 操作步骤
>! 
> - 开发者可通过控制台开通服务，开通后该存储桶中的图片在下载时会进行 Guetzli 压缩，并将压缩后的图片更新至 CDN 节点（如已开通 CDN 服务）。
> - Guetzli 压缩是付费服务，具体费用可查看 [计费项](https://cloud.tencent.com/document/product/1246/45274)。
>
1. 登录 [数据万象控制台](https://console.cloud.tencent.com/ci)。
2. 在左侧导航栏中，单击**存储桶管理**，进入存储桶管理页面。
3. 单击需操作的存储桶（如 buckettest），进入存储桶配置页面。
4. 在左侧导航栏中，单击**图片处理**，选择**图片压缩**页签，找到**Guetzli 图片压缩**配置项。
5. 单击**编辑**，开启 Guetzli 图片压缩功能，即可使用。
![](https://main.qcloudimg.com/raw/6ec2b5af7e7712ec68b9c77c4afd03e3.png)
>?
> - 开启 Guetzli 后，首次访问图片会返回普通 JPG 原图，同时启动异步 Guetzli 处理，处理完成后再次请求该图片会得到压缩后的结果图。
> - 当前 Guetzli 图片压缩服务仅对质量 q > 70、像素小于400万的 JPG 图片做处理。
> - 数据万象为每个账户提供每月3000张的免费体验额度，超出后将正常计费。未使用额度不会累积至下一月。
> 


## Guetzli 状态码
开启 Guetzli 压缩功能后，对应存储桶中图片请求的 HTTP 头部会增加 x-GuetzliState 标识，用以标注 Guetzli 压缩处理的状态。具体内容如下：

| x-GuetzliState 状态码 | 含义             |
| ----------------- | -------------- |
| < 0                | 无法处理（不满足压缩条件）  |
| 0                 | 不进行 Guetzli 压缩处理 |
| 1                 | 已发起 Guetzli 压缩请求 |
| 2                 | Guetzli 压缩中     |
| 3                 | 原图缓存未过期，暂不处理   |
| 100               | 压缩成功           |
