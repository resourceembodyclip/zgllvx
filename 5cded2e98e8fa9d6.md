# WapWiki Link Aggregator

WapWiki Link Aggregator 是一个面向技术文档整理与知识库建设的外链资源归集系统，专注于对 wap.baike.kmvdvi.cn 域名下的结构化百科文章进行批量索引、分类存储与检索支持。本项目不对原始内容做修改或重写，仅提供链接的抓取、校验、分类与展示能力，适用于需要快速建立垂直领域知识导航站点的个人开发者、内容运营团队及学术研究机构。

项目定位为轻量级外链聚合中间件，核心目标用户包括：需要维护大量参考链接的技术博客作者、搭建行业资源门户的站长、以及进行网络内容结构分析的数据处理工程师。通过统一的数据入口和标准化的输出格式，本项目帮助用户从大量分散的百科条目中快速定位所需信息，减少重复检索成本，提升知识管理效率。

## 功能概览

**批量链接导入** - 支持通过文本文件或标准输入流一次性导入大量 URL 列表，自动解析 URL 结构并提取文章标识符。

**自动分类标记** - 根据 URL 路径中的关键词和数字段特征，对链接进行初步的主题类别推断，生成分类标签供后续筛选使用。

**去重与校验** - 内置链接去重引擎，对重复提交的 URL 自动忽略；同时提供 HTTP 状态码检查，标记失效或重定向的链接。

**结构化存储** - 将所有链接及元数据（导入时间、分类标签、状态码、最后检查时间）存入 SQLite 数据库，支持多条件组合查询。

**导出与集成** - 支持将链接列表导出为 JSON、CSV 和纯文本格式，便于与其他文档生成工具或静态站点生成器集成。

**增量更新** - 支持定时增量扫描，仅对新添加或状态变更的链接进行处理，避免全量重建带来的性能开销。

**查询过滤** - 提供命令行交互式查询界面，支持按类别、状态码、导入批次等条件过滤并输出结果。

**批次追踪** - 记录每批导入的链接数量、时间戳和批次编号（如第 30/56 批），便于大规模资源管理时追踪来源。

## 应用场景

**技术博客参考链接库** - 技术作者在撰写文章时，可将本项目作为参考链接的后台管理系统，将引用的百科条目统一录入，生成文章末尾的参考资料清单，避免手动整理遗漏。

**行业知识导航站建设** - 行业门户网站运营者可使用本项目对特定领域的百科条目进行归类汇总，生成按主题划分的导航页面，为访客提供结构化的信息入口。

**网络内容结构分析** - 数据分析师可导入大量百科链接，通过项目导出的结构化数据（含分类标签和时间戳）进行内容分布趋势分析，识别热门主题与空白领域。

**文档自动化生成流水线** - 团队可将本项目集成到持续集成流水线中，在构建文档站点时自动拉取最新链接列表并生成 Markdown 格式的索引文件，保持文档引用实时更新。

**个人知识库外链管理** - 研究人员可每日批量导入新发现的百科条目，通过项目的查询过滤功能快速检索特定主题下已收录的文章，辅助文献综述与笔记整理。

## 快速开始

以下步骤可在 Linux/macOS/WSL 环境下完成本项目的快速部署与运行。

```bash
# 克隆仓库
git clone https://github.com/example/wapwiki-link-aggregator.git
cd wapwiki-link-aggregator

# 安装依赖（使用 Python 虚拟环境）
python3 -m venv venv
source venv/bin/activate
pip install -r requirements.txt

# 初始化数据库并运行导入示例（含第 30/56 批链接）
python cli.py import --batch 30/56 --file samples/links_batch_30.txt
python cli.py serve --port 8080
```

执行上述命令后，本地 Web 管理界面将在 http://localhost:8080 启动，可通过浏览器访问并管理已导入的链接资源。

## 安装要求

| 依赖组件 | 必需版本 | 说明 |
|---|---|---|
| Python | 3.8 及以上 | 核心运行环境，提供解释器与标准库支持 |
| SQLite | 3.31 及以上 | 嵌入式数据库，用于链接元数据的持久化存储 |
| requests | 2.25.0 及以上 | HTTP 客户端库，用于链接状态校验与内容类型检测 |
| click | 8.0.0 及以上 | 命令行界面框架，用于解析子命令与参数 |
| jinja2 | 3.0.0 及以上 | 模板引擎，用于生成 Web 管理界面的 HTML 页面 |
| pytest | 7.0.0 及以上 | 单元测试框架，用于运行测试套件（仅开发环境必需） |
| black | 22.0.0 及以上 | 代码格式化工具（仅开发环境必需） |
| flake8 | 5.0.0 及以上 | 代码风格检查工具（仅开发环境必需） |

## 文档导航

| 层面 | 目录 | 回答的问题 |
|---|---|---|
| 入门指南 | docs/getting-started.md | 如何安装、配置并首次运行本项目？如何导入第一批链接？ |
| 命令行参考 | docs/cli-commands.md | 所有可用的子命令、参数选项及使用示例有哪些？ |
| 数据模型 | docs/data-model.md | 链接在数据库中如何存储？每个字段的含义和约束是什么？ |
| API 接口 | docs/api-endpoints.md | Web 管理界面后端提供了哪些 RESTful 接口？如何通过 API 进行增删改查？ |
| 部署指南 | docs/deployment.md | 如何将本项目部署到生产环境（如使用 Gunicorn + Nginx）？ |
| 批次管理 | docs/batch-management.md | 批次编号规则是什么？如何管理和切换不同批次的数据？ |

## 资源列表

本节列出本项目第 30/56 批所收录的全部外链资源。所有链接按原始格式原样呈现，未做任何协议补全、域名改写或路径规范化处理。

百科文章主条目

http://wap.baike.kmvdvi.cn/Article/details/454378.sHtML
http://wap.baike.kmvdvi.cn/Article/details/15880.sHtML
http://wap.baike.kmvdvi.cn/Article/details/813383.sHtML
http://wap.baike.kmvdvi.cn/Article/details/790529.sHtML
http://wap.baike.kmvdvi.cn/Article/details/0739473.sHtML
http://wap.baike.kmvdvi.cn/Article/details/4195200.sHtML
http://wap.baike.kmvdvi.cn/Article/details/3274.sHtML
http://wap.baike.kmvdvi.cn/Article/details/0559815.sHtML
http://wap.baike.kmvdvi.cn/Article/details/4719.sHtML
http://wap.baike.kmvdvi.cn/Article/details/68328.sHtML
http://wap.baike.kmvdvi.cn/Article/details/0992.sHtML
http://wap.baike.kmvdvi.cn/Article/details/2859.sHtML
http://wap.baike.kmvdvi.cn/Article/details/4974698.sHtML
http://wap.baike.kmvdvi.cn/Article/details/1540883.sHtML
http://wap.baike.kmvdvi.cn/Article/details/95726.sHtML
http://wap.baike.kmvdvi.cn/Article/details/0554862.sHtML
http://wap.baike.kmvdvi.cn/Article/details/5990.sHtML
http://wap.baike.kmvdvi.cn/Article/details/47323.sHtML
http://wap.baike.kmvdvi.cn/Article/details/1023703.sHtML
http://wap.baike.kmvdvi.cn/Article/details/5410.sHtML
http://wap.baike.kmvdvi.cn/Article/details/580185.sHtML
http://wap.baike.kmvdvi.cn/Article/details/805241.sHtML
http://wap.baike.kmvdvi.cn/Article/details/6970.sHtML
http://wap.baike.kmvdvi.cn/Article/details/16406.sHtML
http://wap.baike.kmvdvi.cn/Article/details/27095.sHtML
http://wap.baike.kmvdvi.cn/Article/details/542020.sHtML
http://wap.baike.kmvdvi.cn/Article/details/6352846.sHtML
http://wap.baike.kmvdvi.cn/Article/details/2168968.sHtML
http://wap.baike.kmvdvi.cn/Article/details/558606.sHtML
http://wap.baike.kmvdvi.cn/Article/details/4796722.sHtML
http://wap.baike.kmvdvi.cn/Article/details/56391.sHtML
http://wap.baike.kmvdvi.cn/Article/details/779905.sHtML
http://wap.baike.kmvdvi.cn/Article/details/333141.sHtML
http://wap.baike.kmvdvi.cn/Article/details/52128.sHtML
http://wap.baike.kmvdvi.cn/Article/details/09789.sHtML
http://wap.baike.kmvdvi.cn/Article/details/7928.sHtML
http://wap.baike.kmvdvi.cn/Article/details/16606.sHtML
http://wap.baike.kmvdvi.cn/Article/details/4920215.sHtML
http://wap.baike.kmvdvi.cn/Article/details/488232.sHtML
http://wap.baike.kmvdvi.cn/Article/details/3975314.sHtML
http://wap.baike.kmvdvi.cn/Article/details/5788687.sHtML
http://wap.baike.kmvdvi.cn/Article/details/55672.sHtML
http://wap.baike.kmvdvi.cn/Article/details/5106.sHtML
http://wap.baike.kmvdvi.cn/Article/details/54008.sHtML
http://wap.baike.kmvdvi.cn/Article/details/707077.sHtML
http://wap.baike.kmvdvi.cn/Article/details/8465724.sHtML
http://wap.baike.kmvdvi.cn/Article/details/54863.sHtML
http://wap.baike.kmvdvi.cn/Article/details/0426.sHtML
http://wap.baike.kmvdvi.cn/Article/details/632236.sHtML
http://wap.baike.kmvdvi.cn/Article/details/4485417.sHtML
http://wap.baike.kmvdvi.cn/Article/details/359158.sHtML
http://wap.baike.kmvdvi.cn/Article/details/970890.sHtML
http://wap.baike.kmvdvi.cn/Article/details/59908.sHtML
http://wap.baike.kmvdvi.cn/Article/details/2472655.sHtML
http://wap.baike.kmvdvi.cn/Article/details/7735975.sHtML
http://wap.baike.kmvdvi.cn/Article/details/363490.sHtML
http://wap.baike.kmvdvi.cn/Article/details/4317812.sHtML
http://wap.baike.kmvdvi.cn/Article/details/268418.sHtML
http://wap.baike.kmvdvi.cn/Article/details/7580615.sHtML
http://wap.baike.kmvdvi.cn/Article/details/8459887.sHtML
http://wap.baike.kmvdvi.cn/Article/details/798686.sHtML
http://wap.baike.kmvdvi.cn/Article/details/0231919.sHtML
http://wap.baike.kmvdvi.cn/Article/details/032364.sHtML
http://wap.baike.kmvdvi.cn/Article/details/190488.sHtML
http://wap.baike.kmvdvi.cn/Article/details/094524.sHtML
http://wap.baike.kmvdvi.cn/Article/details/7728616.sHtML
http://wap.baike.kmvdvi.cn/Article/details/50675.sHtML
http://wap.baike.kmvdvi.cn/Article/details/0626.sHtML
http://wap.baike.kmvdvi.cn/Article/details/779141.sHtML
http://wap.baike.kmvdvi.cn/Article/details/98125.sHtML
http://wap.baike.kmvdvi.cn/Article/details/46911.sHtML
http://wap.baike.kmvdvi.cn/Article/details/15693.sHtML
http://wap.baike.kmvdvi.cn/Article/details/0278.sHtML
http://wap.baike.kmvdvi.cn/Article/details/55084.sHtML
http://wap.baike.kmvdvi.cn/Article/details/8796.sHtML
http://wap.baike.kmvdvi.cn/Article/details/42066.sHtML
http://wap.baike.kmvdvi.cn/Article/details/723686.sHtML
http://wap.baike.kmvdvi.cn/Article/details/72123.sHtML
http://wap.baike.kmvdvi.cn/Article/details/6462594.sHtML
http://wap.baike.kmvdvi.cn/Article/details/34672.sHtML
http://wap.baike.kmvdvi.cn/Article/details/2487.sHtML
http://wap.baike.kmvdvi.cn/Article/details/242065.sHtML
http://wap.baike.kmvdvi.cn/Article/details/0564783.sHtML
http://wap.baike.kmvdvi.cn/Article/details/801764.sHtML
http://wap.baike.kmvdvi.cn/Article/details/852216.sHtML
http://wap.baike.kmvdvi.cn/Article/details/10386.sHtML
http://wap.baike.kmvdvi.cn/Article/details/2080.sHtML
http://wap.baike.kmvdvi.cn/Article/details/3255.sHtML
http://wap.baike.kmvdvi.cn/Article/details/2964.sHtML
http://wap.baike.kmvdvi.cn/Article/details/061632.sHtML
http://wap.baike.kmvdvi.cn/Article/details/1895.sHtML
http://wap.baike.kmvdvi.cn/Article/details/185213.sHtML
http://wap.baike.kmvdvi.cn/Article/details/24938.sHtML
http://wap.baike.kmvdvi.cn/Article/details/37096.sHtML
http://wap.baike.kmvdvi.cn/Article/details/5326979.sHtML
http://wap.baike.kmvdvi.cn/Article/details/455635.sHtML
http://wap.baike.kmvdvi.cn/Article/details/76087.sHtML
http://wap.baike.kmvdvi.cn/Article/details/26928.sHtML
http://wap.baike.kmvdvi.cn/Article/details/4350837.sHtML
http://wap.baike.kmvdvi.cn/Article/details/9692.sHtML
http://wap.baike.kmvdvi.cn/Article/details/0988.sHtML
http://wap.baike.kmvdvi.cn/Article/details/0758.sHtML
http://wap.baike.kmvdvi.cn/Article/details/1842.sHtML
http://wap.baike.kmvdvi.cn/Article/details/5972906.sHtML
http://wap.baike.kmvdvi.cn/Article/details/3247.sHtML
http://wap.baike.kmvdvi.cn/Article/details/7138.sHtML
http://wap.baike.kmvdvi.cn/Article/details/972381.sHtML
http://wap.baike.kmvdvi.cn/Article/details/0817134.sHtML
http://wap.baike.kmvdvi.cn/Article/details/4418379.sHtML
http://wap.baike.kmvdvi.cn/Article/details/41081.sHtML
http://wap.baike.kmvdvi.cn/Article/details/9842.sHtML
http://wap.baike.kmvdvi.cn/Article/details/327338.sHtML
http://wap.baike.kmvdvi.cn/Article/details/41069.sHtML
http://wap.baike.kmvdvi.cn/Article/details/165094.sHtML
http://wap.baike.kmvdvi.cn/Article/details/788694.sHtML
http://wap.baike.kmvdvi.cn/Article/details/03397.sHtML
http://wap.baike.kmvdvi.cn/Article/details/677394.sHtML
http://wap.baike.kmvdvi.cn/Article/details/7943.sHtML
http://wap.baike.kmvdvi.cn/Article/details/2517659.sHtML
http://wap.baike.kmvdvi.cn/Article/details/221610.sHtML
http://wap.baike.kmvdvi.cn/Article/details/07440.sHtML
http://wap.baike.kmvdvi.cn/Article/details/5408.sHtML
http://wap.baike.kmvdvi.cn/Article/details/97998.sHtML
http://wap.baike.kmvdvi.cn/Article/details/7352332.sHtML
http://wap.baike.kmvdvi.cn/Article/details/30045.sHtML
http://wap.baike.kmvdvi.cn/Article/details/1030894.sHtML
http://wap.baike.kmvdvi.cn/Article/details/63392.sHtML
http://wap.baike.kmvdvi.cn/Article/details/6521.sHtML
http://wap.baike.kmvdvi.cn/Article/details/823505.sHtML
http://wap.baike.kmvdvi.cn/Article/details/06363.sHtML
http://wap.baike.kmvdvi.cn/Article/details/0805.sHtML
http://wap.baike.kmvdvi.cn/Article/details/65713.sHtML
http://wap.baike.kmvdvi.cn/Article/details/5957967.sHtML
http://wap.baike.kmvdvi.cn/Article/details/7160.sHtML
http://wap.baike.kmvdvi.cn/Article/details/93211.sHtML
http://wap.baike.kmvdvi.cn/Article/details/262782.sHtML
http://wap.baike.kmvdvi.cn/Article/details/551170.sHtML
http://wap.baike.kmvdvi.cn/Article/details/9755811.sHtML
http://wap.baike.kmvdvi.cn/Article/details/721316.sHtML
http://wap.baike.kmvdvi.cn/Article/details/545802.sHtML
http://wap.baike.kmvdvi.cn/Article/details/0518331.sHtML
http://wap.baike.kmvdvi.cn/Article/details/37064.sHtML
http://wap.baike.kmvdvi.cn/Article/details/327172.sHtML
http://wap.baike.kmvdvi.cn/Article/details/3607.sHtML
http://wap.baike.kmvdvi.cn/Article/details/0748150.sHtML
http://wap.baike.kmvdvi.cn/Article/details/12438.sHtML
http://wap.baike.kmvdvi.cn/Article/details/6611622.sHtML
http://wap.baike.kmvdvi.cn/Article/details/2520261.sHtML
http://wap.baike.kmvdvi.cn/Article/details/1319.sHtML
http://wap.baike.kmvdvi.cn/Article/details/9763.sHtML
http://wap.baike.kmvdvi.cn/Article/details/927516.sHtML
http://wap.baike.kmvdvi.cn/Article/details/2315.sHtML
http://wap.baike.kmvdvi.cn/Article/details/7708337.sHtML
http://wap.baike.kmvdvi.cn/Article/details/3414421.sHtML
http://wap.baike.kmvdvi.cn/Article/details/9084862.sHtML
http://wap.baike.kmvdvi.cn/Article/details/54589.sHtML
http://wap.baike.kmvdvi.cn/Article/details/964403.sHtML
http://wap.baike.kmvdvi.cn/Article/details/84416.sHtML
http://wap.baike.kmvdvi.cn/Article/details/831423.sHtML
http://wap.baike.kmvdvi.cn/Article/details/884901.sHtML
http://wap.baike.kmvdvi.cn/Article/details/7241164.sHtML
http://wap.baike.kmvdvi.cn/Article/details/4572686.sHtML
http://wap.baike.kmvdvi.cn/Article/details/7979169.sHtML
http://wap.baike.kmvdvi.cn/Article/details/085156.sHtML
http://wap.baike.kmvdvi.cn/Article/details/857783.sHtML
http://wap.baike.kmvdvi.cn/Article/details/7878848.sHtML
http://wap.baike.kmvdvi.cn/Article/details/7689484.sHtML
http://wap.baike.kmvdvi.cn/Article/details/0556296.sHtML
http://wap.baike.kmvdvi.cn/Article/details/66596.sHtML
http://wap.baike.kmvdvi.cn/Article/details/98174.sHtML
http://wap.baike.kmvdvi.cn/Article/details/65665.sHtML
http://wap.baike.kmvdvi.cn/Article/details/5850314.sHtML
http://wap.baike.kmvdvi.cn/Article/details/9258.sHtML
http://wap.baike.kmvdvi.cn/Article/details/6414.sHtML
http://wap.baike.kmvdvi.cn/Article/details/24140.sHtML
http://wap.baike.kmvdvi.cn/Article/details/417702.sHtML
http://wap.baike.kmvdvi.cn/Article/details/7362.sHtML
http://wap.baike.kmvdvi.cn/Article/details/82615.sHtML
http://wap.baike.kmvdvi.cn/Article/details/65900.sHtML
http://wap.baike.kmvdvi.cn/Article/details/9882.sHtML

## 项目结构

```
wapwiki-link-aggregator/
├── cli.py                      # 命令行入口，注册所有子命令（import, serve, export, check）
├── requirements.txt            # 生产环境依赖列表（requests, click, jinja2）
├── setup.py                    # 项目打包与安装配置（setuptools 入口）
├── pyproject.toml              # 项目元数据与 black/isort 配置
├── .flake8                     # flake8 代码风格忽略规则配置
├── README.md                   # 项目说明文档（当前文件）
├── CHANGELOG.md                # 版本更新历史记录
├── LICENSE                     # MIT 许可证全文
├── samples/                    # 示例数据目录
│   └── links_batch_30.txt      # 第 30/56 批链接示例文件（纯文本列表）
├── src/                        # 核心源代码目录
│   ├── __init__.py             # 包初始化，导出主要 API 类
│   ├── importer.py             # 链接导入引擎（解析、去重、入库）
│   ├── checker.py              # HTTP 状态校验器（并发请求、超时重试）
│   ├── classifier.py           # 分类标签生成器（基于路径规则与关键词匹配）
│   ├── exporter.py             # 数据导出器（JSON/CSV/TXT 格式）
│   └── database.py             # SQLite 数据库适配器（建表、增删改查、事务管理）
├── web/                        # Web 管理界面模块
│   ├── app.py                  # Flask/FastAPI 应用工厂（路由注册与中间件）
│   ├── templates/              # Jinja2 HTML 模板目录
│   │   ├── index.html          # 首页（统计概览与最近导入列表）
│   │   ├── detail.html         # 单条链接详情页（元数据展示）
│   │   └── query.html          # 查询过滤页面（多条件组合筛选）
│   └── static/                 # 静态资源（CSS 样式表与 JavaScript 脚本）
│       ├── style.css           # 基础布局与响应式样式
│       └── app.js              # 前端交互逻辑（分页、过滤、批量操作）
├── tests/                      # 单元测试目录
│   ├── test_importer.py        # 导入引擎测试（含边界条件与异常处理）
│   ├── test_checker.py         # 校验器测试（模拟 HTTP 响应）
│   ├── test_classifier.py      # 分类器测试（覆盖各类路径模式）
│   └── test_database.py        # 数据库操作测试（事务回滚与并发安全）
├── docs/                       # 详细文档目录
│   ├── getting-started.md      # 入门指南（安装、配置、首次运行）
│   ├── cli-commands.md         # 命令行完整参考（子命令、参数、示例）
│   ├── data-model.md           # 数据模型定义（表结构、索引、字段约束）
│   ├── api-endpoints.md        # Web API 接口文档（请求/响应格式）
│   ├── deployment.md           # 生产部署指南（Gunicorn + Nginx + Supervisor）
│   └── batch-management.md     # 批次管理操作说明（切换、合并、归档）
├── scripts/                    # 辅助运维脚本
│   ├── init_db.sh              # 初始化数据库并创建默认表
│   ├── daily_scan.sh           # 每日增量扫描脚本（cron 调用）
│   └── export_latest.sh        # 导出最新批次链接为 Markdown 列表
└── logs/                       # 运行时日志目录（按天切割）
    ├── access.log              # Web 访问日志
    └── error.log               # 应用错误日志（含堆栈跟踪）
```

## 贡献指南

1. 在 GitHub 上 fork 本仓库，创建以 feature/ 或 fix/ 为前缀的分支，分支名称应简要描述改动内容，例如 feature/add-json-export-format。

2. 在本地开发环境中运行 make install-dev 安装所有开发依赖（包括 pytest、black、flake8），提交代码前执行 make lint 和 make test 确保代码风格通过检查且所有测试用例保持绿色。

3. 编写或更新与改动对应的单元测试，新增功能的测试覆盖率不低于 85%，并确保 docs/ 目录下的相关文档同步更新，尤其是 cli-commands.md 和 api-endpoints.md。

4. 提交 pull request 时请填写 PR 模板中的各项内容，包括改动动机、测试结果、文档更新情况，并关联相关 issue（若有）。PR 至少需要一位项目维护者审阅通过后方可合并。

5. 在 PR 合并前，请确保分支与主分支保持同步（rebase 或 merge），且提交历史清晰整洁（使用 git rebase -i 整理提交信息）。合并后，贡献者姓名将被列入 CONTRIBUTORS.md 文件。

## 常见问题

问：导入链接时提示 "duplicate entry"，但我确认这批链接之前未导入过，如何排查？

答：该提示通常因为数据库中的 url_hash 唯一索引冲突。请先执行 cli.py dedup --dry-run 检查是否有 URL 字符串大小写或尾部斜杠差异导致去重失败。若确定重复为误判，可使用 cli.py import --force 强制跳过唯一约束。另外，检查是否在同一批次文件中包含了重复行，可使用 sort -u 预处理文件。

问：Web 管理界面加载链接列表非常缓慢，如何优化查询性能？

答：当链接数量超过 5000 条时，建议先执行 cli.py index --rebuild 重建数据库索引，确保 status_code、category、batch_id 三列均有独立索引。同时在查询时尽量使用过滤条件（如指定 category 或 batch_id）而非全表扫描。若仍存在性能问题，可在 app.py 中启用分页查询，每页限制 50 条，并考虑使用数据库连接池。

问：导出的 JSON 文件中包含的 last_checked 字段为 null，这是什么原因？

答：last_checked 字段在链接导入时不会自动填充，仅当执行 cli.py check --update 命令后才会更新该字段为最近一次 HTTP 状态检查的时间戳。若希望导出时包含该信息，请先运行链接状态校验命令。此外，若 links 文件中的 URL 因网络超时未能完成检查，该字段也会保持为 null，可调整 checker.py 中的 timeout 参数值后重试。

## 许可证

MIT License

Copyright (c) 2026 WapWiki Link Aggregator Contributors

Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated documentation files (the "Software"), to deal in the Software without restriction, including without limitation the rights to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the Software, and to permit persons to whom the Software is furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.

> 外链数量: 180 | 生成时间: 2026-07-02 22:13:34
