# 轻量化多租户白标客户门户 SaaS
## 项目建设及报价方案

---

# 一、项目背景

本项目计划建设一套面向企业客户使用的：

**多租户、完全白标、自托管 Client Portal SaaS 平台。**

平台主要服务于咨询、设计、软件开发、代理服务、专业服务等 B2B 企业，为每一家租户提供独立的客户门户。

系统重点不是建设完整 CRM、ERP 或营销自动化平台，而是围绕以下核心场景：

- 企业客户门户；
- 企业品牌白标；
- 独立访问域名；
- 客户及员工账号；
- 文件共享；
- 客户服务工单；
- 报价单及 PI；
- SaaS 订阅自动开通。

整体产品方向参考 ClientPortal 类产品，但根据本项目实际需求进行轻量化建设，控制系统复杂度和初期投入。

---

# 二、项目建设目标

本项目完成后，平台运营方可以通过一套系统同时服务多个企业租户。

每个企业租户拥有：

- 自己的 Logo；
- 自己的品牌颜色；
- 自己的企业信息；
- 自己的员工账号；
- 自己的客户账号；
- 自己的文件；
- 自己的工单；
- 自己的报价单；
- 自己的 PI；
- 自己的独立域名。

例如：

```text
平台系统
│
├── Tenant A
│   ├── portal.company-a.com
│   ├── Company A Logo
│   ├── Company A Theme
│   ├── Staff
│   └── Clients
│
├── Tenant B
│   ├── clients.company-b.com
│   ├── Company B Logo
│   ├── Company B Theme
│   ├── Staff
│   └── Clients
│
└── Tenant C
```

Tenant A 与 Tenant B 虽然运行在同一套 SaaS 平台中，但：

- 数据互不可见；
- 用户互不可见；
- 文件互不可见；
- 域名不同；
- 品牌不同。

最终下游客户访问时，看不到平台开发方品牌信息。

---

# 三、系统角色

本项目设计四类核心角色。

## 3.1 平台管理员 Platform Admin

负责整个 SaaS 平台运营。

主要权限：

- Tenant 管理；
- Tenant 开通；
- Tenant 停用；
- 查看订阅状态；
- 查看域名状态；
- 查看 SaaS 使用情况；
- 查看 Webhook；
- 查看系统日志；
- 系统配置。

---

# 3.2 租户管理员 Tenant Admin

每家企业客户的最高管理员。

主要权限：

- 企业资料管理；
- Logo 设置；
- 品牌颜色设置；
- 自定义域名设置；
- 员工管理；
- 客户管理；
- 文件管理；
- 工单管理；
- Quote 管理；
- PI 管理；
- Billing 状态查看。

---

# 3.3 内部员工 Staff

租户内部工作人员。

主要可以：

- 查看客户；
- 管理文件；
- 处理工单；
- 创建 Quote；
- 创建 PI；
- 服务下游客户。

V1 采用标准员工角色，不做复杂自定义权限编辑器。

---

# 3.4 下游客户 Client

企业租户所服务的最终客户。

客户登录以后只能查看自己的：

- 文件；
- 工单；
- Quote；
- PI；
- 个人资料。

不能查看其他客户或企业内部数据。

---

# 四、系统整体架构

```text
                       SaaS Platform
                            │
                            ▼
                 ┌─────────────────────┐
                 │   Platform Admin    │
                 └─────────┬───────────┘
                           │
                 ┌─────────┴─────────┐
                 │                   │
              Tenant A            Tenant B
                 │                   │
        portal.company-a.com  clients.company-b.com
                 │                   │
        ┌────────┴───────┐   ┌──────┴─────────┐
        │                │   │                │
   Tenant Admin        Staff Tenant Admin    Staff
        │                              │
      Clients                        Clients
        │                              │
 ┌──────┼────────┐              ┌──────┼────────┐
 │      │        │              │      │        │
Files Tickets Quote/PI         Files Tickets Quote/PI
```

---

# 五、核心功能清单

# 5.1 SaaS 平台管理

平台管理后台主要包含：

### Dashboard

展示：

- Tenant 总数量；
- Active Tenant；
- Suspended Tenant；
- Active Subscription；
- Domain 状态；
- 系统运行状态。

### Tenant 管理

支持：

- Tenant 列表；
- Tenant 详情；
- Tenant 创建；
- Tenant 状态修改；
- Tenant 停用；
- Tenant 恢复；
- Tenant 基本使用情况查看。

### Subscription 管理

显示：

- Subscription 状态；
- 套餐；
- 到期时间；
- Lemon Squeezy Subscription ID；
- Tenant 状态。

### Domain 管理

显示：

- Tenant；
- 自定义域名；
- DNS 状态；
- SSL 状态；
- 域名启用状态。

---

# 5.2 多租户系统

系统必须支持多个 Tenant 共存。

每一个 Tenant 数据完全独立。

包括：

- 用户数据；
- 客户数据；
- 文件；
- 工单；
- Quote；
- PI；
- 企业配置；
- 域名；
- 品牌信息。

即：

```text
Tenant A
≠
Tenant B
```

任何 Tenant 用户都不能通过修改 URL、接口参数或其他方式访问其他 Tenant 数据。

---

# 5.3 企业白标 Branding

Tenant Admin 可以自行设置品牌。

包括：

### 企业资料

- Company Name；
- Company Address；
- Website；
- Support Email；
- Phone；
- PDF Footer。

### 品牌

支持上传：

- Logo；
- Favicon。

支持设置：

- Primary Color；
- Accent Color；
- Background Theme。

系统将自动应用于：

- Login；
- Dashboard；
- Button；
- Navigation；
- Quote；
- PI；
- PDF。

---

# 5.4 完整 White Label

系统设计原则：

下游客户访问 Tenant Portal 时：

**不出现平台开发商品牌。**

包括：

- 不出现开发商 Logo；
- 不出现 Powered by；
- 不出现开发商网站；
- 不出现开发商公司名称。

Tenant 的：

- Logo；
- Company Name；
- Colors；
- Domain；

将作为客户看到的品牌。

---

# 5.5 自定义独立域名

Tenant 默认可以拥有平台子域名，例如：

```text
company-a.portal.example.com
```

同时支持绑定自己的域名：

```text
portal.company-a.com
```

Tenant Admin 输入：

```text
portal.company-a.com
```

系统生成 DNS 配置提示。

例如：

```text
Type:

CNAME

Name:

portal

Target:

xxxxx.platform-domain.com
```

用户完成 DNS 配置以后，平台自动验证。

---

# 5.6 SSL 自动配置

Custom Domain 验证通过以后：

系统自动处理 HTTPS / SSL。

状态包括：

```text
Waiting DNS

Verifying

SSL Pending

Active

Failed
```

成功以后：

```text
https://portal.company-a.com
```

直接访问。

Tenant 不需要手动上传 SSL 证书。

---

# 5.7 员工管理

Tenant Admin 可以：

- 创建 Staff；
- 邀请 Staff；
- 禁用 Staff；
- 查看 Staff 状态。

Staff 使用自己的账号登录系统。

---

# 5.8 客户管理

Tenant Admin / Staff 可以：

- 新增 Client；
- 编辑 Client；
- 邀请 Client；
- 禁用 Client；
- 查看 Client 详情。

Client 信息：

- Name；
- Company；
- Email；
- Phone；
- Status；
- Notes。

---

# 5.9 文件管理

Tenant Admin / Staff 可以：

- 创建文件夹；
- 上传文件；
- 下载文件；
- 删除文件；
- 重命名；
- 将文件关联给 Client；
- 搜索文件。

Client 可以：

- 查看自己的文件；
- 下载自己的文件；
- 上传允许上传的文件。

---

# 5.10 文件安全

文件不能直接公开访问。

只有具备权限的用户才能：

```text
Authorization
      ↓
Temporary Secure URL
      ↓
Download
```

防止通过文件 URL 访问其他 Tenant 或 Client 文件。

---

# 5.11 简易工单

Client 可以：

- 创建工单；
- 查看工单；
- 回复；
- 上传附件。

Staff 可以：

- 查看工单；
- 回复；
- 分配负责人；
- 修改状态；
- 修改优先级；
- 关闭工单。

---

# 5.12 工单状态

包括：

```text
Open

In Progress

Waiting for Client

Resolved

Closed
```

Priority：

```text
Low

Normal

High

Urgent
```

---

# 5.13 Quote 报价单

Tenant Admin / Staff 可以创建 Quote。

包含：

- Quote Number；
- Customer；
- Date；
- Expiry Date；
- Product / Service；
- Description；
- Quantity；
- Unit Price；
- Tax；
- Discount；
- Total；
- Terms；
- Notes。

---

# 5.14 PI Proforma Invoice

支持创建：

**Proforma Invoice**

信息包括：

- PI Number；
- Customer；
- Date；
- Expiry Date；
- Items；
- Quantity；
- Price；
- Subtotal；
- Tax；
- Discount；
- Total；
- Payment Terms；
- Notes。

---

# 5.15 PDF 导出

Quote / PI 支持：

**Download PDF**

PDF 自动使用：

- Tenant Logo；
- Tenant Company Name；
- Tenant Address；
- Tenant Brand Color；
- Tenant Contact Information。

因此不同 Tenant 导出的文件拥有不同企业品牌。

---

# 六、订阅系统

平台订阅通过：

**Lemon Squeezy**

完成。

---

# 6.1 Subscription 开通流程

```text
Tenant Customer

      ↓

Lemon Squeezy Checkout

      ↓

Payment Successful

      ↓

Webhook

      ↓

Create Tenant

      ↓

Create Admin

      ↓

Activate Subscription

      ↓

Tenant Available
```

---

# 6.2 支持订阅生命周期

包括：

- Subscription Created；
- Subscription Updated；
- Payment Successful；
- Payment Failed；
- Subscription Cancelled；
- Subscription Resumed；
- Subscription Expired。

---

# 6.3 自动停用

当 Tenant Subscription 真正过期：

```text
ACTIVE
   ↓
EXPIRED
   ↓
SUSPENDED
```

Tenant 数据不会立即删除。

Tenant Admin 可以恢复订阅以后继续使用。

---

# 6.4 取消订阅

取消订阅以后：

不立即停止服务。

例如：

```text
用户已经支付到 10 月 1 日

9 月 15 日取消

9 月 15 日～10 月 1 日
仍然正常使用

10 月 1 日以后
Tenant Suspended
```

这样符合标准 SaaS Billing 生命周期。

---

# 七、自托管能力

本项目支持部署至客户自己的：

**AWS Account**

客户最终拥有：

- AWS；
- Domain；
- Server；
- Database；
- Storage；
- Source Code；
- Lemon Squeezy；
- Email Service。

项目完成以后不存在开发商服务器强依赖。

---

# 八、无远程 License 激活

系统不会设计：

```text
Remote License Server
```

不会存在：

```text
developer-server.com/license/check
```

才能运行的机制。

即：

- 无远程 License；
- 无 Call Home；
- 无远程激活；
- 无开发商后台控制程序运行。

---

# 九、推荐技术方案

## 前端

```text
Next.js

React

TypeScript

Tailwind CSS

shadcn/ui
```

---

# 后端

```text
NestJS

TypeScript

REST API
```

---

# 数据库

```text
PostgreSQL
```

采用：

```text
Tenant ID

+

Application Isolation

+

PostgreSQL Row Level Security
```

进行多租户数据隔离。

---

# 缓存 / Queue

```text
Redis
```

用于：

- Cache；
- Queue；
- Background Jobs。

---

# 文件

```text
AWS S3
```

---

# PDF

```text
HTML

+

Playwright / Chromium
```

动态生成 PDF。

---

# SaaS Billing

```text
Lemon Squeezy
```

---

# Deployment

```text
Docker
+
AWS
```

---

# 十、AWS 部署架构

推荐：

```text
                        Internet
                           │
                           ▼
                     CloudFront
                           │
                           ▼
                 Custom Domain / SSL
                           │
                           ▼
                   Application Layer
                           │
               ┌───────────┴───────────┐
               │                       │
             Web                       API
               │                       │
               └───────────┬───────────┘
                           │
            ┌──────────────┼─────────────┐
            │              │             │
            ▼              ▼             ▼
      PostgreSQL         Redis           S3
        RDS                              Files
```

---

# 十一、主要技术难点

# 11.1 多租户强隔离

最大风险：

Tenant A 通过修改请求参数访问 Tenant B 数据。

解决方式：

```text
Tenant Resolver

+

Backend Tenant Scope

+

Database Row Level Security

+

Cross Tenant Security Test
```

形成多层保护。

---

# 11.2 Custom Domain

问题：

系统未来可能存在几十、几百甚至更多 Tenant。

每个 Tenant 都可能使用自己的域名。

传统：

```text
Nginx
+
Certbot
```

人工维护模式不适合 SaaS。

解决：

使用 AWS CloudFront 多租户域名能力统一处理：

- Tenant Domain；
- DNS；
- TLS；
- Routing。

---

# 11.3 SSL 自动化

Tenant 不应该自己购买、上传和更新证书。

由平台统一负责：

```text
Domain Validation

↓

TLS Certificate

↓

Deployment

↓

Renewal
```

实现真正 SaaS 化。

---

# 11.4 White Label 完整性

不能只做到：

```text
换 Logo
```

而要覆盖：

- Login；
- Dashboard；
- Browser Title；
- Favicon；
- Email；
- Quote；
- PI；
- PDF；
- Error Page。

防止在某些页面出现平台开发商品牌。

---

# 11.5 Lemon Squeezy Webhook

Webhook 可能存在：

- 重复；
- 延迟；
- 乱序；
- 网络失败。

因此系统需要：

```text
Webhook Signature Verification

+

Webhook Log

+

Idempotency

+

Retry

+

Async Processing
```

防止：

- Tenant 重复创建；
- Subscription 重复处理；
- 状态异常。

---

# 11.6 文件权限

文件系统必须同时处理：

```text
Tenant Permission

+

Client Permission
```

例如：

Tenant A Client A：

只能访问：

```text
Tenant A
+
Client A Files
```

不能通过文件 ID 访问其他文件。

---

# 十二、安全设计

V1 将重点覆盖：

- Password Hash；
- Secure Cookie；
- JWT / Session；
- Rate Limit；
- Input Validation；
- Tenant Isolation；
- Role Authorization；
- File Permission；
- Webhook Signature；
- Audit Log；
- Secure Header；
- SQL Injection Protection；
- IDOR Protection。

---

# 十三、Audit Log

系统记录关键行为：

例如：

```text
Login

Invite User

Upload File

Delete File

Create Ticket

Update Ticket

Create Quote

Create PI

Add Domain

Verify Domain

Subscription Suspended
```

用于：

- 安全；
- 排查；
- 审计；
- 客户支持。

---

# 十四、V1 页面范围

## Platform Admin

- Login
- Dashboard
- Tenants
- Tenant Detail
- Subscriptions
- Domains
- Webhooks
- Logs
- Settings

---

## Tenant Admin

- Login
- Dashboard
- Clients
- Client Detail
- Files
- Tickets
- Ticket Detail
- Quotes
- Quote Editor
- PI
- PI Editor
- Team
- Branding
- Custom Domain
- Billing
- Profile

---

## Client

- Login
- Dashboard
- Files
- Tickets
- Ticket Detail
- Quotes
- PI
- Profile

---

# 十五、V1 不包含范围

为保证项目周期和预算，本阶段明确不包含：

- CRM；
- Sales Pipeline；
- Marketing Automation；
- Appointment；
- Calendar；
- SMS；
- AI；
- Native APP；
- Online Chat；
- Accounting；
- Electronic Signature；
- Workflow Builder；
- File Online Editor；
- Google Drive Integration；
- Dropbox Integration；
- Custom Permission Builder；
- Custom Email Sending Domain；
- 多语言；
- Online Invoice Payment；
- Recurring Invoice；
- Knowledge Base；
- Advanced BI。

以上内容如后续需要，可作为 Phase 2 独立扩展。

# 十六、第三方费用

以下费用不包含在开发报价内，由客户实际使用账户支付：

- AWS；
- Domain；
- Lemon Squeezy Transaction Fee；
- Email Sending；
- DNS；
- 其他第三方服务。

初期轻量使用情况下，AWS 可以采用成本较低的基础配置。

后续随客户量增长再扩容。
