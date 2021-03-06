
## 什么是 MX 记录？
MX 记录是用于电子邮件系统发邮件时，根据收信人的地址后缀来定位邮件服务器。域名的 MX 记录需要在域名管理界面才可修改。
例如，当有人发邮件给 “cloud@tencent.com” 时，系统将对 “tencent.com” 中的 MX 记录进行解析。若 MX 记录存在，系统可根据 MX 记录的优先级，将邮件转发到与该 MX 相应的邮件服务器上。

## 如何设置 MX 记录？
腾讯云企业邮提供以下两种设置方式：

### 一键添加 MX 记录（推荐）
若您的域名在 DNPod DNS 解析进行托管，可通过腾讯云企业邮提供的一键添加功能快速添加 MX 记录。具体操作可参见 [一键添加 MX 记录](https://cloud.tencent.com/document/product/613/57920)。

### 手动添加 MX 记录

手动设置 MX 记录的流程如下图所示：
![](https://main.qcloudimg.com/raw/5f2464f543e2c7cd962d625824404761.png)
### 步骤1. 进入域名管理页面
域名管理页面是在购买域名时，由域名提供商提供的。如不清楚域名提供商，可先查询域名提供商。
### 步骤2. 进入解析记录设置页面
不同域名提供商填写 MX 记录的设置位置不同，一般是在 “域名管理” 下的 “域名解析”，若未找到设置域名解析的位置，请咨询域名提供商。
### 步骤3. 添加 MX 记录
腾讯企业邮要求设置的 MX 记录如下：
 - 邮件服务器名：mxbiz1.qq.com 优先级：5
 - 邮件服务器名：mxbiz2.qq.com 优先级：10

目前仅提供以下2个域名提供商的 MX 记录设置：
- [在腾讯云添加 MX 域名解析](https://cloud.tencent.com/document/product/613/46023)
- [在 DNSPod 添加 MX 域名解析](https://cloud.tencent.com/document/product/613/46024)

