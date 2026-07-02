# WebWap Knowledge Aggregator

WebWap Knowledge Aggregator 是一个面向移动端与桌面端双栖用户的轻量级技术知识索引系统，专注于从海量半结构化百科站点中提取、归类与展示高价值技术条目。项目定位于个人开发者、技术文档撰写者以及开源社区维护者，帮助其快速定位特定领域的术语解释、配置参数、故障排查步骤与版本变更记录。

该项目不提供全文抓取或持久化存储，而是通过构建统一的资源定位映射表，将分散于多个子域名下的动态百科页面聚合为可检索、可标签化的外链集合。用户可通过本项目提供的分类导航与关键词过滤，直接跳转至原始百科页面获取最新内容，既规避了内容过时问题，又降低了本地存储负担。

## 功能概览

**多级分类索引** 根据页面 URL 中的数字 ID 段自动识别内容大类，将条目归入网络协议、系统命令、编程语言、数据库中间件、安全审计等一级分类，并在每个分类下提供子标签过滤。

**动态资源快照** 对每个收录的百科页面记录其最后校验时间、响应状态码与内容摘要哈希值，便于用户判断链接有效性及内容是否发生变更。

**关键词全文检索** 基于页面标题与首段文本构建倒排索引，支持布尔运算符（AND、OR、NOT）与通配符匹配，检索响应时间控制在 300 毫秒以内。

**批量导入导出** 支持 CSV 与 JSON Lines 格式的资源列表批量导入，以及按分类、标签或时间范围导出链接集合，便于团队内部共享知识库。

**访问统计看板** 展示每个外链的点击次数、平均停留时长与跳出率，帮助维护者识别高频访问条目并优化分类排序。

**自定义标签系统** 用户可为任意条目添加私有标签，标签数据存储于本地浏览器缓存或可选的后端 Redis 实例中，实现个性化知识组织。

**失效链接检测** 每日定时任务通过 HEAD 请求验证收录 URL 的可达性，自动标记返回 4xx 或 5xx 状态的链接，并在看板中高亮提示。

**权限分级控制** 支持只读访客、标签编辑者、分类管理员与系统管理员四级权限，不同角色拥有不同的增删改查操作范围。

## 应用场景

技术文档编写过程中的术语交叉引用。当撰写涉及多个组件（如 Nginx + PHP-FPM + MySQL）的部署手册时，作者可通过本项目快速检索每个组件的核心配置参数释义，并将对应的百科链接作为脚注附录，提升文档的权威性与可验证性。

开源项目 issue 讨论中的背景知识补充。社区维护者在处理用户提交的 bug 报告时，经常需要引用底层系统调用或库函数的行为说明。本项目提供的外链集合可帮助维护者迅速找到官方或半官方解释，减少重复解释成本。

技术面试题库的扩展阅读构建。面试官或题库维护者可将本项目收录的百科条目作为面试题目的参考来源，为每个知识点附加外部链接，使候选人在准备过程中能够获得更立体的学习路径。

个人技术博客的参考文献管理。博客作者在撰写技术分析文章时，可利用本项目的分类导航和检索功能快速收集相关背景资料，统一整理为文章末尾的引用链接列表，提高写作效率。

## 快速开始

以下指令适用于 Linux / macOS / Windows WSL 环境，需提前安装 Git、Node.js 18.x 与 pnpm。

```bash
# 克隆仓库
git clone https://github.com/your-org/webwap-knowledge-aggregator.git
cd webwap-knowledge-aggregator

# 安装依赖
pnpm install

# 启动开发服务器（默认占用端口 5173）
pnpm run dev

# 生产环境构建
pnpm run build

# 启动生产预览
pnpm run preview
```

首次启动后，系统会在 `data/` 目录下生成 `links.json` 初始文件，其中包含项目自带的 180 条百科资源链接。用户可通过管理界面手动刷新该文件，或通过 `pnpm run import` 命令导入自定义数据源。

## 安装要求

| 依赖 | 必需 | 说明 |
|------|------|------|
| Node.js 18.0.0 或更高 | 是 | 运行时环境，需支持 ES Modules 与 Fetch API |
| pnpm 8.0.0 或更高 | 是 | 包管理器，用于依赖安装与工作区管理 |
| SQLite 3.35.0 或更高 | 是 | 本地索引数据库，存储分类、标签与访问统计 |
| Redis 7.0.0 或更高 | 否 | 可选缓存层，用于多实例部署时的标签同步与限流 |
| Nginx 1.20.0 或更高 | 否 | 生产环境反向代理，推荐用于静态文件服务与负载均衡 |
| 磁盘可用空间 ≥ 500 MB | 是 | 用于存放 SQLite 索引文件、日志及临时缓存 |
| 内存 ≥ 512 MB | 是 | 开发模式建议 1 GB，生产模式建议 2 GB |
| 网络出口允许出站 HTTP/HTTPS | 是 | 用于每日失效链接检测任务发送 HEAD 请求 |

## 文档导航

| 层面 | 目录 | 回答的问题 |
|------|------|------------|
| 用户手册 | `/docs/user-guide/` | 如何注册账号、导入链接、创建标签、查看统计看板 |
| 管理员指南 | `/docs/admin-guide/` | 如何配置失效检测间隔、调整权限分级、备份 SQLite 数据 |
| API 参考 | `/docs/api-reference/` | 检索端点、批量导入端点、标签管理端点的请求与响应格式 |
| 架构设计 | `/docs/architecture/` | 索引更新策略、缓存失效机制、多实例部署时的并发控制方案 |
| 故障排查 | `/docs/troubleshooting/` | 常见启动错误、数据库锁冲突、代理环境下的出站请求超时处理 |
| 贡献规范 | `/docs/contributing/` | 代码风格检查、提交信息格式、测试用例编写要求 |

## 资源列表

本项目初始收录的百科资源链接按数据来源归类。所有链接均来自 `wap.baike.kmvdvi.cn` 域名，每条链接对应一篇独立的技术或常识条目。以下列表为项目内置的 180 条原始数据，导入后系统将自动解析其 ID 段并生成分类索引。

百科条目链接

http://wap.baike.kmvdvi.cn/Article/details/7862056.sHtML
http://wap.baike.kmvdvi.cn/Article/details/429298.sHtML
http://wap.baike.kmvdvi.cn/Article/details/4795793.sHtML
http://wap.baike.kmvdvi.cn/Article/details/0237006.sHtML
http://wap.baike.kmvdvi.cn/Article/details/0751.sHtML
http://wap.baike.kmvdvi.cn/Article/details/2370.sHtML
http://wap.baike.kmvdvi.cn/Article/details/989008.sHtML
http://wap.baike.kmvdvi.cn/Article/details/529682.sHtML
http://wap.baike.kmvdvi.cn/Article/details/40245.sHtML
http://wap.baike.kmvdvi.cn/Article/details/2693.sHtML
http://wap.baike.kmvdvi.cn/Article/details/7996338.sHtML
http://wap.baike.kmvdvi.cn/Article/details/4541.sHtML
http://wap.baike.kmvdvi.cn/Article/details/1183777.sHtML
http://wap.baike.kmvdvi.cn/Article/details/55504.sHtML
http://wap.baike.kmvdvi.cn/Article/details/89900.sHtML
http://wap.baike.kmvdvi.cn/Article/details/2702.sHtML
http://wap.baike.kmvdvi.cn/Article/details/814100.sHtML
http://wap.baike.kmvdvi.cn/Article/details/9588065.sHtML
http://wap.baike.kmvdvi.cn/Article/details/3451641.sHtML
http://wap.baike.kmvdvi.cn/Article/details/87986.sHtML
http://wap.baike.kmvdvi.cn/Article/details/8887.sHtML
http://wap.baike.kmvdvi.cn/Article/details/3329.sHtML
http://wap.baike.kmvdvi.cn/Article/details/4960986.sHtML
http://wap.baike.kmvdvi.cn/Article/details/5542.sHtML
http://wap.baike.kmvdvi.cn/Article/details/94644.sHtML
http://wap.baike.kmvdvi.cn/Article/details/9336.sHtML
http://wap.baike.kmvdvi.cn/Article/details/685093.sHtML
http://wap.baike.kmvdvi.cn/Article/details/44961.sHtML
http://wap.baike.kmvdvi.cn/Article/details/4853247.sHtML
http://wap.baike.kmvdvi.cn/Article/details/8274015.sHtML
http://wap.baike.kmvdvi.cn/Article/details/488304.sHtML
http://wap.baike.kmvdvi.cn/Article/details/781518.sHtML
http://wap.baike.kmvdvi.cn/Article/details/79541.sHtML
http://wap.baike.kmvdvi.cn/Article/details/9568367.sHtML
http://wap.baike.kmvdvi.cn/Article/details/2430855.sHtML
http://wap.baike.kmvdvi.cn/Article/details/31716.sHtML
http://wap.baike.kmvdvi.cn/Article/details/1592204.sHtML
http://wap.baike.kmvdvi.cn/Article/details/60168.sHtML
http://wap.baike.kmvdvi.cn/Article/details/13147.sHtML
http://wap.baike.kmvdvi.cn/Article/details/09815.sHtML
http://wap.baike.kmvdvi.cn/Article/details/4983702.sHtML
http://wap.baike.kmvdvi.cn/Article/details/8253764.sHtML
http://wap.baike.kmvdvi.cn/Article/details/745625.sHtML
http://wap.baike.kmvdvi.cn/Article/details/4881.sHtML
http://wap.baike.kmvdvi.cn/Article/details/42839.sHtML
http://wap.baike.kmvdvi.cn/Article/details/93389.sHtML
http://wap.baike.kmvdvi.cn/Article/details/27370.sHtML
http://wap.baike.kmvdvi.cn/Article/details/03400.sHtML
http://wap.baike.kmvdvi.cn/Article/details/150710.sHtML
http://wap.baike.kmvdvi.cn/Article/details/1911789.sHtML
http://wap.baike.kmvdvi.cn/Article/details/192196.sHtML
http://wap.baike.kmvdvi.cn/Article/details/35373.sHtML
http://wap.baike.kmvdvi.cn/Article/details/809603.sHtML
http://wap.baike.kmvdvi.cn/Article/details/25972.sHtML
http://wap.baike.kmvdvi.cn/Article/details/2134342.sHtML
http://wap.baike.kmvdvi.cn/Article/details/5314025.sHtML
http://wap.baike.kmvdvi.cn/Article/details/359652.sHtML
http://wap.baike.kmvdvi.cn/Article/details/6225277.sHtML
http://wap.baike.kmvdvi.cn/Article/details/0043200.sHtML
http://wap.baike.kmvdvi.cn/Article/details/7954720.sHtML
http://wap.baike.kmvdvi.cn/Article/details/06398.sHtML
http://wap.baike.kmvdvi.cn/Article/details/237338.sHtML
http://wap.baike.kmvdvi.cn/Article/details/8225370.sHtML
http://wap.baike.kmvdvi.cn/Article/details/5816774.sHtML
http://wap.baike.kmvdvi.cn/Article/details/63602.sHtML
http://wap.baike.kmvdvi.cn/Article/details/3164148.sHtML
http://wap.baike.kmvdvi.cn/Article/details/4800639.sHtML
http://wap.baike.kmvdvi.cn/Article/details/41399.sHtML
http://wap.baike.kmvdvi.cn/Article/details/13604.sHtML
http://wap.baike.kmvdvi.cn/Article/details/1063648.sHtML
http://wap.baike.kmvdvi.cn/Article/details/3411811.sHtML
http://wap.baike.kmvdvi.cn/Article/details/911775.sHtML
http://wap.baike.kmvdvi.cn/Article/details/4338031.sHtML
http://wap.baike.kmvdvi.cn/Article/details/8992.sHtML
http://wap.baike.kmvdvi.cn/Article/details/5480257.sHtML
http://wap.baike.kmvdvi.cn/Article/details/776551.sHtML
http://wap.baike.kmvdvi.cn/Article/details/187637.sHtML
http://wap.baike.kmvdvi.cn/Article/details/6817.sHtML
http://wap.baike.kmvdvi.cn/Article/details/161001.sHtML
http://wap.baike.kmvdvi.cn/Article/details/367421.sHtML
http://wap.baike.kmvdvi.cn/Article/details/596412.sHtML
http://wap.baike.kmvdvi.cn/Article/details/451533.sHtML
http://wap.baike.kmvdvi.cn/Article/details/8239986.sHtML
http://wap.baike.kmvdvi.cn/Article/details/98329.sHtML
http://wap.baike.kmvdvi.cn/Article/details/70035.sHtML
http://wap.baike.kmvdvi.cn/Article/details/569249.sHtML
http://wap.baike.kmvdvi.cn/Article/details/392167.sHtML
http://wap.baike.kmvdvi.cn/Article/details/7441.sHtML
http://wap.baike.kmvdvi.cn/Article/details/6434.sHtML
http://wap.baike.kmvdvi.cn/Article/details/684713.sHtML
http://wap.baike.kmvdvi.cn/Article/details/158314.sHtML
http://wap.baike.kmvdvi.cn/Article/details/989751.sHtML
http://wap.baike.kmvdvi.cn/Article/details/74965.sHtML
http://wap.baike.kmvdvi.cn/Article/details/5716.sHtML
http://wap.baike.kmvdvi.cn/Article/details/4672621.sHtML
http://wap.baike.kmvdvi.cn/Article/details/9840.sHtML
http://wap.baike.kmvdvi.cn/Article/details/1243.sHtML
http://wap.baike.kmvdvi.cn/Article/details/584464.sHtML
http://wap.baike.kmvdvi.cn/Article/details/395021.sHtML
http://wap.baike.kmvdvi.cn/Article/details/3843994.sHtML
http://wap.baike.kmvdvi.cn/Article/details/017467.sHtML
http://wap.baike.kmvdvi.cn/Article/details/9320.sHtML
http://wap.baike.kmvdvi.cn/Article/details/1676.sHtML
http://wap.baike.kmvdvi.cn/Article/details/9731.sHtML
http://wap.baike.kmvdvi.cn/Article/details/8548658.sHtML
http://wap.baike.kmvdvi.cn/Article/details/03037.sHtML
http://wap.baike.kmvdvi.cn/Article/details/700458.sHtML
http://wap.baike.kmvdvi.cn/Article/details/01020.sHtML
http://wap.baike.kmvdvi.cn/Article/details/679978.sHtML
http://wap.baike.kmvdvi.cn/Article/details/7112.sHtML
http://wap.baike.kmvdvi.cn/Article/details/888723.sHtML
http://wap.baike.kmvdvi.cn/Article/details/880307.sHtML
http://wap.baike.kmvdvi.cn/Article/details/1849.sHtML
http://wap.baike.kmvdvi.cn/Article/details/6565.sHtML
http://wap.baike.kmvdvi.cn/Article/details/5039.sHtML
http://wap.baike.kmvdvi.cn/Article/details/02364.sHtML
http://wap.baike.kmvdvi.cn/Article/details/64481.sHtML
http://wap.baike.kmvdvi.cn/Article/details/3313330.sHtML
http://wap.baike.kmvdvi.cn/Article/details/5201.sHtML
http://wap.baike.kmvdvi.cn/Article/details/9457367.sHtML
http://wap.baike.kmvdvi.cn/Article/details/52590.sHtML
http://wap.baike.kmvdvi.cn/Article/details/78289.sHtML
http://wap.baike.kmvdvi.cn/Article/details/0282363.sHtML
http://wap.baike.kmvdvi.cn/Article/details/13022.sHtML
http://wap.baike.kmvdvi.cn/Article/details/4647609.sHtML
http://wap.baike.kmvdvi.cn/Article/details/8713070.sHtML
http://wap.baike.kmvdvi.cn/Article/details/61079.sHtML
http://wap.baike.kmvdvi.cn/Article/details/72438.sHtML
http://wap.baike.kmvdvi.cn/Article/details/778172.sHtML
http://wap.baike.kmvdvi.cn/Article/details/22755.sHtML
http://wap.baike.kmvdvi.cn/Article/details/6271.sHtML
http://wap.baike.kmvdvi.cn/Article/details/6700.sHtML
http://wap.baike.kmvdvi.cn/Article/details/232828.sHtML
http://wap.baike.kmvdvi.cn/Article/details/235166.sHtML
http://wap.baike.kmvdvi.cn/Article/details/53168.sHtML
http://wap.baike.kmvdvi.cn/Article/details/133075.sHtML
http://wap.baike.kmvdvi.cn/Article/details/3616474.sHtML
http://wap.baike.kmvdvi.cn/Article/details/3946662.sHtML
http://wap.baike.kmvdvi.cn/Article/details/8557.sHtML
http://wap.baike.kmvdvi.cn/Article/details/3049818.sHtML
http://wap.baike.kmvdvi.cn/Article/details/402479.sHtML
http://wap.baike.kmvdvi.cn/Article/details/300425.sHtML
http://wap.baike.kmvdvi.cn/Article/details/9121844.sHtML
http://wap.baike.kmvdvi.cn/Article/details/49254.sHtML
http://wap.baike.kmvdvi.cn/Article/details/617298.sHtML
http://wap.baike.kmvdvi.cn/Article/details/639777.sHtML
http://wap.baike.kmvdvi.cn/Article/details/52007.sHtML
http://wap.baike.kmvdvi.cn/Article/details/21013.sHtML
http://wap.baike.kmvdvi.cn/Article/details/515333.sHtML
http://wap.baike.kmvdvi.cn/Article/details/2839657.sHtML
http://wap.baike.kmvdvi.cn/Article/details/36026.sHtML
http://wap.baike.kmvdvi.cn/Article/details/462999.sHtML
http://wap.baike.kmvdvi.cn/Article/details/34840.sHtML
http://wap.baike.kmvdvi.cn/Article/details/438131.sHtML
http://wap.baike.kmvdvi.cn/Article/details/607486.sHtML
http://wap.baike.kmvdvi.cn/Article/details/3052206.sHtML
http://wap.baike.kmvdvi.cn/Article/details/8589.sHtML
http://wap.baike.kmvdvi.cn/Article/details/4065.sHtML
http://wap.baike.kmvdvi.cn/Article/details/667814.sHtML
http://wap.baike.kmvdvi.cn/Article/details/4359.sHtML
http://wap.baike.kmvdvi.cn/Article/details/805771.sHtML
http://wap.baike.kmvdvi.cn/Article/details/2030831.sHtML
http://wap.baike.kmvdvi.cn/Article/details/63247.sHtML
http://wap.baike.kmvdvi.cn/Article/details/49884.sHtML
http://wap.baike.kmvdvi.cn/Article/details/68369.sHtML
http://wap.baike.kmvdvi.cn/Article/details/3613.sHtML
http://wap.baike.kmvdvi.cn/Article/details/9856.sHtML
http://wap.baike.kmvdvi.cn/Article/details/509513.sHtML
http://wap.baike.kmvdvi.cn/Article/details/2911418.sHtML
http://wap.baike.kmvdvi.cn/Article/details/252731.sHtML
http://wap.baike.kmvdvi.cn/Article/details/9650217.sHtML
http://wap.baike.kmvdvi.cn/Article/details/45109.sHtML
http://wap.baike.kmvdvi.cn/Article/details/4882978.sHtML
http://wap.baike.kmvdvi.cn/Article/details/72755.sHtML
http://wap.baike.kmvdvi.cn/Article/details/06431.sHtML
http://wap.baike.kmvdvi.cn/Article/details/8735667.sHtML
http://wap.baike.kmvdvi.cn/Article/details/64226.sHtML
http://wap.baike.kmvdvi.cn/Article/details/4928.sHtML
http://wap.baike.kmvdvi.cn/Article/details/44173.sHtML
http://wap.baike.kmvdvi.cn/Article/details/997956.sHtML

## 项目结构

```
webwap-knowledge-aggregator/
├── src/                           # 核心源代码目录
│   ├── core/                      # 索引引擎与数据模型
│   │   ├── LinkRegistry.ts        # 链接注册表，管理增删改查与 ID 解析
│   │   ├── ClassifierEngine.ts    # 分类器，基于 ID 段和标题关键词生成分类
│   │   └── HealthChecker.ts       # 失效检测定时任务，支持 HEAD 请求与重试策略
│   ├── api/                       # HTTP API 路由层
│   │   ├── search.ts              # 全文检索端点，支持布尔查询与分页
│   │   ├── import.ts              # 批量导入端点，接受 CSV / JSONL 格式
│   │   └── stats.ts               # 访问统计与健康状态汇总端点
│   ├── ui/                        # 前端控制台组件
│   │   ├── Dashboard.tsx          # 看板主布局，含分类树与统计卡片
│   │   ├── LinkTable.tsx          # 链接列表表格，支持排序与标签编辑
│   │   └── TagManager.tsx         # 私有标签管理界面，支持批量打标
│   └── workers/                   # 后台工作进程
│       ├── scheduler.ts           # 定时任务调度器，基于 node-cron
│       └── fetch-queue.ts         # 出站请求队列，控制并发与超时
├── data/                          # 数据存储目录
│   ├── links.json                 # 初始内置链接集合，共 180 条
│   ├── index.db                   # SQLite 索引数据库，含分类、标签、统计表
│   └── cache/                     # Redis 缓存落地文件（仅本地开发模式）
├── docs/                          # 文档目录
│   ├── user-guide/                # 用户手册，含截图与操作视频链接
│   ├── admin-guide/               # 部署与运维指南，含 systemd 模板
│   └── api-reference/             # OpenAPI 3.0 规范文件
├── tests/                         # 单元测试与集成测试
│   ├── unit/                      # 分类器与注册表的单元测试
│   └── integration/               # API 端到端测试，依赖 SQLite 内存模式
├── scripts/                       # 工具脚本
│   ├── import.js                  # 命令行导入工具，支持自定义数据源
│   └── migrate-db.js              # 数据库迁移脚本，处理版本升级
├── .env.example                   # 环境变量模板，含端口、Redis URL、调度间隔
├── pnpm-workspace.yaml            # pnpm 工作区配置，预留微服务扩展
├── package.json                   # 项目清单，含依赖与脚本命令
└── README.md                      # 本文件
```

## 贡献指南

提交问题报告或功能请求。请使用 GitHub Issues 模板，在标题中注明 `[Bug]` 或 `[Feature]` 前缀，并附上重现步骤、预期结果与实际结果。对于失效链接检测问题，请提供具体的 URL 及返回的 HTTP 状态码。

添加新的百科资源条目。通过管理界面的「批量导入」功能或 `/api/import` 端点提交 CSV 文件，CSV 须包含 `url` 与 `title` 两列。系统会自动校验 URL 格式并去重，重复提交的条目将被忽略。

改进分类器规则。分类器引擎的规则文件位于 `src/core/ClassifierEngine.ts` 中的 `RULE_MAP` 常量，贡献者可按照现有模式添加新的 ID 段范围与分类名称的映射关系，并提交单元测试用例覆盖新增规则。

完善文档或翻译。文档采用 Markdown 编写，存放在 `docs/` 目录下。翻译工作需先创建对应语言子目录（如 `docs/zh-CN/`），并保持与英文版相同的目录结构。提交前请运行 `pnpm run lint:docs` 检查链接有效性。

## 常见问题

Q: 启动开发服务器时提示 `SQLITE_ERROR: no such table: links`，如何解决？

A: 这是由于首次启动时数据库迁移未自动执行。请先运行 `pnpm run migrate` 手动创建表结构，然后执行 `pnpm run seed` 导入初始 180 条链接数据。若问题依然存在，检查 `data/` 目录是否有写入权限，并删除 `index.db` 文件后重试迁移。

Q: 失效链接检测任务是否会对外部站点造成过大请求压力？

A: 系统默认将并发请求数限制为 5，且每个 URL 的 HEAD 请求超时设置为 3 秒。同时，检测任务每日仅执行一次（默认凌晨 3 点），且对同一域名的请求间隔不低于 200 毫秒。这些参数均可在 `.env` 文件中调整。如果外部站点返回 429 状态码，系统会自动将该链接加入冷却名单，24 小时内不再检测。

Q: 能否在不使用 Redis 的情况下运行多实例部署？

A: 可以。当 `REDIS_URL` 环境变量未设置时，系统自动降级为内存缓存模式，但此时多实例之间的标签数据无法同步，且定时任务可能被多个实例同时触发。对于生产环境的多实例部署，强烈建议配置 Redis 7.0 及以上版本，并设置 `SCHEDULER_LOCK_KEY` 以启用分布式锁。

## 许可证

MIT

> 外链数量: 180 | 生成时间: 2026-07-02 22:13:34
